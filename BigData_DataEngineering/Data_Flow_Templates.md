# Dataflow: Qwik Start - Templates (GSP192)



keywords: `Dataflow`, `BigQuery`, `Storage`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1101?parent=catalog



## QuickNote



## Create a Cloud BigQuery Dataset and Table Using Cloud Shell

First thing is to create a BigQuery dataset and table.

```sh
bq mk taxirides
```

Create a table and define its schema to the dataset. The definition is in the form like `[FIELD]:[DATA_TYPE],[FIELD]:[DATA_TYPE]`.

```sh
bq mk \
--time_partitioning_field timestamp \
--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
passenger_count:integer -t taxirides.realtime
```

Create a storage bucket

```sh
export BUCKET_NAME=qwik_jkw_bucket
gsutil mb gs://$BUCKET_NAME/
```



## Create a Cloud BigQuery Dataset and Table Using the GCP Console



Here we use a pre-defined template to start our new job.



## Run the Pipeline

Steps:

*   From the`Navigation menu` find Big Data section and click `Dataflow` service.
*   Click `+ CREATE JOB FROM TEMPLATE` on the top of the window.
*   Tyope a job name for the Dataflow job, e.g. `qwik-dataflow-jkw`.
*   Under `Cloud Dataflow temnplate` select `Cloud Pub/Sub Topic to BigQuery`.
*   Under `Cloud Pub/Sub input topic`, type `projects/pubsub-public-data/topics/taxirides-realtime`.
*   Under `BigQuery output table`, type `<myprojectid>:taxirides.realtime`, e.g. `qwiklabs-gcp-5944dcec93ab1113:taxirides.realtime`.
*   Under `Temporary location`, type `gs://Your_Bucket_Name/temp`, e.g. `gs://qwik_jkw_bucket/temp`.
*   Click `RUN`.



![](https://cdn.qwiklabs.com/kJiAXaVD5HH1IYxC0CWj%2BmR%2B9CqSFNJm3aDa%2Fm%2FEIco%3D)



## Submit a query

Now you can submit queries using standard SQL.

```sh
SELECT * FROM `qwiklabs-gcp-5944dcec93ab1113.taxirides.realtime` LIMIT 1000
```



















