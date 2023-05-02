# Object Detection using Yolov8 pretrained Model. 
**Dataset**<br>
[Arthropod Taxonomy Orders Object Detection Dataset](https://www.kaggle.com/datasets/mistag/arthropod-taxonomy-orders-object-detection-dataset)
<br>
<img width="487" alt="Screenshot 2023-05-02 at 3 37 07 PM" src="https://user-images.githubusercontent.com/122048067/235639008-c4c72be3-c7c6-473d-8949-a213057c6ba7.png">
<br>

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

### Model Was trained for 3 epochs only. 
With additional training, it has the potential to produce improved outcomes.<br>
![image](https://user-images.githubusercontent.com/122048067/235774239-4c7c3576-d9b1-45d9-aef9-fa16a9663499.png)


**Export**
```
model.export(format='onnx', dynamic=True)
```

