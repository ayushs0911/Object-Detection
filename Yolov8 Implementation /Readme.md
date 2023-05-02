# Object Detection using Yolov8 pretrained Model. 
**Dataset**
[Arthropod Taxonomy Orders Object Detection Dataset](https://www.kaggle.com/datasets/mistag/arthropod-taxonomy-orders-object-detection-dataset)
```
!kaggle datasets download -d mistag/arthropod-taxonomy-orders-object-detection-dataset
```
**Installations** 
```
! pip install ultralytics
```
**Cloning YOLOv5 Github Repository**
```
!git clone https://github.com/ultralytics/yolov5
!mv yolov5/* ./
```
**Importing YOLO**
```
from ultralytics import YOLO
```
**Loading Model**
```
# Load a model
model = YOLO("yolov8n.pt")  # load a pretrained model (recommended for training)
batch_size = 32
imgsz = 640
```
**Loading Best Weights**
```
model = YOLO("/content/runs/detect/train/weights/best.pt")
```
**Predicting on Test Dataset**
```
for img in test_images:
  results = model.predict(img, save = True)
```
