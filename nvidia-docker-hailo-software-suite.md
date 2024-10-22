# B.2. Hailo Software Suite in NVIDIA Docker installation

Following NVIDIA GPU installation ([B.1.](https://github.com/marcory-hub/hailo/blob/main/nvidia-docker-installation-in-gcp.md)), we'll delve into the setup of the Docker Hailo Software Suite and the necessary steps to enable Docker's utilization of the GPU.

#### Prerequisits 
- Google account
- Paid subscription to Google Cloud Platform (GCP)
- Access to Hailo Developer Zone
- GCP Compute Engine with NVIDIA GPUs and Docker (see [B.1.](https://github.com/marcory-hub/hailo/blob/main/nvidia-docker-installation-in-gcp.md))

## Steps
1. Docker Install of the Hailo Software Suite
2. Use Docker with NVIDIA GPU

## 1. Docker Install of the Hailo Software Suite
1. Access the [Hailo Developer Zone] (https://hailo.ai/developer-zone/sw-downloads/)(_account creation required_).
2. Download the latest Software Suite (Latest releases):
- Software package: AI Software Suite
- Software subpackage: AI Software Suite
- Architecture: x86
- OS: Linux 
- Python version: 3.8
3. Open a terminal on your local computer and upload the .zip file to you home directory in the VM with this command. "LOCAL_PATH" should be replaced with the actual path to the downloaded files on the local machine. Similarly, USERNAME and EXTERNALIP" should be replaced with the username and external IP of the VM.
```sh
scp LOCAL_PATH USERNAME@EXTERNALIP:/home/USERNAME
```
5. SSH into the GCP VM:
```sh
ssh -i ~/.ssh/KEYNAME USERNAME@EXTERNALIP
```
6. Unzip the Hailo Software Suite archive:
```sh
sudo apt install unzip
```
```sh
unzip hailo_ai_sw_suite_2024-10_docker.zip
```
6. Then run the script - which opens a new container and get inside the container with this command. The script might take a while to download and set up the container.
```sh
./hailo_ai_sw_suite_docker_run.sh
```
You can drink some coffee or tea and ignore the Xauthority error as it's not crucial for non-graphical applications.

7. Exit docker with the `exit` command. 

## 2. Use Docker with NVIDIA GPU

1. For each session run this command to confirm that your system has GPUs and that Docker can access them:
```sh
docker run --gpus all nvidia/cuda:11.8.0-base-ubuntu20.04 nvidia-smi
```
```sh
./hailo_ai_sw_suite_docker_run.sh --resume
```
2. To test the hailo suite you can test the following command to give a list of available commands or to see the installed packages
```sh
hailo -h
```
```sh
pip list | grep hailo
```

Continue with [Compiling YOLOv8 Model with Hailo Model Zoo](https://github.com/marcory-hub/hailo/blob/main/compile-the-model-using-hailo-model-zoo.md).





