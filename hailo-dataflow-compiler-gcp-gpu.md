# C.1. Installing Dataflow Compiler with NVIDIA GPU on Google Cloud Platform

The compulation of the onnx file (made [here](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb)) to a hef file that runs on you Raspberry Pi5 AI kit

Steps
1. Install and access Jupyter.
2. Convert onnx neural-network graph into a Hailo-compatible representation.
3. Quantization of a full precision neural network into an 8-bit model.
4. Compiling the network to binary files (HEF).

## 1. Install and access Jupyter
1. Activate your virtual environment:
```sh
. hailo-dfc/bin/activate
```
2. Install jupyter-notebook
```sh
sudo apt-get install jupyter-notebook
```
3. Run Jupyter Notebook:
```sh
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```
After starting Jupyter, you'll see the URLs in the terminal with a token:`Or copy and paste one of these URLs:
        http://hailo-dfc:8888/?token=` followed by the token.
Copy this token.
4. **Option A:** Open your browser and use this URL:
```sh
http://EXTERNALIP:8888
```
Enter the token to get access to the Jupyter notebook.
4. **Option B:** When you have firewall issues run this on your local terminal:
```sh
ssh -N -L localhost:8888:localhost:8888 USERNAME@EXTERNALIP
```
Access Jupyter notebook by typing this in your local browser
```sh
http://localhost:8888
```
Enter the token to get access to the Jupyter notebook.
