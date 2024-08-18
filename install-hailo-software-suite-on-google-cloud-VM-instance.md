# How to Install Hailo SW Suite Without Required Hardware (Part 2/2)

## Install Hailo Software Suite on Google Cloud Platform

This guide continues from [Part 1: Create Google Cloud Platform VM Instance and Connect via SSH](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md), assuming you've successfully set up a virtual machine (VM instance) on Google Cloud Platform (GCP) and established a secure SSH connection to it. In this part, we installing the Hailo Software Suite on the VM.

## Install Docker

1. Open a terminal in your local machine. Use the following command, replacing USERNAME with your GCP username and EXTERNALIP with the copied External IP from the VM instance. Confirm the connection when prompted.

```sh
ssh -L 8888:localhost:8888 USERNAME@EXTERNALIP
```
2. Follow the steps from the Docker website under 1 [Install using the `apt` repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
3. Replace step 2 by checking the VERSION_STRING in the first command (if needed replace it with a updated version). Then run these commands to install the specific version of Docker Engine as required for the suite installation as Docker file and add your user to the docker group
```sh
VERSION_STRING=5:20.10.6~3-0~ubuntu-focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```
```sh
sudo usermod -aG docker USERNAME
```
4. Stop and start the VM instance. Run the following command to check if your user belongs to the docker group.
```sh
groups USERNAME
```
## Install Hailo AI software suite
1. Download both the Hailo Software Suite and PCIe driver from [Developer Zone](https://hailo.ai/developer-zone/sw-downloads/) (you need to create an account on hailo.ai to access the downloads).
2. Open a new terminal on your local computer.
3. Upload the .zip file and de .deb file to you home directory with this command in your local terminal with this command. "local_file_path" should be replaced with the actual path to the downloaded files on the local machine. Similarly, "remote_path" should be replaced with the desired location on the VM (e.g., /home/USERNAME/)
```sh
scp local_file_path USERNAME@EXTERNALIP:remote_path
```
4. Install the PCIe driver
```sh
sudo apt update
sudo apt install build-essential
sudo dpkg -i hailort-pcie-driver_4.18.0_all.deb
```
5. Reboot the VM instance and SSH into the VM instance again with these commands
```sh
sudo reboot
```
```sh
ssh -i ~/.ssh/CGPkey USERNAME@EXTERNALIP
```
6. Extract the suite archive
```sh
sudo apt install unzip
```
```sh
unzip hailo_ai_sw_suite_2024-07.1_docker.zip
```
7. Then run the script - which opens a new container and get inside the container with this command (adjust version if needed). The script might take a while to download and set up the container.
```sh
./hailo_ai_sw_suite_docker_run.sh
```
You can ignore the Xauthority error as it's not crucial for non-graphical applications.

Exit docker with the `exit` command. Go back to the docker container with this command
```sh
./hailo_ai_sw_suite_docker_run.sh --resume
```
8. To test the hailo suite you can test the following command to give a list of available commands or to see the installed packages
```sh
hailo -h
```
```sh
pip list | grep hailo
```





