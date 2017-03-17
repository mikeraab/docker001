# Use these instructions to access a Oracle Linux VM via SSH

## If the VM is a Worker Node in Container Cloud Service:

If you are using an Oracle Container Cloud Service (OCCS) instance for this lab, you can utilize its pre-built Linux VM, Docker Engine and UI.  

You should have already provisioned your OCCS instance as part of the prework for this HOL.  

The first step will be to login to one of the OCCS "worker nodes", or Docker host, and verify the Docker installation and check the version.  A worker node is simply a Docker Host/VM that can run Docker containers.

First SSH into a Worker Node in your Pre-built ContainerCS instance using the SSH key that you used when you created the OCCS instance. 

To find a Worker Node IP address, login to your Oracle Cloud My Services Portal and use one of the Public IPs from a Worker Node in the Container Cloud Service Console:

<img src=images/003-worker-ip.png />


Modify the below command with your Worker Node IP and the path for your private key:

```
$ ssh opc@ip_address -i /users/yourName/folder/sshkey/privateKey
```

> *Note - the above format is usable directly in a Mac terminal. If you are using a Windows computer, use an appropriate Windows SSH client, like Putty or another SSH client*
