# Run a Big Data Text Processing Pipeline in Cloud Dataflow (GSP047)



Google Cloud Dataflow is a tool for a wide range of data types. It supports not only structured or semi-structured data but also unstructured data. It supports both batch or streaming as well. But unlike Dataprep, you have to write your own script to process the data, including fetching data from the source and saving the processed data to the destination. The pros and cons of Dataflow are in the same step, highly flexible to the data, but accompanying with heavily struggling jobs to the work.



keywords: `Dataflow`, `Java`, `Maven`



## Reference

* Qwiklabs: <https://www.qwiklabs.com/focuses/608?parent=catalog>
* Examples: <https://github.com/GoogleCloudPlatform/DataflowSDK-examples>



## Creating a Maven Project

In the cloud shell, type the following command to create a project for Cloud Dataflow SDK for java and example pipelines.

```sh
mvn archetype:generate \
      -DarchetypeGroupId=org.apache.beam \
      -DarchetypeArtifactId=beam-sdks-java-maven-archetypes-examples \
      -DarchetypeVersion=2.8.0 \
      -DgroupId=org.example \
      -DartifactId=first-dataflow \
      -Dversion="0.1" \
      -Dpackage=org.apache.beam.examples \
      -DinteractiveMode=false
```



## Run the Text Processing Pipeline on Cloud Dataflow

In the cloud shell, run an example `WordCount` on the Cloud Dataflow.

```sh
export PROJECT_ID=<your-project-id>
export BUCKET_NAME=<your-backet-name>

mvn -Pdataflow-runner compile exec:java \
      -Dexec.mainClass=org.apache.beam.examples.WordCount \
      -Dexec.args="--project=${PROJECT_ID} \
      --stagingLocation=gs://${BUCKET_NAME}/staging/ \
      --output=gs://${BUCKET_NAME}/output \
      --runner=DataflowRunner"
```



