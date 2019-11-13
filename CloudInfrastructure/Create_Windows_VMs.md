# Create a Windows Virtual Machine (GSP093)



The steps creating Windows VMs are similar to those creating Unix-like VMs. The different step between them is requiring a remoted desktop tool (RDP). If your machine is running out of Windows, you can use a third tool like `Chrome RDP`.

The overall steps are:

*   We can create a Windows VM whose OS is **Windows Server** which can be selected via google cloud console. (`Navigation Menu` > `Compute Engine` > `VM intances`)

*   Activate the cloud shell and type the following command to test the status of windows startup.

    ```sh
    # check the status of windows vm startup, 
    # the message below indicates the startup finished
    # `Finished running startup scripts.`
    gcloud compute instances get-serial-port-output instance-1 --zone us-central1-a
    ```

*   Before you coonect with Windows VM, you have to setup a password. Click the name of VM instance and under the remote access, click `set windows password` button. **The action will generate a new user so that you can access the machine**.

