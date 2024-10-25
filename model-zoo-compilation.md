# 3.1 Compiling the Model using Hailo Model Zoo

WORK IN PROGRESS

This guide outlines how to optimize (if you have a GPU) and compile your YOLOv8 model for object detection using Hailo Model Zoo within a Docker container.

### Prerequisites

- A VM with hailo software suite [with GPU](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-gpu-docker-software-suite-installation.md) or [no-GPU](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-no-gpu-docker-software-suite-installation.md), the latter does not optimize the model.
- The ["runs.zip" file](https://github.com/marcory-hub/hailo/blob/main/colab_yolov8s_create_model.ipynb) containing the best.onnx model


## 1. Copy `best.onnx` to VM
Unzip `runs.zip` locally and locate the `best.onnx` file (can be found in /runs/detect/train/weights/best.onnx).

Open a local terminal and use `scp` to copy the file to your VM. Replace `PATH_TO` with the local path to `best.onnx`, `USERNAME` with your VM username, and `EXTERNALIP` with your VM's external IP address.
```sh
scp PATH_TO/runs/detect/train/weights/best.onnx USERNAME@EXTERNALIP:/home/USERNAME
```

## 2. Copy the `best.onnx` file to Docker Container
In your local terminal SSH into your VM with
```
ssh -i ~/.ssh/CGPkey USERNAME@EXTERNALIP
```
Find the docker `CONTAINER-ID` of the Hailo Software Suite by running:
```
sudo docker ps -a
```
Identify the container related to Hailo if multiple containers are running. 

Copy `best.onnx` to the containers's workspace folder (replace `CONTAINER_ID` with the actual ID):
```sh
docker cp /home/USERNAME/best.onnx CONTAINER-ID:/local/workspace/
```
Resume the container:
```sh
./hailo_ai_sw_suite_docker_run.sh --resume
```
## 3. Generate the COCO TFRecord files (_if needed_)
The COCO TFRecord files are needed to optimize and compile the YOLOv8 model using the Hailo Model Zoo. These may already exist in your Hailo setup, but if not, you can generate them.

Run the following commands inside the Docker container to download the COCO dataset and generate TFRecord files in the folder `/local/shared_with_docker/.hailomz/coco/val2017/`
```sh
python hailo_model_zoo/hailo_model_zoo/datasets/create_coco_tfrecord.py val2017
```
```sh
python hailo_model_zoo/hailo_model_zoo/datasets/create_coco_tfrecord.py calib2017
```
## 4. Compile your YOLOv8 Model
To optimize and compile the YOLOv8 model, run the following command within the Docker container. Replace `--classes 3` with the actual number of classes in your model.
```sh
hailomz compile yolov8s --ckpt=best9.onnx --hw-arch hailo8l --calib-path /local/shared_with_docker/.hailomz/coco/val2017/ --classes 3 --performance
```
## 5. Copy Compiled Model to your local computer
Exit the Docker container using `exit`.

Use `docker cp` to copy the compiled model (`yolov8s.hef`) from the container to your VM's home folder.
```sh
docker cp CONTAINER_NAME:/local/workspace/yolov8s.hef /home/USERNAME
```
Finally, use `scp` from your local terminal to copy the `yolov8s.hef` file from your VM to your local machine. Replace the placeholders with your VM username, VM path to the file, and desired local path:
```sh
scp USERNAME@EXTERNALIP:/path/to/file /local/path/to/file
```

Continue with [Deploy Model on Raspberry Pi 5](https://github.com/marcory-hub/hailo/blob/main/rpi-5-hailo-8l-deploy-model.md)