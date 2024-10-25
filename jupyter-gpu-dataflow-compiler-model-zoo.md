# C.2. Using Hailo Dataflow Compiler with NVIDIA GPU on Google Cloud Platform with Model Zoo

Work in progress

To-do
- Replace by Jupyter notebook when finished

#### Prerequisits
- Hailo Dataflow Compiler with Jupyter notebook installation as described [here](https://github.com/marcory-hub/hailo/blob/main/jupyter-gpu-dataflow-compiler-installation.md)

## Steps

## 1. Activate VN on GCP and Run Jupyter Notebook

1. Activate your virtual environment:
```sh
. hailo-dfc/bin/activate
```
2. Run Jupyter Notebook:
```sh
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```
After starting Jupyter, you'll see the URLs in the terminal with a token:`Or copy and paste one of these URLs:
        http://hailo-dfc:8888/?token=` followed by the token.
Copy this token.
3. **Option A:** Open your browser and use this URL:
```sh
http://EXTERNALIP:8888
```
Enter the token to get access to the Jupyter notebook.
3. **Option B:** When you have firewall issues run this on your local terminal:
```sh
ssh -N -L localhost:8888:localhost:8888 USERNAME@EXTERNALIP
```
Access Jupyter notebook by typing this in your local browser
```sh
http://localhost:8888
```
Enter the token to get access to the Jupyter notebook.
## 2.
