# 3.1 Compiling the Model using Hailo Model Zoo

WORK IN PROGRESS

This guide outlines how to compile your YOLOv8 model for object detection using Hailo Model Zoo.

### Prerequisites

## 1. Copy files and images to VM
Put `best.onnx`, `dataset.yaml` and `data.zip` in a folder and compile to `files.zip`.
```sh
scp PATH_TO/files.zip USERNAME@EXTERNALIP:/home/USERNAME
```
Unzip the file
```sh
unzip files.zip
```
```sh
unzip data.zip
```
## 2. Install Hailo Model Zoo
1. Activate the hailo-dfs environment:
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
3. cd back to you working directory
```sh
cd ..
```

## 3. Convert the onnx file to a hef file with model zoo

1. Adjust classes number accordingly to your model:
```sh
hailomz compile yolov8s --ckpt=best.onnx --hw-arch hailo8l --calib-path data/train/images --classes 3 --performance
```
You can run without --performance to test the command
2. Finally, use `scp` from your local terminal to copy the `yolov8s.hef` file from your VM to your local machine. Replace the placeholders with your VM username, and desired local path:
```sh
scp USERNAME@EXTERNALIP:yolov8s.hef /LOCAL_PATH
```

Continue with [Deploy Model on Raspberry Pi 5](https://github.com/marcory-hub/hailo/blob/main/rpi-5-hailo-8l-deploy-model.md).