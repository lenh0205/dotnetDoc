===========================================================================
# YOLO (You Only Look Once)
* -> is a **`real-time object detection algorithm`** known for its **speed** and **accuracy**

## Mechanism
* -> detects multiple objects in an image by **`dividing it into a grid`** and **`predicting bounding boxes and class probabilities simultaneously`**

===========================================================================
# Basic using 'YOLOv5' for image detection on your Windows machine

## install "Python":
* _download **Python 3.8+** from https://www.python.org/downloads/_
* _make sure to check **Add Python to PATH** during installation_

## install "PyTorch"
```bash
$ pip install torch torchvision torchaudio
```

##  Clone 'YOLOv5' Repository
* _download and install Git from https://git-scm.com/downloads_
```bash
$ git clone https://github.com/ultralytics/yolov5.git
$ cd yolov5

# install YOLOv5 Requirements/Dependencies
$ pip install -r requirements.txt
``` 

## Download a Pretrained YOLOv5 Model
* -> to actually start using YOLOv5 for object detection, download a **`pre-trained model`** (_e.g., yolov5s for small, yolov5m for medium_):

```bash
$ python detect.py --weights yolov5s.pt --img 640 --source data/images/
# -> use the yolov5s.pt model
# -> detect objects in sample images from "data/images/"
# -> display results in "runs/detect/exp/"
```

## Test with our own image
* -> place our image inside yolov5/data/images/, then run:
```bash
$ python detect.py --weights yolov5s.pt --img 640 --source data/images/your_image.jpg
```

## View the Results
* -> after running detection, check the "runs/detect/exp/" folder for the output image with detected objects

===========================================================================
# Role of 'Python'- For Modifying, Training, and Deploying YOLO Models
* -> Python is the main language used to interact with YOLO
* -> we can modify and fine-tune the model using Python scripts
* -> we can integrate YOLO with other tools (like OpenCV and OCR libraries)

```py
// script to loads a YOLO model, runs inference, and shows detected objects
from ultralytics import YOLO

model = YOLO("yolov8n.pt")  # Load pre-trained YOLOv8 model
results = model("image.jpg")  # Run detection on an image
results.show()  # Display results
```

===========================================================================
# Steps
* basically, use **YOLO to detect the region containing numbers** then **pass detected regions to an OCR model to recognize the actual digits**

# OpenCV - Handling Image Processing Tasks
* -> OpenCV helps with **image input/output, preprocessing, and post-processing** (_resize images, normalize pixel values_)
* -> allows us to **`capture frames from a webcam`**, **`draw bounding boxes`**, and **`apply transformations`**

```py
import cv2

// Use OpenCV to process an image before running YOLO:
image = cv2.imread("image.jpg")  # Read image
image = cv2.resize(image, (640, 640))  # Resize for YOLO
cv2.imshow("Processed Image", image)  # Show the image
cv2.waitKey(0)
```

## Computer Vision - Image Preprocessing & Feature Extraction
* -> **Preprocessing** - before feeding an image into YOLO, it must be resized, normalized, and sometimes converted to grayscale or another format.
* -> **Feature Extraction** - YOLO's CNN automatically extracts features like edges, shapes, and textures, making it useful for detecting objects


# CNN in YOLO - Feature Extraction & Object Detection
* -> YOLO extracts features and predicts bounding boxes & class labels

## Deep Learning Basics: Neural Networks & CNNs
* -> YOLO is built on **`Convolutional Neural Networks (CNNs)`**, a type of **deep learning model** that is specifically designed to **process image data**
* -> a **CNN extracts features** from an image using layers like **`convolution`**, **`pooling`**, and **`activation functions`**
* -> YOLO applies CNNs in **`a single forward pass to detect objects in real-time`**


# Custom Training 
* -> if detecting numbers, we train YOLO on a **labeled dataset of digits**

## PyTorch/TensorFlow - Machine Learning Frameworks 
* -> **`YOLO models`** (YOLOv5, YOLOv8, etc.) are **`implemented using 'PyTorch' or 'TensorFlow'`**
* -> these frameworks provide the **tools for training, fine-tuning, and running YOLO models**
* -> train a custom YOLO model using **`train.py`** (_**`PyTorch`** for **YOLOv5** or Ultralytics for YOLOv8_)

```cs
// Example: If you are training YOLO on custom data, you will:
// -> Use PyTorch or TensorFlow to load and train the model.
// -> Define loss functions and optimizers for training.
// -> Convert and save the trained model for deployment
```

## Data Annotation - Labeling Images for Training Custom Models
* -> to train YOLO on custom data, we must **annotate images with bounding boxes**
* -> tools like **`LabelImg`** (or Roboflow) **`generate YOLO-compatible labels`**

```bash
// YOLO expects labeled data in .txt format, where each line contains:
$ <class_id> <x_center> <y_center> <width> <height>

// Example label file for detecting the number "5":
$ 0 0.45 0.50 0.30 0.40

// 0 → Class ID (e.g., digit 5)
// 0.45 0.50 → Center of bounding box (normalized)
// 0.30 0.40 → Width and height (normalized)
```

# Post-Processing - OCR Integration
* -> After YOLO detects number regions, an OCR tool (like Tesseract) extracts exact digits
* -> YOLO is primarily designed for object detection, not text recognition; for better accuracy in number extraction, we may need to combine YOLO with an OCR model like **`Tesseract`** or **`EasyOCR`**
* -> use **`OCR`** engine to recognize the detected numbers

===========================================================================
# Alternatives to YOLOv5 for Object Detection

## YOLOv8 (Latest Version)
Faster and more accurate than YOLOv5.
Easier to train and modify using Python.
Best for real-time applications.
Use Case: If you want a more optimized and powerful YOLO model

## Faster R-CNN (Region-Based Convolutional Neural Network)
More accurate but slower than YOLO.
Uses region proposals to detect objects.
Use Case: When accuracy is more important than speed (e.g., medical image analysis)

## SSD (Single Shot MultiBox Detector)
Similar to YOLO, but processes images differently.
Faster than Faster R-CNN but less accurate.
Use Case: Works well for small objects in images.

## EfficientDet (Google’s Efficient Object Detection)
More power-efficient than YOLO.
Use Case: Mobile and embedded applications.
