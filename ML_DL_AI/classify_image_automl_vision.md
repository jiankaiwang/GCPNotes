# Classify Images of Clouds in the Cloud with AutoML Vision (GSP223)



keywords: `AutoML`, `Training`, `Inference`, `Deployment`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/8406?parent=catalog



## Quick Notes



### Setup AutoML Vision

* Enable `Cloud AutoML API`. 

* Surf the page AutoML(https://cloud.google.com/automl/ui/vision) and assign the Project ID to continue. 

* Click `SET UP Noe` to enable the required APIs.

* Set up IAM to give AutoML permissions.

```sh
export PROJECT_ID=$DEVSHELL_PROJECT_ID
export QWIKLABS_USERNAME=<USERNAME|Email>

# add IAM
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="user:$QWIKLABS_USERNAME" \
    --role="roles/automl.admin"
    
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="serviceAccount:custom-vision@appspot.gserviceaccount.com" \
    --role="roles/ml.admin"
    
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="serviceAccount:custom-vision@appspot.gserviceaccount.com" \
    --role="roles/storage.admin"
```



### Upload training images to Google Cloud Storage

In order to use AutoML, you have to put images on the cloud storage bucket.

```sh
export BUCKET=$PROJECT_ID-vcm
gsutil -m cp -r gs://automl-codelab-clouds/* gs://${BUCKET}
```



### Create a dataset

Now you have to provide AutoML Vision a csv file which contains images' URL and their labels.

```sh
# fetch the dataset metadata
gsutil cp gs://automl-codelab-metadata/data.csv .

# update the content
sed -i -e "s/placeholder/${BUCKET}/g" ./data.csv

# upload to cloud storage bucket
gsutil cp ./data.csv gs://${BUCKET}
```

Now back to `AutoML Vision` page. Click `+ NEW DATASET` and select `Single-Label Classification`. Click `Create dataset`.

In `Select files to import`, select `Select a CSV file on Cloud Storage` and click `BROWSE` to select the `data.csv` just uploaded.



### Inspect images

* Click `IMAGES` to view the uploaded images.
* Click `LABEL STATS` to see the summary.



### Train your model

* Click `TRAIN`. AutoML would auto split the dataset into train, validation and test subdatasets.
* Click `START TRAINING`.

* Leave `Cloud-hosted` selected, then click `Continue`.
* Set a node hour budget be set to `1`.



### Evaluate your model

* In the `Evaluate` tab, there are several metrics, like `Avg precision`, `Precision`, `Recall`, `Confusion matrix`, etc.



### Generate Prediction

*  In the `Test & Use` tab, you can create predictions based on the model you trained.
*  Click `Deploy` the mode and enter the number of node you are going to use for deploying.
*  After deploying the model, you can `UPLOAD IMAGES` to generate predictions.

