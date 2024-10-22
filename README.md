# hailo

## Re-training a Custom YOLOv8 Model for Raspberry Pi 5 Hailo-8L AI Kit in the Cloud

#### How to 
- Use your custom dataset in Google Colab to re-train a YOLOv8 model (free).
- Optimize and compile it with a docker installation of the Hailo Software Suite on Google Cloud Platform (GCP) (for good optimization use a pay-as-you-go GPU instance).
- Deploy you custom YOLOv8 model on a Raspberry Pi 5 equipped with the Hailo-8L AI kit.

#### Prerequisites
- Familiarity with Python and basic object detection concepts.
- A Google account for access to Colab (model training) and GCP (optional, for the Hailo Software Suite)
- Raspberry Pi 5 with Hailo-8L AI kit installed. For instructions see [How to Set Up Raspberry Pi 5 and Hailo-8L](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
- Install and follow the basic detection pipeline. For instructions see [Hailo RPi5 Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#installation)
- Paid subscription to GCP if you want to optimize the model. (Optional, recommended for optimization with a GPU instance)

# Steps
### 1. Re-train a YOLOv8 model
1. Prepare Dataset
2. Train YOLOv8 Model in Colab
### 2. Install Docker Hailo Software Suite
#### **Option A:** Free exploration of Hailo Software Suite (_no GPU_)
1. Create a Virtual Machine in Google Cloud Platform 
2. Docker Install Hailo Software Suite
#### **Option B:** Pay-as-you-go Use of Hailo Software Suite (_with GPU_)
1. Using NVIDIA GPUs with Docker on Google Cloud Platform
2. Hailo Software Suite in NVIDIA Docker installation
### 3. Optimize, Compile and Deploy your model
1. Compile the Model using Hailo Model Zoo
2. Deploy Model on Raspberry Pi 5


### Note
 While it's possible to use only a CPU instance (option A) to get familiar with the software, using a GCP instance (option B) with a GPU provides good performance.

## 1.1. Prepare Dataset

Gather images containing the objects you want to detect.
Annotate these images using a tool like [CVAT](https://www.cvat.ai/) in YOLO1.1 format (bounding boxes and class labels). Or use the [hornet3000+](https://www.kaggle.com/datasets/marcoryvandijk/vespa-velutina-v-crabro-vespulina-vulgaris) dataset, more information on the YOLOs8n model on a Raspberry Pi4 8GB can be found [here](https://github.com/vespCV/hornet3000).

## 1.2. [Make YOLOv8s Model on Colab](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb)

Follow this notebook [hailo_YOLOv8s Google Colab notebook](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb) to train a YOLOv8 model using your custom dataset in Colab. The training process will generate a **best.onnx** file, which represents your trained model. 

Do not forget to download the model to your local computer before you stop the notebook.
## **Option A:** Free exploration of Hailo Software Suite (_no GPU_)

## A.1. [Create a Virtual Machine in Google Cloud Platform (no GPU)](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md)
_If you're using pay-as-you-go GPU instances, you can skip this section_.

While Colab offers free GPU resources, the virtual machine (VM) is needed to do the docker install of the Hailo Software Suite. A GPU is not available in the Free Trial.

## A.2. [Docker Install of the Hailo Software Suite](https://github.com/marcory-hub/hailo/blob/main/install-hailo-software-suite-on-google-cloud-VM-instance.md)
_If you're using pay-as-you-go GPU instances, you can skip this section_.

## **Option B:** Pay-as-you-go Use of Hailo Software Suite (_with GPU_)

## B.1. [Using NVIDIA GPUs with Docker on Google Cloud Platform]()
_If you're NOT using pay-as-you-go GPU instances, you can skip this section_.

Optimize and compile a yolo model with a Dockerized Hailo Software Suite on Google Cloud Platform (GCP) using T4 GPU instances.
## B.2.[Hailo Software Suite in NVIDIA Docker installation](https://github.com/marcory-hub/hailo/blob/main/nvidia-docker-hailo-software-suite)
_If you're NOT using pay-as-you-go GPU instances, you can skip this section_.

## 3.1. [Compile the Model using Hailo Model Zoo](https://github.com/marcory-hub/hailo/blob/main/compile-the-model-using-hailo-model-zoo.md)
Use Hailo Model Zoo within the Docker container to convert your model to a Hailo-optimized format (`yolov8s.hef`).

## 3.2. [Deploy Model on Raspberry Pi 5](https://github.com/marcory-hub/hailo/blob/main/deploy-model-on-raspberry-pi-5-ai-kit.md)

Transfer the Hailo Executable File **yolov8s.hef** file to your Raspberry Pi 5. This will involve setting up the Hailo runtime environment and integrating your model into your application.
