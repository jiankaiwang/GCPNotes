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
	copy [-m] [-r] [...]                                    # -m: parallel
	mb [-c regional] [-l us-central1] <gs://${BUCKET_NAME}> # create buckets
	cp <gs://from> <local_path>
	notification
		list
		create [-t upload_notification]                       # create a notification to pub/sub
		       [-f json] [-e OBJECT_FINALIZE]                 # -t: type, -f: format, -e: event
		       gs://${IV_BUCKET_NAME}
```

