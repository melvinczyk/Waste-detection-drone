# Tello waste detection drone

## Project Description
This is a project that implements real-time object detection with custom and collected data on a [Tello](https://www.ryzerobotics.com/de/tello) drone.

I used the official [YOLOv5](https://github.com/ultralytics/yolov5) model by Ultralytics and trained it on two custom datasets. I used a custom personal dataset with ~70 images and one dataset [Trash Detection Image Dataset](https://universe.roboflow.com/trash-dataset-for-oriented-bounded-box/trash-detection-1fjjc/dataset/10) was used to train a more accurate model with ~1800 images. The code for the models is under `'/yolov5'`, the personal model is under `'/Personal-Trash-Data'`, and the roboflow dataset is under `'/Medium-Trash-Data'`.

The personal model was labelled and processed using [Roboflow](https://roboflow.com/).

## Real-time detection
This project uses real-time detection and logging for each run that you do. Each log is documented in the `'/Logs'` directory with each run being labeled `'/runX'` with `X` being the current run iteration. In ever

My personal model was trained on ~80 images with 100 epochs and trained in ~3 hrs. Here is what the real time detection looks like for my model:
![personal_gif](/images/ours.gif)

The roboflow model consists of ~1800 images with 50 epochs and batch size of 244. The training took ~12 hrs and here is what the results from this model look like:
![medium_model_gif](/images/model_.gif)

### Comparison

Here is a comparison between my personal dataset and the online roboflow:
![personal](/images/Our%20model%20frame.png)
![online_model](/images/Medium%20model%20frame.png)

### Usage

*This is a python based project. You need python installed on your machine to run this program.*

Must connect to a Tello drone and run `python main.py` in the shell. A black box will appear which is used to capture user input for controls. The drone can be controlled via the keyboard with the following commands:

* 'e': takeoff
* 'q': land
* 'w': forward
* 'a': left
* 's': backwards
* 'd': right
* 'K_UP': up
* 'K_DOWN': down
* 'K_LEFT': rotate ccw
* 'K_RIGHT': rotate cw