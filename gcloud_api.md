# Google Cloud API Document



## Reference



-   `gcloud` document: <https://cloud.google.com/sdk/gcloud>



## Quick Note

```sh
gcloud [-h]
  auth # get authorized info
    list
  config # get project configure
    list [--all]
      project
    set # set the resource configure
    	compute/zone <zone_name>
  compute
    instances # related to VM instances
      create <instance-name> --machine-type <n1-standard-2> --zone <zone-id>
      describe <instance-name> --zone <machine-zone>
      list 
    ssh <instance-name> --zone <instance-zone> # connect with SSH
    project-info 
      describe --project <project-id>
    instance-templates # create instance templates
      create <nginx-template> [--metadata-from-file] [startup-script=startup.sh]
    target-pools # create target pools
    	create <nginx-pool>  
    instance-groups 
    	managed 
    		create <nginx-group> # create managed instance group
    			[--base-instance-name nginx] [--size 2]  
    	    [--template nginx-template] [--target-pool nginx-pool]
    	  set-named-ports <nginx-group> [--named-ports http:80] #map port for instance group
    firewall-rules 
    	create <www-firewall> [--allow tcp:80] # create firewall rules
   	forwarding-rules 
   		create <nginx-lb> # create a (L3) network load balancer 
         [--region us-central1] [--ports=80] [--target-pool nginx-pool]
      list # list all forwaring rules, including load balancer's IP addr
    http-health-checks 
    	create <http-basic-check> # http health checks
    backend-services 
    	create <nginx-backend> # create a backend service
      	[--protocol HTTP] [--http-health-checks http-basic-check] [--global]
      add-backend <nginx-backend> # add instance group into backend service
    		[--instance-group nginx-group] [--instance-group-zone us-central1-a] [--global]
    url-maps
    	create <web-map> [--default-service nginx-backend] # create url map
    target-http-proxies # create target http proxy to route requests to url map
    	create <http-ln-proxy> [--url-map web-map]
  components # list all components
  	list
  	update # update SDK installation
  	install <component_id>
  	remove <component_id>
  beta # you have to run `gcloud compinents install beta` first
  	interactive
  container 
  	clusters 
  		create <CLUSTER-NAME> # create a cluster
      get-credentials <CLUSTER-NAME>  		
      delete <CLUSTER-NAME> # delete a cluster
	pubsub # for Pub/Sub
		topics 
			create
			list
	functions # for cloud functions
		deploy <function-name> 
			[--allow-unauthenticated \] 
			[--stage-bucket gs://${STAGING_BUCKET_NAME} \] 
			[--trigger-topic ${UPLOAD_NOTIFICATION_TOPIC} \] # based on specific pub/sub topic
			[--entry-point GCStoPubsub \] 
			[--runtime nodejs8 \]
			[--timeout 540 \]
		logs
			read [--filter "GCStoPubsub"] [--limit 100]
		delete <function-name>
	iam 
		service-accounts
			create <my-account> --display-name <my-account> # create a sevice count
			keys create <key.json> [--iam-account=my-account@$PROJECT.iam.gserviceaccount.com]
	projects 
		add-iam-policy-binding $PROJECT \
			[--member=serviceAccount:my-account@$PROJECT.iam.gserviceaccount.com] \
			[--role=roles/bigquery.admin]
```

