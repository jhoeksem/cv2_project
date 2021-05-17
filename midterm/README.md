1) 

Sources: 
https://colab.research.google.com/github/Tony607/detectron2_instance_segmentation_demo/blob/master/Detectron2_custom_coco_data_segmentation.ipynb
https://towardsdatascience.com/how-to-train-detectron2-on-custom-object-detection-data-be9d1c233e4
 
I used Detectron2, which is similar to a mask-rcnn design. Mask-RCNN has two main steps. The first is region proposal, where the network predicts where objects might be and assigns them bounding boxes. The second is classification, where the network predicts which class is contained in a given bounding box. Only predictions with a high enough confidence are kept for the final prediction. 
 
Mask-RCNN has been shown in the past to have strong performance on varied object data and occluded sets of data. This is pronounced in Cityscapes datasets and many COCO datasets. Mask-RCNN is also small enough to run on Google Colab free GPUs in under an hour and receive relatively good results. 
 
No data augmentation was used at this stage. Using LabelMe, 39 uncluttered images were annotated for a training dataset and 6  uncluttered images were annotated for a test dataset. 

