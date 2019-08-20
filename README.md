# makeYOLOv3

## Introduction
This is a translation of the details summary of the the [blog]([https://chtseng.wordpress.com/2018/10/08/如何快速完成yolo-v3訓練與預測/](https://chtseng.wordpress.com/2018/10/08/%E5%A6%82%E4%BD%95%E5%BF%AB%E9%80%9F%E5%AE%8C%E6%88%90yolo-v3%E8%A8%93%E7%B7%B4%E8%88%87%E9%A0%90%E6%B8%AC/)), about how to training the yolov3 with your own dataset.
Thanks for the author's ([ch-tseng](https://github.com/ch-tseng/makeYOLOv3)) awesome work.
## Requirements
* OpenCV - [How to install opencv?]([https://www.pyimagesearch.com/2018/05/28/ubuntu-18-04-how-to-install-opencv/](https://www.pyimagesearch.com/2018/05/28/ubuntu-18-04-how-to-install-opencv/)) 
* Darknet - [Darknet official site]([https://pjreddie.com/darknet/yolo/](https://pjreddie.com/darknet/yolo/))
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
```
folderCharacter = "/" # \\ is for windows
# directory of voc format labels (.xml)
xmlFolder = "/media/sf_ShareFolder/tomato_A/labels"
# images directory
imgFolder = "/media/sf_ShareFolder/tomato_A/images"
# directory of training set
saveYoloPath = "/media/sf_ShareFolder/tomato_A/yolo"
# class list of your dataset
# ie: "classList = {"car": 0, "bus": 1, "airplane": 2}"
classList = { "0_tomato_flower":0, "1_tomato_young": 1 }
modelYOLO = "yolov3-tiny" #yolov3 or yolov3-tiny
# how many portion of the dataset will be separated as test dataset
testRatio = 0.2
# configure setting
cfgFolder = "cfg.tomato_A"
cfg_obj_names = "obj.names"
cfg_obj_data = "obj.data"
#
numBatch = 24
numSubdivision = 8
# path to darknet folder
darknetEcec = "../darknet/darknet"
```
