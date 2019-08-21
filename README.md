# makeYOLOv3

## Introduction
This is a translation of the details summary of the the [blog](https://tinyurl.com/y7juz9fp), about how to training the yolov3 with your own dataset.
Thanks for the author's ([ch-tseng](https://github.com/ch-tseng/makeYOLOv3)) awesome work.
## Requirements
* OpenCV - [How to install opencv?](https://www.pyimagesearch.com/2018/05/28/ubuntu-18-04-how-to-install-opencv/)
* Darknet - [Darknet official site](https://pjreddie.com/darknet/yolo/)
## Training with your own dataset
[train.py](https://github.com/dawnknight/makeYOLOv3/blob/master/train.py "train.py") is the combination script of [1_labels_to_yolo_format.py](https://github.com/dawnknight/makeYOLOv3/blob/master/1_labels_to_yolo_format.py "1_labels_to_yolo_format.py"), [2_split_train_test.py](https://github.com/dawnknight/makeYOLOv3/blob/master/2_split_train_test.py "2_split_train_test.py"), [3_make_cfg_file.py](https://github.com/dawnknight/makeYOLOv3/blob/master/3_make_cfg_file.py "3_make_cfg_file.py") and [4_train_yolo.py](https://github.com/dawnknight/makeYOLOv3/blob/master/4_train_yolo.py "4_train_yolo.py").

* (optional) **Generate your own dataset and labels**: After collecting all the images, we can starting generate the labels for all the images. We can use [LabelImg]([https://github.com/tzutalin/labelImg](https://github.com/tzutalin/labelImg)) to the labeling process. The label format can select between yolo or voc.

In the [train.py](https://github.com/dawnknight/makeYOLOv3/blob/master/train.py "train.py"), it includes 5 steps as follwoing:
* **Create a yolo dataset**: convert voc format label (.xml)  to yolo format label (.txt) (optional) and save all the yolo labels and corresponding images in the same folder.
*  **Split the dataset in the training and testing dataset**: Accoding the the *testRatio*, the entire dataset separate into testing and training dataset. The list of the training and testing set save in the *cfgFolder* as test.txt and train.txt.
* **Generate the corrsponding cfg file**: Generate the *obj.names* and *obj.data* and save it in the *cfgFolder*. 
* **Pre-training process**:  Download the pre-trained yolo model [darknet53.conv.74](https://pjreddie.com/media/files/darknet53.conv.74), and according to *modelYOLO* and other attributes modify yolov3-tiny.cfg or yolov3.cfg.
* **Start training**: The checkpoints will save in "path_to_cfgFolder/weights/." 
### Some details in [train.py](https://github.com/dawnknight/makeYOLOv3/blob/master/train.py "train.py")

* Directory of voc format labels (.xml)
```
xmlFolder = "/media/sf_ShareFolder/tomato_A/labels"
```
* Images directory
```
imgFolder = "/media/sf_ShareFolder/tomato_A/images"
```
* Directory of training set
```
saveYoloPath = "/media/sf_ShareFolder/tomato_A/yolo"
 ```
* Class list of your dataset. ie: "classList = {"car": 0, "bus": 1, "airplane": 2}"
```
classList = { "0_tomato_flower":0, "1_tomato_young": 1 }
```
* Type of the yolo model, you want to train. Choose between yolov3 and yolov3-tiny.
```
modelYOLO = “yolov3-tiny"
```
* How many portion of the dataset will be separated as test dataset
```
testRatio = 0.2
```
* Configure setting
```
cfgFolder = "cfg.tomato_A"
cfg_obj_names = "obj.names"
cfg_obj_data = "obj.data"
```
* Path to darknet folder
```
darknetEcec = "../darknet/darknet"
```
* If you want to use GPU, you can add ``` "-gpus 0,1,2,3" ``` at the end of line 220.
```
executeCmd = darknetEcec +  " detector train "  + cfgFolder + folderCharacter + "obj.data "  + cfgFolder + folderCharacter + fileCFG +  " darknet53.conv.74"  + " -gpus 0,1,2,3"
```
* (Ubuntu) If you want to save the training logs, you can add  ```"| tee path_to_logfile" ```
```
executeCmd = darknetEcec + " detector train " + cfgFolder + folderCharacter + "obj.data " + cfgFolder + folderCharacter + fileCFG + " darknet53.conv.74"+" -gpus 0,1,2,3 | tee trainlog.txt"
```


## Traing 
```
python train.py
```
## Testing
* Single image detection
```
python playYOLO.py -i image_name.file_extension
ie. python playYOLO.py -i test.jpg
```
* Video detection
```
python playYOLO.py -v video_name.file_extension
ie. python playYOLO.py -v test.mp4
```
### Some details in [playYOLO.py](https://github.com/dawnknight/makeYOLOv3/blob/master/playYOLO.py "playYOLO.py")

* Model type - related to *modelYOLO* in  [train.py](https://github.com/dawnknight/makeYOLOv3/blob/master/train.py "train.py")
```
modelType = “yolo"
```
* *cfg_obj_names* in [train.py](https://github.com/dawnknight/makeYOLOv3/blob/master/train.py "train.py"), which include the classes in the training dataset.
```
classesFile = “../darknet/data/voc.names"
```
* Path to configure file - yolov3.cfg or yolov3-tiny.cfg in *cfgFolder*
```
modelConfiguration = “../cfgFolder/yolov3.cfg"
```
* Path to  the trained weights - should in /*cfgFolder*/weights/
```
modelWeights = “../cfgFolder/weights/yolov3.weights"
