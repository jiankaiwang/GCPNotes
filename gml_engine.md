# Google ML Engine API



## Reference



## Quick Notes

The below is example for you understanding the following commands.

```sh
export MODEL_DIR=output
export TRAIN_DATA=$(pwd)/data/data.csv
export EVAL_DATA=$(pwd)/data/eval.csv
export TEST_DATA==$(pwd)/data/test.csv
export JOB_NAME=census_single_1
export OUTPUT_PATH=gs://$BUCKET_NAME/$JOB_NAME
export MODEL_BINARIES=$OUTPUT_PATH/export/census/1559274982/
export MODEL_NAME=census
export REGION=us-central1-a
```

Quick api reference used the environment variables in some cases.

```sh
gcloud 
	ai-platform 
		local 
			train \  # run a local training task
        --module-name trainer.task \  # main entry
        --package-path trainer/ \  # the training package
        --job-dir $MODEL_DIR \
        --train-files $TRAIN_DATA \
        --eval-files $EVAL_DATA \
        --train-steps 1000 \
        --eval-steps 100 \
        --verbosity DEBUG
			predict \  # run a local prediction task
				--model-dir output/export/census/1559272937 \
				--json-instances $TEST_DATA  # the test data
		jobs 
			submit 
				training $JOB_NAME \ # submit a training job to the ml engine
    			--job-dir $OUTPUT_PATH \
          --runtime-version 1.10 \
          --module-name trainer.task \
          --package-path trainer/ \
          --region $REGION \
          --train-files $TRAIN_DATA \
          --eval-files $EVAL_DATA \
          --train-steps 1000 \
          --eval-steps 100 \
          --verbosity DEBUG
       	stream-logs $JOB_NAME
		models 
			create $MODEL_NAME --regions=$REGION
			list
		versions 
			create v1 \  # v1 is the version control you can create
        --model $MODEL_NAME \
        --origin $MODEL_BINARIES \
        --runtime-version 1.10
    predict \
			--model $MODEL_NAME \
			--version v1 \  # selected the api ready to use
			--json-instances $TEST_DATA
```

