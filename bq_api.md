# BigQuery API Document



Official document link: <https://cloud.google.com/bigquery/docs/reference/bq-cli-reference>



## Quick Note



*   create a dataset

```sh
bq mk <ds-name>
```



*   create a table with its schema

The definition is in the form like `[FIELD]:[DATA_TYPE],[FIELD]:[DATA_TYPE]`.

```sh
bq mk \
--time_partitioning_field timestamp \
--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
passenger_count:integer -t taxirides.realtime
```











