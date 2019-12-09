#ETL Processing on GCP Using Dataflow and BigQuery (GSP290)

keywords: `ETL`, `Dataflow`, `BigQuery`

## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/3460?parent=catalog
* Git Repository: https://github.com/GoogleCloudPlatform/professional-services.git



## Quick Note

Copy datasets from a storage bucket to porject's one.

```sh
gsutil cp gs://python-dataflow-example/data_files/usa_names.csv gs://$PROJECT/data_files/
gsutil cp gs://python-dataflow-example/data_files/head_usa_names.csv gs://$PROJECT/data_files/
```

Create a virtual envorinment running apache-beam with GCP toolkit supported.

```sh
git clone https://github.com/GoogleCloudPlatform/professional-services.git
cd professional-services/examples/dataflow-python-examples/
# Here we set up the python environment.
# Pip is a tool, similar to maven in the java world
sudo pip install --upgrade virtualenv

#Dataflow requires python 2.7
virtualenv -p `which python 2.7` dataflow-env

source dataflow-env/bin/activate
pip install apache-beam[gcp]
```

Data is ingested on `Cloud Storage` to `BigQuery`.

```sh
python dataflow_python_examples/data_ingestion.py \
	--project=$PROJECT \
	--runner=DataflowRunner \
	--staging_location=gs://$PROJECT/test \
	--temp_location gs://$PROJECT/test \
	--input gs://$PROJECT/data_files/head_usa_names.csv \
	--save_main_session
```

Transform data.

```sh
python dataflow_python_examples/data_transformation.py \
	--project=$PROJECT \
	--runner=DataflowRunner \
	--staging_location=gs://$PROJECT/test \
	--temp_location gs://$PROJECT/test \
	--input gs://$PROJECT/data_files/head_usa_names.csv \
	--save_main_session
```

Enrich data stored in Cloud BigQuery.

```sh
python dataflow_python_examples/data_enrichment.py \
	--project=$PROJECT \
	--runner=DataflowRunner \
	--staging_location=gs://$PROJECT/test \
	--temp_location gs://$PROJECT/test \
	--input gs://$PROJECT/data_files/head_usa_names.csv \
	--save_main_session
```

Data lake to Mart.

```sh
python dataflow_python_examples/data_lake_to_mart.py \
	--worker_disk_type="compute.googleapis.com/projects//zones//diskTypes/pd-ssd" \
	--max_num_workers=4 \
	--project=$PROJECT \
	--runner=DataflowRunner \
	--staging_location=gs://$PROJECT/test \
	--temp_location gs://$PROJECT/test \
	--save_main_session
```

Data lake to Mart CoGroupByKey.

```sh
python dataflow_python_examples/data_lake_to_mart_cogroupbykey.py \
	--worker_disk_type="compute.googleapis.com/projects//zones//diskTypes/pd-ssd" \
	--max_num_workers=4 \
	--project=$PROJECT \
	--runner=DataflowRunner \
	--staging_location=gs://$PROJECT/test \
	--temp_location gs://$PROJECT/test \
	--save_main_session
```

