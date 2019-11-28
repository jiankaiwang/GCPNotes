# Predict Housing Prices with Tensorflow and AI Platform (GSP418)



keywords: `Datalab`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/3644?parent=catalog
* Datalab on GCP Tutorial: https://cloud.google.com/datalab/docs/quickstart
* Repository: https://github.com/vijaykyr/tensorflow_teaching_examples.git



## Quick Note

The following script is an example for creating a datalab. After created a datalab entity, you can start the datalab via port 8081.

```sh
gcloud config set core/project PROJECT_ID
gcloud config set compute/zone us-central1-f

# here we create a bucket for code packages
gsutil mb -c multi_regional -l us gs://[bucket_name]

datalab create my-datalab --machine-type n1-standard-4
```

You can create a notebook and execute a script from it.

