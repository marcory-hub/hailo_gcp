# B.1. Using NVIDIA GPUs with Docker on Google Cloud Platform

This guide provides steps for setting up a GCP Compute Engine instance with GPU support to run the Hailo Software Suite using Docker. This guide provides a basic configuration. You might need to adjust settings based on your specific needs.

#### Prerequisits
- Google account
- Paid subscription to Google Cloud Platform (GCP)

## Steps
1. Create a VM with GPU
2. Generate public- and private SSH key
3. Upload public key to CGP
4. Use private key to connect VM with local terminal
5. Install Docker
6. NVIDIA Docker Installation
7. Run a Docker Container with GPU

## 1. Create a VM with GPU

1. **Create or Select a Project**: Creat a `NEW PROJECT` or use an excisting project in Google Cloud Platform (cloud.google.com)
2. **Create VM instance**: In the Navigation Menu (top left) navigate to `Compute Engine` > `VM instances` > click `CREATE INSTANCE` (top middel)
3. **Configure the VM instance**: 
**Name**: choose a descriptive name such as `hailo-gpu`.

**Region/Zone**: Select a region based on your location or target audience (affects latency). Choose a zone within the region for redundancy (availability). 

**Machine configuration**: select `GPU` -> `n1-standard-8` (8 vCPU, 30 GB memory). GCP offers Spot VMs for lower costs. However, these VMs can be interrupted if needed by GCP, but 60-90% discount. 

**Boot disk**: 
Click `SWITCH IMAGE` and select
  - Image: Ubuntu 20.04 LTS x86/64
  - Standard persistent disk
  - Size: 200 GB
  - Standard persistent disk to save costs. 
  - Size (GB): 200 GB and resize upwars later if needed. Downgrading size is not possible. 

**Firewall**: activate allow HTTP traffic (needed for Jupyter notebook on local browser).
5. Click `CREATE`

## 2. Generate public- and private key

In your local terminal generate a public- and private key with the following command below. Replace PATH with a secure location on your local drive where you store your keys (for example ~/.ssh/). Replace KEYNAME with a descriptive name (for example CGPkey). Replace USERNAME with your username in GCP. (Optional: a strong passphrase for added security).
```sh
ssh-keygen -t rsa -b 4096 -f PATH/KEYNAME -C USERNAME
```

## 3. Upload public key to CGP

1. In GCP Navigation Menu navigate to `Compute Engine` > `Metadata` > `SSH KEYS` > `EDIT` > `+ADD ITEM`
2. Open your public key in your local terminal (for example with nano GCPkey.pub)
3. Copy this key
4. In GCP click `ADD SSH KEY`
5. Paste the key in SSH key #
6. click 'SAVE'

## 4. Use private key to connect VM with local terminal

1. **Obtain VM External IP**: In the GCP Navigation Menu, navigate to Compute Engine > VM instances. Ensure the VM instance is active. If not, click Start/resume in the instance's menu. Copy External IP address of the instance.
2. **Establish SSH Connection**: Open a terminal in your local machine. Use the following command, replacing ~/.ssh with the path you use to store public- and private keys. Replace KEYNAME with your key-name (such as GCPkey), USERNAME with your GCP username and EXTERNALIP with the copied IP, confirm the connection when prompted.
```sh
ssh -i ~/.ssh/KEYNAME USERNAME@EXTERNALIP
```

## 5. Install Docker

1. Follow the steps from the Docker website under 1 [Install using the `apt` repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
2. Replace step 2 by checking the VERSION_STRING in the first command (if needed replace it with a updated version). Then run these commands to install the specific version of Docker Engine as required for the suite installation as Docker file.
```sh
VERSION_STRING=5:20.10.6~3-0~ubuntu-focal
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```
3. Add your USERNAME to the docker group
```sh
sudo usermod -aG docker USERNAME
```
4. Reboot, SSH to the instance again (as described before under `Establish SSH Connection) and run the following command to check if your user belongs to the docker group.
```sh
groups USERNAME
```

## 6. NVIDIA Docker Installation

1. Install the NVIDIA Driver 525:

```sh
sudo apt install libnvidia-common-525 libnvidia-gl-525 nvidia-driver-525 -y
```
Reboot after installation and SSH to the instance again as described before.
```sh
sudo reboot
```
```sh
ssh -i ~/.ssh/KEYNAME USERNAME@EXTERNALIP
```

2. Set up the NVIDIA Docker Repository:
```sh
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```
3. Install NVIDIA Container Toolkit:
```sh
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```
4. Install CUDA 11.8:
```sh
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run
```
- Select `continue` when installer finds an existing package manager installation of the driver.
- Uncheck the driver installation as we've already installed driver 525.
- Then select `install`installation option.
5. Set up environment variables. Add the following lines to your ~/.bashrc file:
```sh
echo 'export PATH=/usr/local/cuda-11.8/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```
Apply the changes:
```sh
source ~/.bashrc
```
6. Verify the installation with:
```sh
nvidia-smi
nvcc -V
```
## 7. Run a Docker Container with GPU

1. To run a basic Docker container with GPU support:
```sh
docker run --gpus all nvidia/cuda:11.8.0-base-ubuntu20.04 nvidia-smi
```
Continue with [B.2 Hailo Software Suite in NVIDIA Docker installation](https://github.com/marcory-hub/hailo/blob/main/nvidia-docker-hailo-software-suite.md).
