# Introduction to Kubernetes Engine (GSP100)

keywords: `kubernetes`, `k8s`

## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/878?parent=catalog

## Quick Notes

* `Kubernetes` provides a way of managing, deploying, and scaling the Docker `container` apps.

* In GCP, the Kubernetes engine consists of several compute engines to form a `container cluster`.

When using Kubernetes as the management tool for the container apps, the following benefits are provided:
  * `Load balancing` for the compute engine instances
  * `Node Pools` for a subset of nodes within the cluster to provide additional flexibility
  * `Automatic scaling` the cluster's nodes
  * `Automatic updating` the cluster's node software
  * `Automatic repairing` the cluster's node to maintain the health and availability
  * `Logging and monitoring` the cluster state

## Kubernetes on GCP

* Setting a default compute zone

Access the cloud shell and type the following command to create a k8s cluster.

```sh
# prototype: gcloud config set compute/zone <zone-name>
$ gcloud config set compute/zone us-central1-a
```

* Creating a K8s cluster

A cluster consists of one master machine and several working machines called `Nodes`. In GCP, machines are `Compute Engine Instances`.

```sh
# prototype: gcloud container cluster create <cluster-name>
$ gcloud container clusters create service1
```

* Get the authentication credentials for the cluster

You have to get the credentials to interact with the cluster.

```sh
# prototype: gcloud container clusters get-credentials <cluster-name>
$ gcloud container clusters get-credentials service1
```

* Deploy the app to the cluster

Next, you are going to deploy the containerized app to the cluster. In Kubernetes Engines, Kubernetes use `Object` to create and manage the resources. Kubernetes provides `Deployment` object for deploying stateless applications like web services, and provides `Service` object to define the rule and the balance for accessing the applications from the internet.

Run the following commands to create a deployment from the container image.

```sh
$ kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

Run the following commands to create a service exposing an existing deployment and also create a LoadBalancer for this application.

```sh
$ kubectl expose deployment hello-server --type=LoadBalancer --port 8080
```

After you create a deployment and create a service using expose, you can use `get` command to list the services.

```sh
$ kubectl get service
```

You can see the `EXTERNEL-IP` and surf the link `http://[EXTERNEL-IP]:8080/`.

* Clean the app and the cluster.

```sh
$ gcloud container clusters delete service1
```