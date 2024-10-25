# Installing and Compiling the Model using Hailo Model Zoo

#### Prerequisits
- Google account
- Paid subscription to Google Cloud Platform (GCP)

## Steps
1. Copy files with dataset and model to your compue engine on GCP
2. Install Hailo Model Zoo
3. Convert onnx file to hef file

## 1. Copy files and images to VM
1. Start your `Compute Engine` on GCP.
2. Put `best.onnx`, `dataset.yaml` and `data.zip` in a folder and compile to `files.zip`. Then copy it to your VM:
```sh
scp PATH_TO/files.zip USERNAME@EXTERNALIP:/home/USERNAME
```
3. SSH to your VM:
```sh
ssh -i ~/.ssh/KEYNAME USERNAME@EXTERNALIP
```
4. Unzip the files
```sh
unzip files.zip
```
```sh
unzip data.zip
```
## 2. Install Hailo Model Zoo
1. Activate the hailo-dfc environment:
```sh
. hailo-dfc/bin/activate
```
2. Clone the Hailo Model Zoo repository:
```sh
git clone https://github.com/hailo-ai/hailo_model_zoo.git
```
2. cd into the folder and install all requirements:
```sh
cd hailo_model_zoo
pip install -e .
```
3. cd back to you working directory:
```sh
cd ..
```
## 3. Convert the onnx file to a hef file with model zoo

1. Start optimization and compilation. Adjust classes number accordingly to your model:
```sh
hailomz compile yolov8s --ckpt=best.onnx --hw-arch hailo8l --calib-path data/train/images --classes 3 --performance
```
2. Finally, use `scp` from your local terminal to copy the `yolov8s.hef` file from your VM to your local machine. Replace the placeholders with your VM username, and desired local path:
```sh
scp USERNAME@EXTERNALIP:yolov8s.hef /LOCAL_PATH
```

Continue with [Deploy Model on Raspberry Pi 5](https://github.com/marcory-hub/hailo/blob/main/rpi-5-hailo-8l-deploy-model.md).