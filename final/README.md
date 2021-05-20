# Report
## a. 

My reasoning for Colab/Detectron2/mask-rcnn remains relatively unchanged. 

Colab provides easy access to GPUs, easy environment configuration, and easy data access with Google Drive.

Detectron2/mask-rcnn has shown strong performance on diverse object sets and also occluded objects. The model is also resource-light as it can easily run on Colab. I also have learned that Detectron2 performs automatic data augmentation for training data, which is a plus.

## b.

I had already established a somewhat okay solution, so for this solution I wanted to push what could yeild much more interesting results. This was 3 specific experiments: 
1) Can training on pure object data work? 
2) Can training transfer across different lighting settings? 
3) Can mixing in (minimal) occluded object data improve performance?

Questions 1 and 2 are interesting because, if true, they would potentially simplify training data collection. Amazon has a vast amount of products, and photographing and labelling cluttered boxes is much less feasible than photographing and labelling single objects. 

Question 3 is interesting because, if some cluttered data is required to train the model to expect many and potentially overlapping images, can we minimize this cluttered data requirement?

Experiments performed:
1) training on pure object data and testing on top_view_top_light occluded data
2) training on pure object data and testing on top_view_ambient_light occluded data
3) training on mix of object data and top_view_ambient_light occluded data; testing on top_view_top_light occluded data

Pure object dataset was 100 images, 10 per object type. Each set of occluded data was 10 images. I restricted training epochs to 800. This trains quickly and I was getting INF/NAN gradient errors frequently above 800-900 training epochs on object data. 

## c.

Experiment 1: Average AP bounding box score was easily below 10, which is terrible. Most object identification included multiple objects in a single "object"; the model was clearly struggling at separating objects. The solution is likely not just more epochs. The architecture would likely need to change radically to have single object training data be effective for training occluded multiple-object scenes. Classification image results included in the Github.   

Experiment 2: Average AP was zero for everything. I suspect something was misformatted; it is also possible that the model failed to recognize any object above the 50% confidence threshold.

Experiemnt 3: This was a really interesting experiment! The average AP bounding box score hovered around 30, a significant improvement from experiment 1. What interests me the most is that the addition of labelled occluded training data was relatively small;adding 10 cluttered scenes to 100 objects. This small change shows the model much more easily distinguishing between objects, as well as getting a modest bump in class accuracy as well (without changing the software architecture!). Additionally, at the very least this shows that different lighting setups don't totally disqualify different sets of data, as this experiment's training data was ambient light and testing data was top light. 

My justification for mAP remains the same.

## d: accuracy commentary: 
The AP scores and results definitely feel like a step back from the midterm. However, the midterm's data was easily structured such that the training data closely mirrored the testing data. These experiments showed that pure object data is difficult to map well onto occluded scenes, but also that a small investment of labelled cluttered occluded data can modestly boost results. Lighting conditions are, at the very least, not required to be the same for useful training vs testing data. More epochs or different training parameters almost definitely aren't the larger solution. If using pure object data is the end goal, an architecture rewrite expecting to map partial object details into a cluttered space would be necessary. 

If I had more time, I would love to have explored the full range of lighting and angle conditions. Labelling takes forever, and I should've started sooner if I wanted to label that much data. I would also have liked to have played with the software architecture more. Detectron2 got good initial results when the training data mapped to the testing data, but poor results when mapping objects to scenes. Pattern recognition could also have played a larger role, as shiny handles, polka-dot febreeze containers, and weirdly-shaped headphones offer easy clues humans use to identify objects. 

# Instructions for how to run code
1. Copy the occluded_data folder directly into your personal Google Drive, link: https://drive.google.com/drive/folders/1e8FtRnGTWx1TKdfpwNM9LfnyZ9QAQDMW?usp=sharing
2. Download the Colab notebook and run in Colab cell by cell (you will need to restart runtime after 2nd cell), link: https://github.com/jhoeksem/cv2_project/blob/master/final/household_objects_occluded.ipynb
3. Alternately, if the github colab file doesn't work, the notebook is also contained here: https://colab.research.google.com/drive/1P2YTDX2loffeIV7R9bhBUT6kB4ou2uX0?usp=sharing
4. If you would like to try running the code with different datasets, the train and test data initialization boxes have commented pieces that you can turn on and off easily.

# Consent 
To Adam Czajka, you have my consent to forward this work to Amazon Robotics. I don't think it's particularly high-quality work, but if you feel they would find it useful or interesting, please feel free to use it. 
