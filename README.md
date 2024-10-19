# hailo

## Training a Custom YOLOv8 Model for Raspberry Pi 5 with Hailo-8L AI kit

#### How to 
- Train a YOLOv8 model from the Hailo Model Zoo with your custom dataset in Google Colab 
- Optimize and compile it with a docker installation of the Hailo Software Suite on Google Cloud Platform (GCP) and 
- Deploy you custom YOLOv8 model on a Raspberry Pi 5 equipped with the Hailo-8L AI kit.

#### Benefits
- No local x86 Unix training computer needed.
- Free Training: Utilize free GPU resources in Colab for efficient training.
- Customizable Model: Train a model to detect objects specific to your needs.
- Hailo-8L Acceleration: Achieve faster and efficient inference on your Raspberry Pi 5.

#### Prerequisites
- Familiarity with Python and basic object detection concepts.
- A Google account (for Colab and GCP).
- Raspberry Pi 5 with Hailo-8L AI kit installed. For instructions see [How to Set Up Raspberry Pi 5 and Hailo-8L](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
- Utilize the basic detection pipeline. For instructions see [Hailo RPi5 Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#installation)

# Steps
1. [Prepare Dataset](prepair-dataset)
2. [Make a YOLOv8 Model in Colab](make-a-yolov8-model-in-colab)
3. [Create a Virtual Machine in Google Cloud Platform](create-a-virtual-machine-in-google-cloud-platform)
4. [Docker Install Hailo Software Suite](docker-install-software-suite)
## 1. Prepare Dataset

Gather images containing the objects you want to detect.
Annotate these images using a tool like CVAT in YOLO1.1 format (bounding boxes and class labels).

## 2. [Make YOLOv8s Model on Colab](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb)

Follow this notebook [hailo_YOLOv8s Google Colab notebook](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb) to train a YOLOv8 model using your custom dataset in Colab. The training process will generate a **best.onnx** file, which represents your trained model. 

Do not forget to download the model before you stop the notebook.

## 3. [Create a Virtual Machine in Google Cloud Platform](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md)

While Colab offers free GPU resources, the virtual machine (VM) is needed to do the docker install of the Hailo Software Suite. 

## 4. [Docker Install of the Hailo Software Suite](https://github.com/marcory-hub/hailo/blob/main/install-hailo-software-suite-on-google-cloud-VM-instance.md)
The suite installation as a Docker file is needed for step 5 optimizing and conversion.

## 5. [Compile the Model using Hailo Model Zoo](https://github.com/marcory-hub/hailo/blob/main/compile-the-model-using-hailo-model-zoo.md)
Use Hailo Model Zoo within the Docker container to convert your model to a Hailo-optimized format (yolov8s.hef).

## 6. [Deploy Model on Raspberry Pi 5]

Transfer the Hailo Executable File **yolov8s.hef** file to your Raspberry Pi 5. This will involve setting up the Hailo runtime environment and integrating your model into your application.
