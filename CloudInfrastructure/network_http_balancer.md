# Set Up Network and HTTP Load Balancers (GSP007)



In this tutorial, we are going to setup two different types of load balancers, netowrk and http one.



## Reference

* L3 [Network Load Balancer](https://cloud.google.com/compute/docs/load-balancing/network/)
* L7 [HTTP(s) Load Balancer](https://cloud.google.com/compute/docs/load-balancing/http/)



## Setup gcloud region and zone

```sh
gcloud config set compute/zone us-central1-a
gcloud config set compute/region us-central1
```



## Create multiple web server instances

In GCP, you can setup multiple web server instances via `Instance Templates` and `Managed Instance Groups`. The Instance Templates define the look and the work to run on each virtual machine. The Managed Instance Groups use the template to instantiate the number of the VM instances.



**Create a startup script.**

```sh
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
systemctl start nginx
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

**Create an instance template using the startup script.**

```sh
gcloud compute instance-templates create nginx-template --metadata-from-file startup-script=startup.sh 
```

The output would be like the below.

```text
NAME            MACHINE_TYPE   PREEMPTIBLE  CREATION_TIMESTAMP
nginx-template  n1-standard-1               2019-10-04T20:12:24.252-07:00
```

**Create a target pool.** The target pool allows a single access to all instances and is necessary for load balacner in the future steps.

```sh
gcloud compute target-pools create nginx-pool
```

The output would be like the below.

```text
NAME        REGION       SESSION_AFFINITY  BACKUP  HEALTH_CHECKS
nginx-pool  us-central1  NONE
```

**Create a managed instance group.**

```sh
gcloud compute instance-groups managed create nginx-group \
         --base-instance-name nginx \
         --size 2 \
         --template nginx-template \
         --target-pool nginx-pool
```

The output would be like the below.

```text
NAME         LOCATION       SCOPE  BASE_INSTANCE_NAME  SIZE  TARGET_SIZE  INSTANCE_TEMPLATE  AUTOSCALED
nginx-group  us-central1-a  zone   nginx               0     2            nginx-template     no
```

**We now can list all instances created.**

```sh
gcloud compute instances list
```

The result is like the below.

```text
NAME        ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP      STATUS
nginx-7rbk  us-central1-a  n1-standard-1               10.128.0.2   35.192.186.35    RUNNING
nginx-qtdn  us-central1-a  n1-standard-1               10.128.0.3   104.198.134.199  RUNNING
```

**Create a firewall rule to allow connecting to port 80 via external IP.**

```sh
gcloud compute firewall-rules create www-firewall --allow tcp:80
```



## Create a Network Load Balancer

**Create a forwarding rules.**

```sh
gcloud compute forwarding-rules create nginx-lb \
         --region us-central1 \
         --ports=80 \
         --target-pool nginx-pool
```

**List all forwarding rules.**

```sh
gcloud compute forwarding-rules list
```

The output would be like the below.

```sh
NAME     REGION      IP_ADDRESS     IP_PROTOCOL TARGET
nginx-lb us-central1 X.X.X.X        TCP         us-central1/targetPools/nginx-pool
```

Now you can visit the load balancer via the IP address (http://X.X.X.X).



## Create a HTTP(s) Load Balancer

We first **health-checks** on http environment to verify the instance is responding to HTTP or HTTPS traffic.

```sh
gcloud compute http-health-checks create http-basic-check
```

The output would be like the below.

```text
NAME              HOST  PORT  REQUEST_PATH
http-basic-check        80    /
```

**Now we can define a http service and map a port name to the relevant one for the created instance group, here is `nginx-group`.**

```sh
gcloud compute instance-groups managed \
       set-named-ports nginx-group \
       --named-ports http:80
```

The output would be like the below.

```text
Updated [https://www.googleapis.com/compute/v1/...]
```

**Create a backend service.**

```sh
gcloud compute backend-services create nginx-backend \
      --protocol HTTP --http-health-checks http-basic-check --global
```

The output would be like the below.

```text
Created [https://www.googleapis.com/compute/v1/projects/...].
NAME          BACKENDS PROTOCOL
nginx-backend          HTTP
```

**Add the instance group into this created backend service.**

```sh
gcloud compute backend-services add-backend nginx-backend \
    --instance-group nginx-group \
    --instance-group-zone us-central1-a \
    --global
```

The output would be like the below.

```sh
Updated [https://www.googleapis.com/compute/v1/pr...]
```

**Create a default URL map directing all requests into all your instances.**

```sh
gcloud compute url-maps create web-map --default-service nginx-backend
```

The output would be like the below.

```text
NAME     DEFAULT_SERVICE
web-map  backendServices/nginx-backend
```

**Create a traget HTTP Proxy to route requests to URL map.**

```sh
gcloud compute target-http-proxies create http-lb-proxy --url-map web-map
```

The output would be like the below.

```text
NAME           URL_MAP
http-lb-proxy  web-map
```

**Create a global forwarding rules to handle and route requests.**

```sh
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80
```

The output would be like the below.

```text
Created [https://www.googleapis.com/compute/v1/projects/qw ...]
```

**List all forwarding rules.**

```sh
gcloud compute forwarding-rules list
```

The output would be like the below.

```text
NAME              REGION IP_ADDRESS    IP_PROTOCOL TARGET
http-content-rule        O.O.O.O       TCP         http-lb-proxy
nginx-lb   us-central1   X.X.X.X       TCP         us-central1/....
```

Now you can sruf the http load balancer webpage http://O.O.O.O.











