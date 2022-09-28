# Fast-Large-Image-Object-Detection-yolov7
## Introduction
Continuous development in the hot topics of Computer Vision and Deep Learning lead us to performance saturated models. Regardless of high-end computing machines and state of the art deep learning models, there exist a computational bottle neck which places an upper cap on resolution of an image that can be processed through the model. Models like Yolo which promises super-speed can process limited scaled image. This makes Yolo unable to process large scale images (e.g., 5000x500). Our research group felt the need to extend this scale and remove the caps of yolo. In the below sections, we’ll walk you through our approach
## Overview
The proposed framework is developer with the aim to handle large scale imagery e.g., aerial, satellite and other. Our framework uses state-f-the-art Yolov7 as backbone and intensive multithreading and parallel processing to process large resolution images which are usually beyond the scope of existing object detection and classification models (which are limited to a specific scale when it comes to processing the images). 
## Our Framework
Our framework takes batch input. Here comes, multithreading to handle batch of input images. An individual thread is initiated to handle each image in batch. To further parallelize the workload and keeping our approach in compliance with object detectors’ grid-based approach, we assign new individual threads to each grid in which each image is divided into. This creates network of nested thread to process grids of each input image in parallel. 
Each cop is passed through Yolo. In this stage object detection is performed. Detailed architecture of Yolov7 can be found on https://arxiv.org/abs/2207.02696
Yolo plots detection boxes on the cropped chunks (without global context of large-scale image). Now its required to stitch small crops of images to get back original large-scale images. Therefore, remapping of detection boxes of cropped images is also required to be post processed and get them plotted on large scale image. To achieve this goal, normalization followed by globalization of coordinates is done. At this stage, another problem arises due to overlapped crops. We get overlapping detection boxes due to underlying overlapped crops. This problem is solver by non max suppression. This will be second NMS in overall pipeline. In this way, we will get rid of overlapping detection boxes. The power of multiprocessing is also utilized for NMS to knock redundant detection boxes.
The structural pipeline of presented solution is shown in figure below:
![alt text](https://github.com/shah0nawaz/Fast-Large-Image-Object-Detection-yolov7-/blob/main/diagrams/Fast-Large-Image-Object-Detection-yolov7.png)


## Dataset Preparation 
The oil yolov7 model is trained on oil storage tanks (OST) dataset https://www.kaggle.com/datasets/airbusgeo/airbus-oil-storage-detection-dataset. If any object detection researcher or practitioner wants to reproduce the resutls. Here is the data preparation procedure of OST dataset.
The detailed data preparation procedure is explained in the https://github.com/shah0nawaz/Oil-Storage-Tanks-Data-Preparation-YOLO-Format.

## Training (yolov7)
Once the dataset is in yolo format, the procedure for training the object detection model as same as expained in yolov7 original repository https://github.com/WongKinYiu/yolov7.


## Testing
```
python test.py 
```
Thanks to these open source repos  
https://github.com/WongKinYiu/yolov7  
https://github.com/avanetten/yoltv4  
https://www.kaggle.com/datasets/towardsentropy/oil-storage-tanks  
https://github.com/shah0nawaz/Oil-Storage-Tanks-Data-Preparation-YOLO-Format
