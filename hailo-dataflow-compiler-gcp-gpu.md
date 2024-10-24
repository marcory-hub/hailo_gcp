# C.1. Installing Dataflow Compiler with NVIDIA GPU on Google Cloud Platform

The compulation of the onnx file (made here) to a hef file that runs on you raspberry pi5 AI kit

Steps
1. Convert onnx neural-network graph into a Hailo-compatible representation
2. Quantization of a full precision neural network into an 8-bit model
3. Compiling the network to binary files (HEF) 


Activate your virtual environment:
```sh
. hailo-dfc/bin/activate
```
Install jupyter-notebook
```sh
sudo apt-get install jupyter-notebook
```
Run Jupyter Notebook:
```sh
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```
After starting Jupyter, you'll see a URL in the terminal with a token. Copy this URL.

```sh
http://EXTERNALIP:8888
```

On your local terminal run:
```sh
ssh -N -L localhost:8888:localhost:8888 USERNAME@EXTERNALIP
```
Access Jupyter notebook by typing this in your local browser
```sh
http://localhost:8888
```

