# YOLO From Scratch
"You only Look Once" (YOLO) is a popular algorithm because it achieves high accuracy while also being able to run in real time. This algoritm "only looks once" at the image in sense that it requires only **one forward propagation** pass through the network to make predictions. After non-max suppression, it then ouputs recognized objects together with bounding boxes. 

## How it Works?
1. Input Image is divided into NxN Grid cells. For each object present on image, one grid cell is repsonsible fore predicting object. 
2. Each grid predicts 'B' bounding box and 'C' class probabilities. And Bounding Box consists of 5 components (x,y,w,h, confidence). 
<img width="820" alt="Screenshot 2023-05-03 at 9 00 10 AM" src="https://user-images.githubusercontent.com/122048067/235827658-80d985f4-0628-4163-894a-4aea0122e7e9.png"><br>

The cell which has centre of object that cell determines or is responsible for detecting object. 
## To Evaluate accuracy of Bounding Boxes
- IoU (Intersection over Union) is used as a metric to evaluate the accuracy of predicted bounding boxes. 
- IoU is a measure of how well the predicted bounding box overlaps with the ground truth bounding box.<br>
It is defined as the ratio of the intersection area between the two boxes to the union area of the two boxes:

```IoU = Intersection / Union```

where "Intersection" is the area of overlap between the predicted and ground truth bounding boxes, and "Union" is the total area of the two boxes.
<img width="694" alt="Screenshot 2023-05-03 at 9 04 52 AM" src="https://user-images.githubusercontent.com/122048067/235828117-d499d8fa-0099-46d7-8074-69be76fd61e0.png">

## Non Max Suppression (NMS)
We use NMS to make sure that your algorithm detects your object only once. <br>
We are running image classification and localization algorithm on every grid cell, it is possible that many of the cells say that their ‘Pc’ Class Probability is highest.
So, what NMS does is that it cleans up other unwanted detections so we end up with one detection for particular object.
### How does this NMS work?
- First it looks for probabilities (Pc) associated with each of these detection for particular object
- It takes largest ‘Pc’ which is most confident detection for the object
- NMS part looks for all remaining bounding boxes and chooses all those bounding boxes which has high Intersection over Union (IOU) with the bounding box of highest ‘Pc’ and suppresses them.
- Then we look for remaining bounding box and find highest ‘Pc’ and again NMS looks for remaining bounding boxes which has high IOU with bounding box of high ‘Pc’ and then they will get suppressed.<br>

By doing this for every object we get only one boundng box for each object. <br>
i<img width="744" alt="Screenshot 2023-05-03 at 9 08 30 AM" src="https://user-images.githubusercontent.com/122048067/235828447-852b77dc-09f1-4f69-9bc5-d535e6eecc9b.png">
1. It takes largest Pc which is 0.9 in this case
2. It check IOU for all the remaining bounding boxes (i.e. for 0.6, 0.7 for Car 1 and 0.8, 0.7 for Car 2)
3. Now, NMS will suppress 0.6 and 0.7 for car 1 as they have high IOU with respect to bounding box of Pc=0.9, so like this we get only one bounding box for car 1 which is highlighted in the image.
4. Next, for remaining bounding boxes we have highest Pc=0.8 for car2 and again we check IOU for remaining boxes (i.e. 0.9 for car1 and 0.7 for car2)
5. Now, NMS will suppress 0.7 as it has high IOU with respect to bounding box of Pc=0.8. And we get only one bounding box for car 2 as well.

## Multiple objects in Single Cell
We use multiple anchor boxes for solving this,

Each cell represents this output (Pc, x, y, h, w, c1, c2, c3) which is a vector of shape (8, 1) i.e. 8 rows and 1 column. c1, c2, c3 are the different classes say person, car, bike.

So, the shape of bounding box can change depending on number of classes. <br>
<img width="898" alt="Screenshot 2023-05-03 at 9 11 42 AM" src="https://user-images.githubusercontent.com/122048067/235828728-6b0bcd41-dcd0-4c62-b9c2-aab59090e177.png">

Now, each cell won’t be able to output 2 detection so have to pick any one of the two detections to output.

With the idea of anchor boxes what you are going to do is predefine 2 different shapes called Anchor Box 1 and Anchor Box 2. By this we can do two predictions with 2 anchor boxes.

In general we can use more anchor boxes, to capture variety of shapes the object has.

So, for two anchor boxes how our output will be in case of 3 classes,

The output will be vector of size (16, 1) and the vector contains, (Pc1, x1, y1, h1, w1, c1, c2, c3, Pc2, x2, y2, h2, w2, c1, c2, c3)
## Combining All steps 
<img width="859" alt="Screenshot 2023-05-03 at 9 13 19 AM" src="https://user-images.githubusercontent.com/122048067/235828858-d98d05e4-a687-4f89-9ed7-f59d25356093.png">
