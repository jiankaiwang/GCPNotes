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
### Tools
### Big Data
### Artificial Intelligence



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







