# Machine Learning with Tensorflow (GSP273)



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/3391?parent=catalog



## Quick Notes



### Environment Setting

Create a VM instance via clicking `Navigation menu` > `Compute Engine` > `VM instances`. Login the machine by clicking `SSH` at the end of line.

```sh
# update repo list
sudo apt -y update
sudo apt -y upgrade

# install git toolkit
sudo apt -y install git

# glone a repo from github, the source code would be used in this tutorial
git clone  https://github.com/GoogleCloudPlatform/data-science-on-gcp/

# install python environment
sudo apt-get install -y python-pip python-dev python3-pip python3-dev virtualenv

# create a virtual environment for tensorflow
sudo pip install virtualenv
virtualenv -p python3 venv
source venv/bin/activate

# install tensorflow
pip install tensorflow==1.14
```



### Create minimal training and test datasets

Here we use the source data files which is developed by processing the raw flight data from the Bureau of Transport Statistics website.

```sh
# create a folder for the training and testing datasets
mkdir -p ~/data/flights

# define local environment variables
export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET=${PROJECT_ID}

# extract a part of training data (head 10003 rows) into a sub dataset
gsutil cp \
  gs://${BUCKET}/flights/chapter8/output/trainFlights-00001*.csv \
  full.csv
head -10003 full.csv > ~/data/flights/train.csv
rm -f full.csv

# extract a part of testing data (head 10003 rows) into a sub dataset
gsutil cp \
	gs://${BUCKET}/flights/chapter8/output/testFlights-00001*.csv \
	full.csv
head -10003 full.csv > ~/data/flights/test.csv
rm -f full.csv
```



### Create a TensorFlow experimental framework in Python

In the end of the tutorial, we are going to deploy the trained model to the Google Cloud Platform. In order to do that, the deployment must be structued as a Python module, not just a simple script.

```sh
# create a suggested directory structure
mkdir -p ~/tensorflow/flights/trainer
cd ~/tensorflow/flights/trainer

# create a __init__.py as the module definition
touch __init__.py

# create and edit a script named model.py as the core script
vim model.py
```

Add the following content to the `model.py`.

```python
import tensorflow as tf

CSV_COLUMNS  = \
('ontime,dep_delay,taxiout,distance,avg_dep_delay,avg_arr_delay,' + \
 'carrier,dep_lat,dep_lon,arr_lat,arr_lon,origin,dest').split(',')
LABEL_COLUMN = 'ontime'
DEFAULTS     = [[0.0],[0.0],[0.0],[0.0],[0.0],[0.0],\
                ['na'],[0.0],[0.0],[0.0],[0.0],['na'],['na']]

def read_dataset(filename, mode=tf.contrib.learn.ModeKeys.EVAL,
                 batch_size=512, num_training_epochs=10):
      # This is double indented to make a later edit simpler
      if mode == tf.contrib.learn.ModeKeys.TRAIN:
         num_epochs = num_training_epochs
      else:
         num_epochs = 1
      # could be a path to one file or a file pattern.
      input_file_names = tf.train.match_filenames_once(filename)
      filename_queue = tf.train.string_input_producer(
          input_file_names, num_epochs=num_epochs, shuffle=True)
      # Read in and parse the CSV
      reader = tf.TextLineReader()
      _, value = reader.read_up_to(
          filename_queue, num_records=batch_size)
      value_column = tf.expand_dims(value, -1)
      columns = tf.decode_csv(value_column, record_defaults=DEFAULTS)
      features = dict(zip(CSV_COLUMNS, columns))
      label = features.pop(LABEL_COLUMN)
      return features, label
```

Create and edit a new file named `task.py`. Add the following content. This script is prepared for executing model function.

```python
import argparse
import model
# import trainer.model as model
import tensorflow as tf

if __name__ == '__main__':
  parser = argparse.ArgumentParser()
  parser.add_argument(
      '--traindata',
      help='Training data file(s)',
      required=True
  )
  # parse args
  args = parser.parse_args()
  arguments = args.__dict__
  traindata = arguments.pop('traindata')
  
# Call read_dataset from model.py
feats, label = model.read_dataset(traindata)
# Find the average of all the labels that were read in
avg = tf.reduce_mean(label)
print(avg)
```

