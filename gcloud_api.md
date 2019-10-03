# Google Cloud API Document



## Reference



-   `gcloud` document: <https://cloud.google.com/sdk/gcloud>



## Quick Reference

```sh
gcloud [-h]
  auth # get authorized info
    list
  config # get project configure
    list [--all]
      project
    set # set the resource configure
  compute
    instances # related to VM instances
      create <instance-name> --machine-type <n1-standard-2> --zone <zone-id>
      describe <instance-name> --zone <machine-zone>
    ssh <instance-name> --zone <instance-zone> # connect with SSH
    project-info 
      describe --project <project-id>
  components # list all components
  	list
  	update # update SDK installation
  	install <component_id>
  	remove <component_id>
  beta # you have to run `gcloud compinents install beta` first
  	interactive
```

