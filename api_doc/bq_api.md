# BigQuery API Document



Official document link: <https://cloud.google.com/bigquery/docs/reference/bq-cli-reference>



## Quick Note

```sh
export DATASET_ID=intelligentcontentfilter
export TABLE_NAME=filtered_content
```

```sh
bq [--project_id ${PROJECT_ID}]                      # project_id is required
  mk [--time_partitioning_field timestamp] \         # create a dataset
     [--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
  timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
  passenger_count:integer] \
    [--schema intelligent_content_bq_schema.json] \
    [--table ${DATASET_ID}.${TABLE_NAME}]
  show <${DATASET_ID}.${TABLE_NAME}>
  load [--source_format=CSV]                         # load csv file into bigquery table
       [--skip_leading_rows=1] 
       dbname.tablename
       gs://bucket_id/file.csv 
       <colname>:string,<colname2>:string,<colname3>:string
  query [--use_legacy_sql=false] 
        [--destination_table=dbname.tablename] 
        "$QUERY \`$TABLE\` $SUBSAMPLE"
```

P.S.

* The definition is in the form like `[FIELD]:[DATA_TYPE],[FIELD]:[DATA_TYPE]`.



## Reinforced SQL Note

* BQML Model

Create or replace a linear regression model.

```sql
CREATE or REPLACE MODEL taxi.taxifare_model
OPTIONS
  (model_type='linear_reg', labels=['total_fare']) AS
-- paste the training dataset query here
```

Create or replace a logistic regression model.

```sql
CREATE OR REPLACE MODEL `ecommerce.classification_model`
OPTIONS
(model_type='logistic_reg', labels = ['will_buy_on_return_visit']) AS
-- paste the training dataset query here
```

Evaluate the model.

```sql
SELECT
  roc_auc,
  CASE
    WHEN roc_auc > .9 THEN 'good'
    WHEN roc_auc > .8 THEN 'fair'
    WHEN roc_auc > .7 THEN 'decent'
    WHEN roc_auc > .6 THEN 'not great'
  ELSE 'poor' END AS model_quality
FROM
  ML.EVALUATE(MODEL ecommerce.classification_model, ... );
```

Predict the result via the trained model.

```sql
SELECT
  *
FROM
  ml.PREDICT(MODEL `ecommerce.classification_model_2`, ... );
```













