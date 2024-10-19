# Deploy Model on Raspberry Pi 5 AI kit with Hailo-8L accelerator

#### Before you begin:
- Ensure you have a Raspberry Pi 5 with the Hailo-8L AI kit installed. Refer to the official guide for setup instructions: [How to Set Up Raspberry Pi 5 and Hailo-8L](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
- Familiarize yourself with the basic detection pipeline provided by Hailo: [Hailo RPi5 Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#installation)

## 1. Copy the model
Open a terminal on your local computer and copy the `yolov8s.hef` file to the Raspberry Pi. Replace placeholders with your actual paths and username. The user on the Raspberry Pi needs write permissions to the destination path.
```
scp /path/to/local/yolov8s.hef USERNAME@RASPBERRYPI_IP:/home/hailo/hailo-rpi5-examples/resources/
```
## 2. Configure Labels (on Raspberry Pi):
Navigate to the `resources` folder on your Raspberry Pi. Create a new JSON file named `yolov8s_labels.json` and adjust the following settings:
- `detection_threshold`: This value determines the minimum confidence score for a detection to be considered valid (default 0.5).
- `max_boxes`: This defines the maximum number of detections to be displayed (default 200).
- `labels`: This is a list containing the actual class names for your model (replace with your specific classes). Save this JSON file under `/home/hailo/hailo-rpi5-examples/resources/yolov8_labels.json`.
Example `yolov8s_labels.json`:
```
{
    "detection_threshold": 0.5,
    "max_boxes":4,
    "labels": [
      "Vespa_velutina",
      "Vespa_crabro",
      "Vespula_vulgaris"
    ]
}
```
## 3. Run Inference:

Navigate back to the `hailo-rpi5-examples` directory and source your Hailo virtual environment:
```
source setup_env.sh
```
Run your model (rename `yolov8s.hef` and `yolov8_labels.json` to a more descriptive name). This will utilize the Hailo-8L accelerator to perform object detection using your YOLOv8 model on the Raspberry Pi camera input.
```
python3 basic_pipelines/detection.py --hef-path resources/yolov8s.hef --input rpi --labels-json resources/yolov8_labels.json
```