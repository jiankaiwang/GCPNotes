# A Tour of Qwiklabs and the Google Cloud Platform (GSP282)



### Quick Notes

*   Service Categories: https://cloud.google.com/docs/overview/cloud-platform-services#top_of_page
*   Role documents: <https://cloud.google.com/iam/docs/understanding-roles#primitive_roles>
*   Google API Design Guide: <https://cloud.google.com/apis/design/>



## GCP Brief

* GCP is a suite of cloud services hosted on Google's infrastructure.

* The entity operating GCP is `PROJECT`. It is the collection of resources, for example, a set of databases, a cluster of containers, a set of the network used by the compute instances, etc. The `PROJECT_ID` is identifiable across the whole GCP. Resources and APIs are specific to `PROJECT_ID`, including permissions, etc.



## Service Categories

There are seven categories of services provided by Google Cloud Platform.

### Compute 
* VM instances
* K8s engines: for container clusters
* App engines
* Serverless Cloud functions: handle responses from other services, like pub/sub, etc
* Serverless Cloud runs: no state HTTP container, in other words, merge serverless services with container tech.

### Storage
storing in blob or databases for structured, semi-structured, non-structured or for relational, non-relational data
* Bigtable: a PB scale and column-wide NoSQL, especial for large-scaled analyzing or operating 
* Filestore(newer) /w Datastore Mode: A highly scalable NoSQL database but supports SQL-like queries. It is especial for mobile-scaled services.
* Storage: traditional files or blobs accesses, uploads, deletes, or downloads
* SQL: MySQL, or PostgreSQL like traditional relational databases
* Spanner: enterprise-grade SQL databases merging NoSQL advantages like scalability or expending, etc.
* Memorystore: cache-based NoSQL databases, the core is Redis

### Networking
balance the service traffic and provision security rules
* VPC Network: provide direct communication among services in a virtual private cloud networking
* Network Service: DNS, CDN, NAT, etc.
* Hybrid Connection: VPN, Cloud Router, Interconnector, etc.
* Internet Service Provider: Optimize cloud services, including load balance, etc.
* Internet Security: SSL, etc.

### Stackdriver

a suite of cloud monitoring, tracking logging and service reliability tools

* Monitoring: Monitor app, network, engine, service log events.
* Debugger: Debug your source code, you have to import GCP SDK first.
* Trace: Compare to Monitoring, Trace focuses on tracking user's events on APP, Compute engines. (like URL tracking, web traffic, etc.)
* Log Record
* Error Reporting: Supervise the services hosted on GCP. If the services or engines go error, it would report the influence details, like how many people are influenced, how much time the services are out of work, etc.
* Profiler: A suite of tools can help developers do the statistics about service performance from the scope of source code.

### Tools

A suite of services helps developers manage or build developing pipelines.

* Cloud Build (版本): Requires Cloud Build API.
* Cloud Task (工作): Do asynchronous jobs (called tasks).
* Container Registry
* Cloud Scheduler
* Cloud Identity Platform: Provide Custom IAM.
* Cloud Source Repositories: Git repositories hosted on GCP.
* Deployment Manager: Deploy multiple resources based on a simple `.yaml` file.
* 私人目錄
* Endpoints: Help developers manage APIs.

### Big Data

a suite of services allows you to process and analyze large datasets

* Composer: Manage Apache Airflow environment on GCP. (Requires Composer API)
* Dataproc: Build Hadoop or Spark clusters and run data transformers on them. Basically, Dataproc supports the batch data pipeline.
* Pub/Sub: It is an architecture for asynchronous streaming data. The subscriber consumes data which is from a topic and is sent by a publisher. (Requires Pub/Sub API)
* Dataflow: A batch and streaming data processing pipeline.
* BigQuery: A serverless data warehouse. It also supports SQL queries and basic ML model, so that users can easily do ETL on datasets and analyze them.
* IoT Core: A fully managed service allows you to manage, connect and ingest data from a huge number of devices. It uses stand MQTT protocol. (Requires Google Cloud IoT API)
* Data Catalog
* Data Fusion: A tool helps users merge data (ETL) from different sources.
* Cloud Healthcare API: The service allows users to access healthcare standards easier. 
* Cloud Genomics: A service helps users manage and analyze genomics data.
* Dataprep: It is a data transform service built on Trifacta, a Web UI tool.

### Artificial Intelligence

A suite of services or APIs enables users to run specific artificial intelligence or machine learning tasks.

* Google Cloud Data Labeling: Label various types of data, including images, videos, text, etc.
* AI Platform (older name: ML Engine): A service allows users to run a complete ML or AI workflow from scratch.
*  Natural Language
* Vision
* Video Intelligence
* Translation
* Talent Solution
* Recommendations AI
* AutoML Tables



## Role and Permissions

There are three different types of roles. (You can view all of them with their role type in `IAM & admin` option on the menu.)

*   viewer
*   editor
*   owner



## APIs and Services

You have to enable the API via the `APIs and Services` > `Libaray` on the navigation menu.



## Cloud Shell

Start the cloud shell via clicking the button on the top-right of the window. The cloud shell has already installed some useful tools, including `gcloud` toolkit. A simple example to list the auth state of the current project.

```sh
$ gcloud auth list
```







