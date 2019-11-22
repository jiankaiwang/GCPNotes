# Awwvision: Cloud Vision API from a Kubernetes Cluster (GSP066)



keywords: `kubernetes clsuters`, `vision API`, `container`, `web-app`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1241?parent=catalog
* Cloud Vision sample on GCP: https://github.com/GoogleCloudPlatform/cloud-vision



## Quick Notes



### Create a Kubernetes Engine cluster

```sh
# setup zone
gcloud config set compute/zone us-central1-a

# create a k8s cluster
gcloud container clusters create awwvision \
    --num-nodes 2 \
    --scopes cloud-platform
    
# use the container's credentials
gcloud container clusters get-credentials awwvision

# verify the cluster's health
kubectl cluster-info
```



### Get and Deploy the Sample Code

```sh
# clone the source code
git clone https://github.com/GoogleCloudPlatform/cloud-vision

# deployment
cd cloud-vision/python/awwvision
make all
```



### Check the Kubernetes resources on the cluster

```sh
# list pods
kubectl get pods

# list deployments
kubectl get deployments -o wide

# get deployment in details
kubectl get svc awwvision-webapp
```



### Visit your new web app and start its crawler

View the webapp via the IP address listed from the `kubectl get svc` command. Click the `Start the Crawler` button. Click the `Go Back` to see the result.





