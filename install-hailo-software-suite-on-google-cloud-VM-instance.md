# How to Install Hailo SW Suite Without Required Hardware (Part 2/2)

## Install Hailo Software Suite on Google Cloud Platform

This guide continues from [Part 1: Create Google Cloud Platform VM Instance and Connect via SSH](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md), assuming you've successfully set up a virtual machine (VM instance) on Google Cloud Platform (GCP) and established a secure SSH connection to it. In this part, we installing the Hailo Software Suite on the VM.

## Install Docker

1. Open a terminal in your local machine. Use the following command, replacing USERNAME with your GCP username and EXTERNALIP with the copied IP. Confirm  the connection when prompted.

```sh
ssh -L 8888:localhost:8888 USERNAME@EXTERNALIP
```
2. Follow the steps from the Docker website under 1 [Install using the `apt` repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
3. Replace step 2 by this command to install the specific version of Docker Engine as required for the suite installation as Docker file:
```sh
VERSION_STRING=5:20.10.6~3-0~ubuntu-focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```

```sh
sudo usermod -aG docker USERNAME
```
4. Stop and start the VM instance
5. to verify that the user is indeed in the docker group use the command
```sh
groups USERNAME
```
## Install Hailo AI software suite
1. download both the suite and PCIe driver from [Developer Zone](https://hailo.ai/developer-zone/sw-downloads/) 
2. open a new terminal on your local computer
3. upload the .zip file and de .deb file to you home directory with this command in your local terminal with this command. Replace local_file_path, USERNAME, EXTERNALIP and remote_path
```sh
scp local_file_path USERNAME@EXTERNALIP:remote_path
```
4. install the PCIe driver
```sh
sudo apt update
sudo apt install build-essential
sudo dpkg -i hailort-pcie-driver_4.18.0_all.deb
```
5. Reboot the VM instance
```sh
sudo reboot
```
6. SSH into the VM instance again with, be patient and then SSH again with the command
```sh
ssh -i ~/.ssh/CGPkey USERNAME@EXTERNALIP
```
7. Extract the suite archive
```sh
sudo apt install unzip
```
8. then run the script - whick opens a new container, and attaches to it ("gets inside it") with this command (adjust version if needed)
```sh
unzip hailo_ai_sw_suite_2024-07.1_docker.zip
```
9.
```sh
./hailo_ai_sw_suite_docker_run.sh
```
You can likely ignore the Xauthority error as it's not crucial for non-graphical applications.


to see the installed packages
```sh
pip list | grep hailo
```

to see the list of available commands
```sh
hailo -h
```

exit docker with the exit command
Go back to the docker container use
```sh
./hailo_ai_sw_suite_docker_run.sh --resume
```



