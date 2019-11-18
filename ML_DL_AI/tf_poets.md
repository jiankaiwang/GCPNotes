# TensorFlow for Poets (GSP077)

In this tutorial, use transfer learning to speed up the training time. Transfer learning is like transferring knowledge, using the weights already trained and adding several new layers for training. Transfer learning is a popular technique for similar tasks. Here we use transfer learning to retrain a model for classifying flowers. 



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1095?parent=catalog
* Used Git Repository: https://github.com/googlecodelabs/tensorflow-for-poets-2
* Used Datasets:
  * Flowers: http://download.tensorflow.org/example_images/flower_photos.tgz



## Quick Notes



### About the Data Preparation

The recommanded data structure as below. 

```text
+ tf_files/
	+ flower_photos/    # dataset name
		+ flower1/        # category 1
			- f1-1.jpg      # image 1
			- f1-2.jpg      # image 2
			- ...
		+ flower2/        # category 2
			- f2-1.jpg
			- f2-2.jpg
			- ...
		+ flower3/        # category 3
		+ flower4/        # category 4
		+ ...
```



### About the model architecture

The model architecture is the key to transfer learning. Here you can simply change the model architecture by changing the parameter `--architecture` value.

```sh
export IMAGE_SIZE=224
export ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}"
python -m scripts.retrain \
  --bottleneck_dir=tf_files/bottlenecks \
  --how_many_training_steps=500 \
  --model_dir=tf_files/models/ \
  --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
  --output_graph=tf_files/retrained_graph.pb \
  --output_labels=tf_files/retrained_labels.txt \
  --architecture="${ARCHITECTURE}" \
  --image_dir=tf_files/flower_photos
```

You can also find the pre-trained model on https://tfhub.dev/. It is a collection for different model architectures and is released by Google.



### About the inference

The script is a good example of demonstrating the model inference. However, it is not the best practice due to its inference mode. Each time, an image was ready for inference, it requires the whole process from loading a model into the memory and activating a new session , to release the resource used in the inference process.

```sh
python -m scripts.label_image \
    --graph=tf_files/retrained_graph.pb  \
    --image=tf_files/flower_photos/daisy/21652746_cc379e0eea_m.jpg
```



