Let's run the script to exam the above parts.

```sh
python task.py --traindata ~/data/flights/train.csv
```



### Editing for Deployment

Edit `model.py` to add functions for deployment.

```python
# import the packages and add these scripts on the top of file
import tensorflow.estimator as tflearn
import tensorflow.contrib.layers as tflayers
import tensorflow.contrib.metrics as tfmetrics
import numpy as np
```

Add features extraction action on the end of the `model.py`.

```python
def get_features():
    # Using three basic inputs
    real = {
      colname : tflayers.real_valued_column(colname) \
          for colname in \
            ('dep_delay,taxiout,distance').split(',')
    }
    sparse = {}
    return real, sparse
```

Add a linear classifier estimator on the end of the `model.py`.

```python
def linear_model(output_dir):
    real, sparse = get_features()
    all = {}
    all.update(real)
    all.update(sparse)
    estimator = tflearn.LinearClassifier(model_dir=output_dir, feature_columns=all.values())
    return estimator
```

You need to modify the function `read_dataset` to define a callback function as its return value. This is required for the training and evaluation specification parameters that will be used by the Tensorflow `train_and_eval` library function.

```python
def read_dataset(filename, mode=tf.contrib.learn.ModeKeys.EVAL,
                 batch_size=512, num_training_epochs=10):
  # the actual input function would be passed to Tensorflow
  def _input_fn():
      # This is double indented to make a later edit simpler
      if mode == tf.contrib.learn.ModeKeys.TRAIN:
			#...
      return features, label

  # return input function callback
  return _input_fn()  
```

When a model is used for prediciton, it is reuqired to receive inputs via REST calls. A serving input can handle JSON format requests is necessary. You need to define a `serving_input_fn`. Add the following content on the end of `model.py`.

```python
def serving_input_fn():
    real, sparse = get_features()

    feature_placeholders = {
      key : tf.placeholder(tf.float32, [None]) for key in real.keys()
    }
    feature_placeholders.update( {
      key : tf.placeholder(tf.string, [None]) for key in sparse.keys()
    } )

    features = {
      # tf.expand_dims will insert a dimension 1 into tensor shape
      # This will make the input tensor a batch of 1
      key: tf.expand_dims(tensor, -1)
         for key, tensor in feature_placeholders.items()
    }
    return tf.estimator.export.ServingInputReceiver(
      features,
      feature_placeholders)
```

Next, you can define a `run_experiment` function to bring all components together. This funtion defines training and evaluation input and uses estimator to specify training, evaluation processes. Add the following content on the end of `model.py`.

```python
def run_experiment(traindata,evaldata,output_dir):
  train_input = read_dataset(traindata, mode=tf.contrib.learn.ModeKeys.TRAIN)
  # Don't shuffle evaluation data
  eval_input = read_dataset(evaldata)
  train_spec = tf.estimator.TrainSpec(train_input, max_steps=1000)
  eval_spec  = tf.estimator.EvalSpec(eval_input)
  run_config = tf.estimator.RunConfig()
  run_config = run_config.replace(model_dir=output_dir)
  print('model dir {}'.format(run_config.model_dir))
  estimator = linear_model(output_dir)

  tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
```

You now can add now operations on `task.py` under `--traindata`.

```python
  parser.add_argument(
      '--evaldata',
      help='Training data can have wildcards',
      required=True
  )
  parser.add_argument(
      '--output_dir',
      help='Output directory',
      required=True
  )
  parser.add_argument(
      '--job-dir',
      help='required by gcloud',
      default='./junk'
  )
```

Add more handling to the script under `arguments.pop('traindata')`.

```python
  evaldata =  arguments.pop('evaldata')
  output_dir = arguments.pop('output_dir')
```

Replace `print(avg)` with the following script.

```python
# Call read_dataset from model.py
#feats, label = model.read_dataset(traindata)
# Find the average of all the labels that were read in
#avg = tf.reduce_mean(label)

tf.logging.set_verbosity(tf.logging.INFO)
model.run_experiment(traindata,evaldata,output_dir)
```

Now you can run the `task.py` via the command.

```sh
python task.py \
       --traindata ~/data/flights/train.csv \
       --output_dir ./trained_model \
       --evaldata ~/data/flights/test.csv
```

