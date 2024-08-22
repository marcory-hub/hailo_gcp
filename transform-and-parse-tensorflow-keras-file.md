# Transforming and Parsing a Tensorflow Keras file (made with Teachable Machine) with Hailo suite on GCP 

This guide provides a step-by-step process for transforming and parsing a TensorFlow Keras model created with Teachable Machine using the Hailo Dataflow Compiler. The process involves training a model on Teachable Machine, downloading it in Keras format, open the Google Cloud VM instance and resume the Docker container, and transferring the model to the VM for further processing.


- **input**: keras_model.h5
- **output**: keras_model_hailo_model.har

  

#### Requirements
- VM instance: [Create Google Cloud Platform VM instance and connect GC VM using SSH from Terminal](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md)
- Installed Hailo Software Suite [Install Hailo SW Suite on Google Cloud Platform](https://github.com/marcory-hub/hailo/blob/main/install-hailo-software-suite-on-google-cloud-VM-instance.md)
- Basic knowledge of Jupyter Notebooks



## Create a TensorFlow Keras file with Teachable Machine

1. Create a  [Teachable Machine](https://teachablemachine.withgoogle.com/) Model: follow the instruction on the website to train your model. You can optionally adjust training parameters like epochs, batch size, and learning rate for better performance. You find this option under the advanced option.
2. Download the model: Download the model in Keras format (.h5 file). (Optional: download also the TFLite model, so you can compare it with the transformed Keras model).

## Upload the .h5 file to the docker container
1. Start the VM instance on the Google Cloud console. Establish an SSH Connection and resume the docker container with these commands.
```sh
ssh -L 8888:localhost:8888 USERNAME@EXTERNALIP
```
```sh
./hailo_ai_sw_suite_docker_run.sh --resume
```
2. Upload your own keras_model.h5 with your local terminal to GCP VM instance to a temp directory. Replace PATH_TO with the path to your keras_model.h5 file on your local machine and adjust USERNAME@EXTERNALIP with your username and the external IP of your VM instance.
```sh
scp /PATH_TO/keras_model.h5 USERNAME@EXTERNALIP:/tmp/keras_model.h5
```
3. Identify and copy the model to the docker container. List all Docker containers and identify the Hailo envirionment with this command.
```sh
docker ps -a
```
4. Copy the uploaded model from the VM's temp directory to the container with this command (replace CONTAINER_ID with the Docker container ID you identified).
```sh
docker cp /tmp/keras_model.h5 CONTAINER_ID:/local/workspace/hailo_virtualenv/lib/python3.8/site-packages/hailo_tutorials/models
```
5. Start the Hailo tutorial and copy the provided URL to your browser to open the Jupyter Notebook for further exploration. 
```sh
hailo tutorial
```
You can open and read DFC_0_Introduction. Then read and run through DFC_1_Parsing_Tutorial. 

## Transform and parse your own keras_model.h5 file
Now let's do it with your keras_model.h5 file. You can add the code blocks under the existing md and code blocks of the Parsing Tutorial.

1. Import necessary libraries for running the script within the Jupyter Notebook environment.
```sh
# General imports used for transformation, parsing and visualization
import tensorflow as tf
from IPython.display import SVG # Scalable Vector Graphics for visualization in Jupyter notebook
 
# Import the ClientRunner class from the hailo_sdk_client package
from hailo_sdk_client import ClientRunner
```
```sh
# Adjust the hardware architecture to hailo8l
chosen_hw_arch = "hailo8l"
```
2. Update model_name and model_path.
```sh
model_name = "keras_model" 

model_path = "../models/keras_model.h5"
model = load_model(model_path)
```
3. Convert your Keras Model to TFLite
```sh
# Define the converter with options
converter = tf.lite.TFLiteConverter.from_keras_model(model)
converter.target_spec.supported_ops = [
    tf.lite.OpsSet.TFLITE_BUILTINS,  # enable TensorFlow Lite ops.
    tf.lite.OpsSet.SELECT_TF_OPS,  # enable TensorFlow ops.
]

# Convert the model
tflite_model = converter.convert()  # may cause warnings, ignore them

# Save the converted TFLite model
tflite_model_path = "../models/keras_model.tflite"
with tf.io.gfile.GFile(tflite_model_path, "wb") as f:
    f.write(tflite_model)
```
4. Translate TFLite to Hailo format (using ClientRunner)
This part loads the TFLite model, creates a ClientRunner instance, translates the TFLite model to the Hailo format and saves the translated model as a HAR file. Then it visualizes the Hailo model using the Hailo Visualize and displays the SVG visualization of the model. 
```sh
# Path to the TensorFlow Lite model
tflite_model_path = "../models/keras_model.tflite"

# Name for the Hailo model
model_name = "keras_model"

# Create a ClientRunner instance to interact with the Hailo hardware
runner = ClientRunner(hw_arch=chosen_hw_arch)

# Translate the TFLite model to the Hailo format and get the model handle and weights
hailo_model_handle, npz_weights = runner.translate_tf_model(tflite_model_path, model_name)

# Save the translated Hailo model as a HAR file
hailo_model_har_name = f"{model_name}_hailo_model.har"
runner.save_har(hailo_model_har_name)

# Visualize the Hailo model using the Hailo Visualizer
!hailo visualizer {hailo_model_har_name} --no-browser

# Display the SVG visualization of the Hailo model
SVG("keras_model.svg")
```





