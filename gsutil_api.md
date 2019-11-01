# Google Storage API (gsutil)



## Reference



* <https://cloud.google.com/storage/docs/gsutil>



## Quick Note

```sh
export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET_NAME=${PROJECT_ID}-upload
```

```sh
gsutil
	ls [gs://<url>/path/]
	cat [gs://<url>/path/]
	copy [-m] [-r] [...]  # -m: parallel
	mb [-c regional] [-l us-central1] <gs://${BUCKET_NAME}>
	cp <gs://from> <local_path>
	notification 
		list
		# -t: type, -f: format, -e: event
		create [-t upload_notification] [-f json] [-e OBJECT_FINALIZE] gs://${IV_BUCKET_NAME}
```