In order to deploy the script to Cloud ML, you have to build them as a module framework. It requires `PKG-INFO`, `setup.cfg` and `setup.py` under the base folder of the module structure.

```sh
pushd ~/data-science-on-gcp/09_cloudml/flights/
cp PKG-INFO ~/tensorflow/flights
cp setup.cfg ~/tensorflow/flights
cp setup.py ~/tensorflow/flights
popd
```

The currenrt path would be like below.

```text
+ tensorlfow
  + flights
    - PKG-INFO
    - setup.cfg
    - setup.py
    + trainer
      - __init__.py
      - model.py
      - task.py
      + trained_model
```

Edit the `setup.py` to ensure the module dependency on Tensorflow is explicitly stated.

```sh
vim ~/tensorflow/flight/setup.py
```

Insert the following line between the `REQUIRED_PACKAGES = [` and `]`.

```python
'tensorflow>=1.7'
```

Update the `PYTHONPATH` environment variable to allow you to call your trainer module.

```sh
export PYTHONPATH=${PYTHONPATH}:~/tensorflow/flights
```

Test executing the trainer task.

```sh
cd ~/tensorflow
export DATA_DIR=~/data/flights
python -m trainer.task \
  --output_dir=./trained_model \
  --traindata $DATA_DIR/train* --evaldata $DATA_DIR/test*
```

If you encounter the problem showing `ImportError: No module named 'model'`. Modify the `task.py`.

```python
# import model
import trainer.model as model
```

You can also add other metrics for compare. The following is an example adding `RSME`. Edit `model.py`.

```sh
vim ~/tensorflow/flights/trainer/model.py
```

```python
def my_rmse(labels,predictions):
    predicted_classes = predictions['probabilities'][:,1]
    custom_metric = tf.metrics.root_mean_squared_error(labels, predicted_classes, name="rmse")
    return {'rmse':custom_metric}
```

Put a new metrics to the estimator, edit `def linear_model`.

```python
def linear_model(output_dir):
    real, sparse = get_features()
    all = {}
    all.update(real)
    all.update(sparse)
    estimator = tflearn.LinearClassifier(model_dir=output_dir, feature_columns=all.values())
    
    # add the new metrics
    estimator = tf.contrib.estimator.add_metrics(estimator, my_rmse)
    return estimator
```

Now you can test the trainer task.

```sh
cd ~/tensorflow
export DATA_DIR=~/data/flights
python -m trainer.task \
  --output_dir=./trained_model \
  --traindata $DATA_DIR/train* --evaldata $DATA_DIR/test*
```

You would see the result like below.

```text
INFO:tensorflow:Saving dict for global step 400: accuracy = 0.90082973, accuracy_baseline = 0.72878134, auc = 0.8935344, auc_precision_recall = 0.9460858, average_loss = 0.43320456, global_step = 400, label/mean = 0.72878134, loss = 216.66727, precision = 0.89921397, prediction/mean = 0.7734606, recall = 0.9729767, rmse = 0.29041532
```

Now the bundle of scripts are prepared for Cloud ML.



### Deploy the TensorFlow experimental framework to the Google Cloud ML Engine

First set environment variables to use Cloud Storage buckets for the source and output locations for the model and result.

```sh
export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET=$PROJECT_ID
export REGION=us-central1
export OUTPUT_DIR=gs://${BUCKET}/flights/chapter9/output
export DATA_DIR=gs://${BUCKET}/flights/chapter8/output
```

Create a jobname and change the working directory.

```sh
export JOBNAME=flights_$(date -u +%y%m%d_%H%M%S)
cd ~/tensorflow
```

Submit to AI platform on Google Cloud.

```sh
gcloud ai-platform jobs submit training $JOBNAME \
  --module-name=trainer.task \
  --package-path=$(pwd)/flights/trainer \
  --job-dir=$OUTPUT_DIR \
  --staging-bucket=gs://$BUCKET \
  --region=$REGION \
  --scale-tier=STANDARD_1 \
  --python-version=3.5 \
  --runtime-version=1.14 \
  -- \
  --output_dir=$OUTPUT_DIR \
  --traindata $DATA_DIR/train* --evaldata $DATA_DIR/test*
```

Now you can see the job information via `Navigation menu` > `AI Platform`.



