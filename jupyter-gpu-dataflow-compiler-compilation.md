# C.2. Using Hailo Dataflow Compiler with NVIDIA GPU on Google Cloud Platform with Jupyter notebook

Work in progress

To-do
- Replace by Jupyter notebook when finished

#### Prerequisits
- Hailo Dataflow Compiler with Jupyter notebook installation as described [here](https://github.com/marcory-hub/hailo/blob/main/jupyter-gpu-dataflow-compiler-installation.md)

## Steps

## 1.

1. Setup and Initialization.
Import Required Libraries:
```python
import tensorflow as tf
from hailo_sdk_client import ClientRunner
```
2. Set Hardware Architecture:
Define the hardware architecture for the Hailo-8L.
```python
chosen_hw_arch = "hailo8l"
```
3. Parse ONNX Model to HAR:
Use the Hailo SDK to parse the ONNX model into a Hailo Archive (HAR) format.
```python
runner = ClientRunner(hw_arch=chosen_hw_arch)
hn, npz = runner.translate_onnx_model(
    "model.onnx",
    "yolov8s",
    start_node_names=["input"],
    end_node_names=["output"],
    net_input_shapes={"input": [1, 3, 640, 640]}
)
```
4. Save HAR File:
```
python
hailo_model_har_name = "yolov8s_hailo_model.har"
runner.save_har(hailo_model_har_name)
```
#### Compilation and Deployment
1. Quantize the HAR file and compile it into a HEF file for deployment on Hailo-8L.
```bash
!hailomz optimize yolov8s --hw-arch hailo8l --har yolov8s_hailo_model.har
!hailomz compile yolov8s --hw-arch hailo8l --har yolov8s_hailo_model.har
```
Deploy on Device:
Use scp from your local terminal to copy the yolov8s.hef file from your VM to your local machine. Replace the placeholders with your VM username, VM path to the file, and desired local path:
```sh
scp USERNAME@EXTERNALIP:/path/to/file /local/path/to/file
```

### Optimization
```python
# General imports
import os
import numpy as np
from PIL import Image
from hailo_sdk_client import ClientRunner

# YOLOv8 specific preprocessing 
def preprocess_yolov8(image, input_size=(640, 640)):
    """Preprocess image for YOLOv8"""
    img = Image.open(image).convert('RGB')
    img = img.resize(input_size, Image.BILINEAR)
    img = np.array(img) / 255.0  # Normalize to [0,1]
    img = np.transpose(img, (2, 0, 1))  # HWC to CHW
    return img

# Prepare calibration dataset
images_path = "../data"
images_list = [img_name for img_name in os.listdir(images_path) if img_name.endswith((".jpg", ".png"))]

calib_dataset = np.zeros((len(images_list), 3, 640, 640), dtype=np.float32)
for idx, img_name in enumerate(sorted(images_list)):
    img_path = os.path.join(images_path, img_name)
    img_preproc = preprocess_yolov8(img_path)
    calib_dataset[idx] = img_preproc

np.save("yolov8s_calib_set.npy", calib_dataset)

# Load the HAR file
model_name = "yolov8s"
hailo_model_har_name = f"{model_name}_hailo_model.har"
assert os.path.isfile(hailo_model_har_name), "Please provide valid path for HAR file"
runner = ClientRunner(har=hailo_model_har_name)

# Create a model script for YOLOv8s
# Note: YOLOv8 doesn't typically need normalization as it's done in preprocessing
alls = ""  # Add any specific model script commands if needed

# Load the model script
runner.load_model_script(alls)

# Optimize the model
runner.optimize(calib_dataset)

# Save the optimized model
quantized_model_har_path = f"{model_name}_quantized_model.har"
runner.save_har(quantized_model_har_path)
```

### Compilation
```python
from hailo_sdk_client import ClientRunner

# Set the model name and path for YOLOv8s
model_name = "yolov8s"
quantized_model_har_path = f"{model_name}_quantized_model.har"

# Load the network to the ClientRunner
runner = ClientRunner(har=quantized_model_har_path)

# Run compilation
try:
    hef = runner.compile()
    
    # Save the compiled HEF file
    file_name = f"{model_name}.hef"
    with open(file_name, "wb") as f:
        f.write(hef)
    print(f"Successfully compiled and saved {file_name}")

except Exception as e:
    print(f"Compilation failed: {str(e)}")
    
    # Additional error handling and debugging
    if "No successful assignment" in str(e):
        print("Compilation failed due to resource allocation issues.")
        print("Consider the following steps:")
        print("1. Check if the model size is suitable for Hailo-8L.")
        print("2. Try adjusting the 'resources_param' in the compilation settings.")
        print("3. Review the model architecture for any unsupported operations.")

# Run the profiler tool
har_path = f"{model_name}_compiled_model.har"
runner.save_har(har_path)

# Use a try-except block for the profiler command
try:
    !hailo profiler {har_path}
    print("Profiler report generated successfully.")
except Exception as e:
    print(f"Error running profiler: {str(e)}"
```

