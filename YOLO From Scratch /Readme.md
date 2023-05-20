## Using YOLO algorithm for Real time detection. 
### Dataset
Sources from Kaggle : PASCAL VOC 2012
### Data Handling 
- Created different directory for Validation dataset and Manually divided data into Train and Dev set 
- Defined Function to Parse XML annotations and returns a Tensor containing Bounding Box and Classes. 
- Defined a function called `generate_output` that takes in a tensor `bounding_boxes` and returns a tensor `output_label`. The purpose of this function is to generate the output label for an object detection task, given the bounding box coordinates of the objects in an image.
- Defined another function `get_imbboxes` that takes in two arguments, `im_path` and `xml_path`, and returns a tuple containing two values, img and bboxes.
- Used the `albumentations library` to define a set of image augmentations. This library is designed to specifcially handle bounding boxes during augmentation 
## Modelling
- Pretrained model used : 'EfficientNetB1'
- Replaced the Fully Connected layers to with convolutional layers to enable end-to-end object detection. These  will retain spatial information and enable object detection at different scales.
- Imported `compute_iou` : computes the intersection over union (IoU) b/w two sets of bounding boxes, it is used evaluation metric, which measures the overlap between two sets of bounding boxes.
- 

## Results
<img width="928" alt="Screenshot 2023-05-20 at 2 11 03 PM" src="https://github.com/ayushs0911/Object-Detection/assets/122048067/13b8bb25-05dd-42a9-945f-b4e6c8f3865d">
