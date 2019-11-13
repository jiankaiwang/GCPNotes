# Getting Started with Cloud Shell & gcloud (GSP002)



## Reference



* Google Cloud SDK (`gcloud`): <https://cloud.google.com/sdk/docs/quickstarts?hl=zh-tw>



## Installing and Initializing gcloud

Surf the webpage (<https://cloud.google.com/sdk/docs/quickstarts?hl=zh-tw>) and download the sdk according to your machine.

* make sure python env in your machine is 2.7

```sh
  python -V
```

* Download the tar file from the above webpage and extract it to the appropriate path. Install the google cloud sdk via the following command.

```sh
  ~/google_cloud_sdk/install.sh
```

* Initialize the gcloud tool.

```sh
  gcloud init
```

* Make sure the configure is complete.

```sh
  gcloud has now been configured!
  You can use [gcloud config] to change more gcloud settings.
  Your active configuration is: [default]
```


## Setting up envicronment variables

```sh
export PROJECT_ID=<your_project_ID>
gcloud compute project-info describe --project <your_project_ID>

# use the default zone fetched from the output of the above command
export ZONE=<your_zone>

echo $PROJECT_ID
echo $ZONE
```



## Create a VM via gcloud command

```sh
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone $ZONE
```



## Using gcloud commands

Please refer to the [doc](gcloud_api.md).



## Auto-Completion

`gcloud interactive` has auto prompting for commands, like flags, sub-commands, etc.

Next we are going to install the beta components.

```sh
gcloud components install beta
```

Enter interactive mode.

```sh
gcloud beta interactive
```

Now you can use `tab` key to complete the command, like path, etc. When a dropdown menu appears, use `tab` key to move through the list and use `space` to select the choice.



## Connecting to the machine via SSH

```sh
gcloud compute ssh <instance-name> --zone <machine-zone>
```

