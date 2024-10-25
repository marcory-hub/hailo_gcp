# DataFlow Compiler using NVIDIA GPUs with on Google Cloud Platform

This guide provides steps for setting up a GCP Compute Engine instance with GPU support to run the Hailo DataFlow Compiler. This guide provides a basic configuration. You might need to adjust settings based on your specific needs.

#### Prerequisits
- Google account
- Paid subscription to Google Cloud Platform (GCP)

## Steps
1. Create a VM with GPU
2. Generate public- and private SSH key
3. Upload public key to CGP
4. Use private key to connect VM with local terminal
5. Install pre-requisets as listed in the [DFC documentation](https://hailo.ai/developer-zone/documentation/v3-29-0/?sp_referrer=install/install.html)
6. Install additional requirements for GPU hardware emulation 

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
## 5. Installing pre-requisets
1. Install python 3.8 packages
```sh
sudo apt-get update
```
```
sudo apt-get install python3.8 python3.8-dev python3.8-distutils python3.8-tk
```
```sh
sudo apt-get install python3-pip python3-venv
```
Install graphviz and libgraphviz-dev:
```sh
sudo apt-get install graphviz libgraphviz-dev
```
```sh
export PATH=$PATH:/usr/share/graphviz
```
Verify the installations
```sh
python3.8 --version
pip3 --version
graphviz --version
```
2. Install driver 525
```sh
sudo apt install nvidia-driver-525
```
3. Install CUDA 11.8 and add it to PATH
```sh
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run
```
```sh
echo 'export PATH=/usr/local/cuda-11.8/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
``` 
```sh
source ~/.bashrc
```
```sh
sudo reboot
```
```sh
nvidia-smi
nvcc -V
```
4. cuDNN 8.9 Installation
Download the cuDNN.deb package from NVIDIA
File: `cudnn-local-repo-ubuntu2004-8.9.5.30_1.0-1_amd64.deb`
_Requires an NVIDIA Developer account and email address._
Add the repository key:
```sh
sudo cp /var/cudnn-local-repo-ubuntu2004-8.9.5.30/cudnn-local-B731B5EB-keyring.gpg /usr/share/keyrings/
```
Install the cuDNN .deb package:
```sh:
sudo dpkg -i cudnn-local-repo-ubuntu2004-8.9.5.30_1.0-1_amd64.deb
```
Update the package list:
```sh
sudo apt-get update
```
Install the cuDNN library and development files:
```sh
sudo apt-get install libcudnn8 libcudnn8-dev
```
Link the cuDNN libraries to CUDA's lib64 directory:
```sh
sudo ln -sf /usr/lib/x86_64-linux-gnu/libcudnn*.so* /usr/local/cuda-11.8/lib64/
```
Verify the installation
```sh
ldconfig -p | grep libcudnn
```
5. Install Hailo Dataflow Compiler
Create virtual environment and enter it:
```sh
sudo apt install virtualenv
```
```sh
virtualenv hailo-dfc
```
```sh
. hailo-dfc/bin/activate
```
6. In the venv install the hailo DFC:
```sh
pip install hailo_dataflow_compiler-3.29.0-py3-none-linux_x86_64.whl 
```
Check the installation with:
```sh
hailo -h
```
```sh
pip freeze | grep hailo
```

