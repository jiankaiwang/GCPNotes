# Dataproc: Qwik Start - Console (GSP103)



Cloud Dataproc is a fast, easy-to-use, fully-managed cloud service for running Apache Spark, Apache Hadoop clusters. It is convenient for customers to create a Google Cloud Dataproc cluster, resize or modify the number of workers in it.



keywords: `Dataproc`, `Spark`



## Reference

* Qwiklabs: <https://google.qwiklabs.com/focuses/586?parent=catalog>
* Submit a custom task example: https://cloud.google.com/dataproc/docs/guides/submit-job



## QuickNote



### Confirm Cloud Dataproc API is enabled

Steps:

*   Click **Navigation menu** > **APIs & Services** > **Library**
*   Type `Cloud Dataproc` on the search bar.
*   Click the button `Enable` if the service is not enabled.



### Create a cluster

Steps:

*   Click **Navigation menu** > **Dataproc** > **Cluster**, and click `Create cluster`.

*   Set the following fields with their values.

    | Field  | Value           |
    | ------ | --------------- |
    | Name   | example-cluster |
    | Region | global          |
    | Zone   | us-central1-a   |

*   Click the `Create`.



### Submit a job

Steps:

*   The following is the demo showing how to run a sample Spark job.

*   Click `Jobs` on the left panel and click `Submit job`. 

*   Set the following fields to update Job.

    | Field             | Value                                                  |
    | ----------------- | ------------------------------------------------------ |
    | Cluster           | example-cluster                                        |
    | Job type          | Spark                                                  |
    | Main class or jar | org.apache.spark.examples.SparkPi                      |
    | Arguments         | 1000 (This sets the number of tasks.)                  |
    | Jar file          | file:///usr/lib/spark/examples/jars/spark-examples.jar |

*   Click `Submit`.



### View the job output

You should see the output like `Pi is roughly 3.1416735514167353` or `Pi is roughly 3.1416830714168307` in the Output window.



#### Update a cluster

The following shows how to change the number of worker instances in the cluster.

Steps:

*   Click `Clusters` in the left panel.
*   Click `example-cluster` in the Clusters list.
*   Click `Configuration` to display the cluster's current settings.
*   Click `Edit`.
*   Enter an integer number on the item **Worker nodes**.

*   Click `Save`.
*   Click `VM Instances` to check out the number of VM instances in the cluster.



















