# A Tour of Qwiklabs and the Google Cloud Platform (GSP282)



### Quick Notes

*   Service Categories: https://cloud.google.com/docs/overview/cloud-platform-services#top_of_page
*   Role documents: <https://cloud.google.com/iam/docs/understanding-roles#primitive_roles>
*   Google API Design Guide: <https://cloud.google.com/apis/design/>



## Service Categories

There are seven categories of services provided by Google Cloud Platform.

*   Compute
*   Storage
*   Networking
*   Stackdriver
*   Tools
*   Big Data
*   Artificial Intelligence



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







