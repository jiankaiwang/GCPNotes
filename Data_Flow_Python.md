# Dataflow: Qwik Start - Python (GSP207)



## Quick Note

Apache Beam: <https://github.com/apache/beam>



## Start Cloud Shell





## Create a Cloud Storage bucket


### Create a bucket


Create a storage bucket called `qwik-bucket-jkw`.

### Install pip and cloud dataflow sdk

Update pip and virtualen, create a virtual env under python2.7 and activate the virtualenv.

```sh
python --version
pip --version
sudo pip install -U pip
sudo pip install --upgrade virtualenv
virtualenv -p python2.7 env
source env/bin/activate
```

Inside virtualenv and install the latest version of Apache Beam.

```sh
pip install apache-beam[gcp]
```

### Run the `wordcount.py` example locally.

```sh
python -m apache_beam.examples.wordcount --output OUTPUT_FILE
```

**Cloud Dataflow is a distribution of Apache Beam.**

### Run an Example Pipeline Remotely

Set the `BUCKET` environment variable to the bucket.

```sh
BUCKET=gs://qwik-bucket-jkw
```

Run the `wordcount.py` example remotely.

```sh
python -m apache_beam.examples.wordcount --project $DEVSHELL_PROJECT_ID \
  --runner DataflowRunner \
  --staging_location $BUCKET/staging \
  --temp_location $BUCKET/temp \
  --output $BUCKET/results/output
```

**Dataflow temp_location must be a valid Google Cloud Storage URL.**



## Check that your job succeeded

Steps:

*   Open the Cloud Dataflow.
*   Watch the `Status` to check the status of the current job.

*   Click `Storage` in the console.
*   Click the created bucket and inspect the results.









