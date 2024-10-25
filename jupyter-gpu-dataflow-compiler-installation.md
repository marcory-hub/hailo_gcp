# C.1. Installing Dataflow Compiler with NVIDIA GPU on Google Cloud Platform

The compulation of the onnx file (made [here](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb)) to a hef file that runs on you Raspberry Pi5 AI kit

#### Prerequisits
- Google account
- Paid subscription to Google Cloud Platform (GCP)

#### Steps
1. Create a VM with GPU
2. Generate public- and private SSH key
3. Upload public key to CGP
4. Use private key to connect VM with local terminal
5. NVIDIA installation in GCP
5. Install and access Jupyter.

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

## 5. NVIDIA Installation in GCP

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
2. Install NVIDIA Container Toolkit:
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
5. Verify the installation with:
```sh
nvidia-smi
nvcc -V
```

## 5. Install and access Jupyter
1. Activate your virtual environment:
```sh
. hailo-dfc/bin/activate
```
2. Install jupyter-notebook
```sh
sudo apt-get install jupyter-notebook
```
3. Run Jupyter Notebook:
```sh
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```
After starting Jupyter, you'll see the URLs in the terminal with a token:`Or copy and paste one of these URLs:
        http://hailo-dfc:8888/?token=` followed by the token.
Copy this token.
4. **Option A:** Open your browser and use this URL:
```sh
http://EXTERNALIP:8888
```
Enter the token to get access to the Jupyter notebook.
4. **Option B:** When you have firewall issues run this on your local terminal:
```sh
ssh -N -L localhost:8888:localhost:8888 USERNAME@EXTERNALIP
```
Access Jupyter notebook by typing this in your local browser
```sh
http://localhost:8888
```
Enter the token to get access to the Jupyter notebook.

---
After this installation next time to use Jupyter
1. Start the VM
2. Activate your virtual environment:
```sh
. hailo-dfc/bin/activate
```
3. Run Jupyter Notebook:
```sh
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```
After starting Jupyter, you'll see the URLs in the terminal with a token:`Or copy and paste one of these URLs:
        http://hailo-dfc:8888/?token=` followed by the token.
Copy this token.
4. **Option A:** Open your browser and use this URL:
```sh
http://EXTERNALIP:8888
```
Enter the token to get access to the Jupyter notebook.
4. **Option B:** When you have firewall issues run this on your local terminal:
```sh
ssh -N -L localhost:8888:localhost:8888 USERNAME@EXTERNALIP
```

