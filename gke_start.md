# Kubernetes Engine: Qwik Start (GSP100)



## Set the default compute zone

The compute zone is a location where the cluster and resource live. First, we need to setup the compute zone via cloud shell.

```sh
gcloud config set compute/zone us-central1-a
```



## Creating a Kubernetes Engine cluster

A cluster consists of at least one cluster master machine and multiple worker machines called workers. A worker is also called a node. A node is essentially a virtual machine instance that runs a Kubernetes process to make itself a part of the cluster.

The following is the command creating a cluster.

```sh
gcloud container clusters create <CLUSTER-NAME>
```

After you created the cluster completely, several VM instances were listed on the `Compute Engine` > `VM instances`. You should see the similar output as the below.

```text
NAME        LOCATION       MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION   NUM_NODES  STATUS
my-cluster  us-central1-a  1.13.7-gke.24   35.188.16.204  n1-standard-1  1.13.7-gke.24  3          RUNNING
```



## Get authentication credentials for the cluster

After creating a cluster, you have to get authentication credentials to interact with it. 

```sh
gcloud container clusters get-credentials <CLUSTER-NAME>
```



## Deploying an application to the cluster

Now you have already created a cluster, you can deploy a containerized application on it.

```sh
kubectl run hello-server --image=gcr.io/google-samples/hello-app:1.0 --port 8080
```

Now create a Kubernetes service that is a Kubernete resource to allow you to expose the application to extenal traffic.

```sh
kubectl expose deployment hello-server --type="LoadBalancer"
```

Now you can inspect the `hello-server` service.

```sh
kubectl get service hello-server
```



## Clean up

You can type the below command to delete the cluster.

```sh
gcloud container clusters delete [CLUSTER-NAME]
```







