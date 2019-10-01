# Analyzing data using Datalab and BigQuery



Reference:

*   <https://googlecoursera.qwiklabs.com/focuses/25426>



## Datalab

Create a datalab on a VM named mydatalabvm.

```sh
gcloud compute zones list
datalab create mydatalabvm --zone <ZONE>
```

Reconnect the VM to recovery the datalab after stopping the VM.

```sh
datalab connect mydatalabvm
```



