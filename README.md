# hailo

## Re-training a Custom YOLOv8 Model for Raspberry Pi 5 Hailo-8L AI Kit in the Cloud

#### How to 
- Use your custom dataset in Google Colab to re-train a pre-trained YOLOv8 model (free).
- Optimize and compile it on Google Cloud Platform (GCP) with Hailo Dataflow Compiler and Model Zoo (for good optimization use a pay-as-you-go GPU instance).
- Deploy you custom YOLOv8 model on a Raspberry Pi 5 equipped with the Hailo-8L AI kit.

#### Prerequisites
- Familiarity with Python and basic object detection concepts.
- A Google account for access to Colab (model training) and GCP (90 days free trail or pay-as-you-go)
- Raspberry Pi 5 with Hailo-8L AI kit installed. For instructions see [How to Set Up Raspberry Pi 5 and Hailo-8L](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
- Install and follow the basic detection pipeline. For instructions see [Hailo RPi5 Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#installation)

# Quick guide to deploy a yolov8s model re-trained with your images on Raspberry Pi 5-AI kit with hailo-8L accelerator
1. Prepare Dataset
2. Train YOLOv8s Model on Colab 
3. Installing Dataflow Compiler with NVIDIA GPU
4. Installing and Compiling the Model using Hailo Model Zoo 
5. Deploy Model on Raspberry Pi 5

## 1. Prepare Dataset
Gather images containing the objects you want to detect. You find tips for best training results [here](https://github.com/ultralytics/yolov5/wiki/Tips-for-Best-Training-Results#dataset)
Annotate these images using a tool like [CVAT](https://www.cvat.ai/) in YOLO1.1 format (bounding boxes and class labels). Or use the [hornet3000+](https://www.kaggle.com/datasets/marcoryvandijk/vespa-velutina-v-crabro-vespulina-vulgaris) dataset, more information on the YOLOs8n model on a Raspberry Pi4 8GB can be found [here](https://github.com/vespCV/hornet3000).

## 2. [Train YOLOv8s Model on Colab](https://github.com/marcory-hub/hailo/blob/main/colab_yolov8s_create_model.ipynb)
Follow this [Google Colab notebook to train a yolov8s model](https://github.com/marcory-hub/hailo/blob/main/colab_yolov8s_create_model.ipynb). The training process will generate a **best.onnx** file, which represents your trained model. 

Do not forget to download the model to your local computer before you stop the notebook.

## 3. [Installing and Compiling the Model using Hailo Model Zoo in the Dataflow Compiler environment](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-gpu-dfc-model-zoo)

## 4. [Compile the Model using Hailo Model Zoo](https://github.com/marcory-hub/hailo/blob/main/model-zoo-compilation.md)
Use Hailo Model Zoo within the Docker container to convert your model to a Hailo-optimized format (`yolov8s.hef`).

## 5. [Deploy Model on Raspberry Pi 5](https://github.com/marcory-hub/hailo/blob/main/rpi-5-hailo-8l-deploy-model.md)

Transfer the Hailo Executable File **yolov8s.hef** file to your Raspberry Pi 5. This will involve setting up the Hailo runtime environment and integrating your model into your application.

# -=* WORK IN PROGRESS *=- 
## Exploration of Hailo Software Suite (_no costs, no GPU_)
While Colab offers free GPU resources, the virtual machine (VM) is needed to do the docker install of the Hailo Software Suite. A GPU is not available in the Free Trial.
## 1. [Create a Virtual Machine in Google Cloud Platform (no GPU)](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-no-gpu-installation.md)
## 2. [Docker Install of the Hailo Software Suite on GCP](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-no-gpu-docker-software-suite-installation.md)

## Pay-as-you-go Use of Docker Compiler (_with GPU_)
## 1. [Installing only Dataflow Compiler on GCP with NVIDIA GPU](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-gpu-dataflow-compiler-installation.md)

## Pay-as-you-go Use of Hailo Software Suite (_with GPU_)
## 1. [Using NVIDIA GPUs with Docker on GCP with NVIDIA GPU](gcp-vm-gpu-docker-installation.md)
## 2.[Hailo Software Suite in NVIDIA Docker installation](https://github.com/marcory-hub/hailo/blob/main/gcp-vm-gpu-docker-software-suite-installation.md)

##  
## C.1. [Installing Dataflow Compiler with NVIDIA GPU on Google Cloud Platform](https://github.com/marcory-hub/hailo/blob/main/jupyter-gpu-dataflow-compiler-installation.md)
## C.2. [Using Hailo Dataflow Compiler with NVIDIA GPU on Google Cloud Platform with Jupyter notebook](https://github.com/marcory-hub/hailo/blob/main/jupyter-gpu-dataflow-compiler-model-zoo.md)






