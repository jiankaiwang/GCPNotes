# Create a Virtual Machine (GSP001)

keywords: `VM instance`, `cloud shell`

## Reference

* Qwiklabs: https://google.qwiklabs.com/focuses/3563?parent=catalog
* `gcloud` document: <https://cloud.google.com/sdk/gcloud>

## Quick Notes

### Step



Here we are going to use `gcloud` CLI and `Google Cloud Console` to create VMs. The following is the basic gcloud commands.

```sh
# list the active account information
gcloud auth list

# show the information about the project
gcloud config list project
```

You can simply activate the `cloud shell` to use gcloud CLI.

All GCP resources are allocated based on `regions` and `zones`. A region consists of multiple zones. The integration among multiple resources must be in the same zone. For example, if you want to attach a persistent disk with a virtual machine, both of them must be in the same zone.



### Create a VM from Google Cloud Console

In the GCP console, on the top-left of the window, select `Navigation menu` > `Compute Engine` > `VM instance`.

After created a VM, click the `SSH` on the end of the line (under the column name `connect`). Create a NGINX server.

```sh
# login as superuser
sudo su -

# update the repo
apt-get update

# install nginx
apt-get install nginx -y

# make sure the install was complete
ps auwx | grep nginx
```

After install the nginx server, back to gcp console, find the external IP and click it or copy and paste it on the  browser url and click go. You should see the welcome page of nginx.



### Create a VM from Google Cloud Shell

It is easy to create a VM via cloud shell. Activate the cloud shell on the top-right of the screen. Type the following command to create a VM.

```sh
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone [your_zone]
```





