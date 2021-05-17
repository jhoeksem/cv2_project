# How to run code: 

1. Copy the uncluttered_data folder directly into your personal Google Drive, link: https://drive.google.com/drive/folders/1R6eReUvs1xZ4-DeDnIIPn8j9wYhdiuW9
2. Download the Colab notebook and run in Colab cell by cell (you will need to restart runtime after 2nd cell), link: https://github.com/jhoeksem/cv2_project/blob/master/midterm/household_objects_uncluttered.ipynb

# Report Questions:

1) 

Code sources I drew heavily from: 
- https://colab.research.google.com/github/Tony607/detectron2_instance_segmentation_demo/blob/master/Detectron2_custom_coco_data_segmentation.ipynb
- https://towardsdatascience.com/how-to-train-detectron2-on-custom-object-detection-data-be9d1c233e4
 
I used Detectron2, which is similar to a mask-rcnn design. Mask-RCNN has two main steps. The first is region proposal, where the network predicts where objects might be and assigns them bounding boxes. The second is classification, where the network predicts which class is contained in a given bounding box. Only predictions with a high enough confidence are kept for the final prediction. 
 
Mask-RCNN has been shown in the past to have strong performance on varied object data and occluded sets of data. This is pronounced in Cityscapes datasets and many COCO datasets. Mask-RCNN is also small enough to run on Google Colab free GPUs in under an hour and receive relatively good results. 
 
2) 

No data augmentation was used at this stage. Using LabelMe, 39 uncluttered images were annotated for a training dataset and 6  uncluttered images were annotated for a test dataset.

I designed my experiments such that i could train in the most lighting and orientation conditions with the least data. This was partly to minimize the annotation burden to myself, but also to see if I could get good classification results with training images. 

I selected 2 test images from each set of objects such that each set of objects would have an illuminated example and an ambient light example, and such that there would be a view from the side and a view from above. As the training data covered the full range of side view, top view and of top light, side light, ambient light, I was hoping that the test data would also reflect that diversity. This was to see if the model was able to accurately classify the full range of conditions. 

I varied epoch length to test the amount of training necessary to get robust results. 

The results are as follows: 
- training for 100 epochs sometimes fails to place a bounding box on objects (I had a .5 certainty score threshhold) 
- even training for as little as 100 epochs yeilded the correct class labels almost every time
- a single object would often have multiple bounding boxes (usually of the same class label). This problem almost completely goes away in the 1000-3000 epoch range
- bounding boxes would often not fully cover the edge of objects. This problem persisted into the 1000 epoch range, but mostly goes away after about 3000 epochs

To summarize the midterm questions:
1. Uncertain, I didn't use data augmentation or specifically test how little of an object's image is necessary for classification
2. I used only 39 images of uncluttered objects and achieved robust class labels and relatively robust bounding boxes by the 1000-3000 epoch range
3. The weakest property would be illumination. Dark spots of an object would often blend into the shadows and not be correctly classified as object pixels. 
4. Surprisingly minimal! Class labels are quite accurate given minimal (100 epochs) training and approach 100% after 1000 epochs. Bounding boxes take more work, and objects can be doubly-covered by multiple bounding boxes at the 100-300 epoch range. 1000 epochs returned good results.

3)

Total loss, class loss, segmentation AP, bounding box AP, and more data can be found in the document midterm_data.pdf in this repository.

After 1000 epochs, bounding box AP was 76.3% and segmentation AP was 84.8%. No class segmentation AP was below 80%.

I went with mAP because COCO provides easy methods for calculating this, and because the measure is a convenient way of distilling multiple IOU values into a single number.

4)

I'm quite happy with the given accuracay for class labels given enough training epochs. The biggest gaps I see are incorrect bounding boxes that slice off edges of images and of dark spots of object the model doesn't correctly segment as objects. The bounding box issue seems to go away with massive training. Dark spots could likely be improved by feeding the model more data so it gets a better feel the outlines of shapes, and uses that in its calculations.

There is also an issue of poor fine-grained borders on object segmentation (the borders look wavey/bumpy and don't closely hug the edge of the objects). This could partly be to my own human error of imperfect labelling. This issue could potentially be corrected by augmenting data to only focus on the border between objects and background. 
