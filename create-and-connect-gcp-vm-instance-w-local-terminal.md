# Create Google Cloud Platform VM instance and connect GC VM using SSH from Terminal

This guide walks you through creating a Virtual Machine (VM) instance in Google Cloud Platform (GCP)

Prerequisits:
	Google account (you can use an existing one)
## Generate public- and private key

1 Generate keys
	In your local terminal generate a public and private key with the following command below
		replace PATH with a secure location on your local drive where you store your keys (for example ~/.ssh/)
		replace KEYNAME with a descriptive name (for example CGPkey)
		replace USERNAME with your username in GCP 
		a strong passphrase is recommended for added security
```sh
ssh-keygen -t rsa -b 4096 -f PATH/KEYNAME -C USERNAME
```

## Create a VM instance

1 Create or Select a Project:
	Creat a NEW PROJECT or use an excisting project in Google Cloud Platform (cloud.google.com)
2 Create VM instance
	in the Navigation Menu (top left) navigate to Compute Engine > VM instances > click CREATE INSTANCE (top middel)
3 Configure the VM instance
	Name: 
		choose a descriptive name for your VM
	Region/Zone: 
		select a region based on your location or target audience (affects latency). Choose a zone within the region for redundancy (availability).
	Machine configuration: 
		select E2	
		select e2-standard-32gb (16gb to save costs)
	Available policies (optional):
		GCP offers Spot VMs for lower costs. However, these VMs can be interrupted if needed by GCP, but 60-90% discount
	Boot disk:
		Operating system: select Ubuntu
		optional: Boot disk type: select Standard persistent disk to save costs
		Size (GB): start with a smaller size (20 GB) and resize upwars later if needed. Downgrading size is not possible.
	Firewall:
		activate allow HTTP traffic (needed for Jupyter notebook on local browser)
	Click CREATE
	
This guide provides a basic configuration. You might need to adjust settings based on your specific needs.
## Upload public key to CGP

1 Add public SSH key
	in GCP Navigation Menu navigate to Compute Engine > Metadata > SSH KEYS > EDIT > +ADD ITEM
	open public key in your local terminal (for example with nano GCPkey.pub)
	copy this key
	in GCP click ADD SSH KEY
	paste the key in SSH key 1
	click SAVE

## Use private key to connect VM with local terminal

1 Obtain VM External IP:
	In the GCP Navigation Menu, navigate to Compute Engine > VM instances
	Ensure the VM instance is active. if not,  click Start/resume in the instance's menu 
	Copy External IP address of the instance

2 Establish SSH Connection
	Open a terminal in your local machine
	Use the following command, replacing USERNAME with your GCP username and EXTERNALIP with the copied IP
	confirm  the connection when prompted 
```sh
ssh -i ~/.ssh/CGPkey USERNAME@EXTERNALIP
```
	
You have now successfully established an SSH tunnel to your VM instance and can interact with it from your local terminal.

**Troubleshooting:** 
Connection refused
	Firewall: Ensure SSH port (22 by default) is open in the VM's firewall rules.
	SSH service: Verify SSH service is running on the VM using sudo systemctl status sshd. If not, start it with sudo systemctl start sshd.
Permission denied (publickey)
	Key permissions: Ensure private key is readable only by you: chmod 600 ~/.ssh/CGPkey.
	Authorized_keys: Verify your public key is added to the authorized_keys file on the VM (usually located at ~/.ssh/).
Connection timed out
	Network connectivity: Check your internet connection and network configuration.
	VM status: Ensure the VM is running and accessible.