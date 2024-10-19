# Compiling YOLOv8 Model with Hailo Model Zoo

This guide outlines how to compile your YOLOv8 model for object detection using Hailo Model Zoo within a Docker container.

### Prerequisites

- An [active VM instance on Google Cloud Platform (GCP)](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md)
- The ["runs.zip" file](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb) containing the best.onnx model
- The [Hailo Software Suite Docker container running on your VM](https://github.com/marcory-hub/hailo/blob/main/install-hailo-software-suite-on-google-cloud-VM-instance.md)

## 1. Copy `best.onnx` to VM
Unzip `runs.zip` locally and locate the `best.onnx` file (found in /runs/detect/train/weights/best.onnx).

Open a local terminal and use `scp` to copy the file to your VM. Replace `PATH_TO` with the local path to `best.onnx`, `USERNAME` with your VM username, and `EXTERNALIP` with your VM's external IP address.
```
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
```
docker cp /home/USERNAME/best.onnx CONTAINER-ID:/local/workspace/
```
Resume the container:
```
./hailo_ai_sw_suite_docker_run.sh --resume
```
## 3. Generate the COCO TFRecord files
The COCO TFRecord files are needed to optimize and compile the YOLOv8 model using the Hailo Model Zoo. These may already exist in your Hailo setup, but if not, you can generate them.

Run the following commands inside the Docker container to download the COCO dataset and generate TFRecord files in the folder `/local/shared_with_docker/.hailomz/coco/val2017/`
```
python hailo_model_zoo/hailo_model_zoo/datasets/create_coco_tfrecord.py val2017
```
```
python hailo_model_zoo/hailo_model_zoo/datasets/create_coco_tfrecord.py calib2017
```
## 4. Compile your YOLOv8 Model
To optimize and compile the YOLOv8 model, run the following command within the Docker container. Replace `--classes 3` with the actual number of classes in your model.
```
hailomz compile yolov8s --ckpt=best9.onnx --hw-arch hailo8l --calib-path /local/shared_with_docker/.hailomz/coco/val2017/ --classes 3 --performance
```
## 5. Copy Compiled Model to your local computer
Exit the Docker container using `exit`.

Use `docker cp` to copy the compiled model (`yolov8s.hef`) from the container to your VM's home folder.
```
docker cp CONTAINER_NAME:/local/workspace/yolov8s.hef /home/USERNAME
```
Finally, use `scp` from your local terminal to copy the `yolov8s.hef` file from your VM to your local machine. Replace the placeholders with your VM username, VM path to the file, and desired local path:
```
scp USERNAME@EXTERNALIP:/path/to/file /local/path/to/file
```

