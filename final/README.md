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

Experiment 1: Average AP score was 

## d.



# Instructions for how to run code
1. Copy the occluded_data folder directly into your personal Google Drive, link: https://drive.google.com/drive/folders/1e8FtRnGTWx1TKdfpwNM9LfnyZ9QAQDMW?usp=sharing




2. Download the Colab notebook and run in Colab cell by cell (you will need to restart runtime after 2nd cell), link: 
3. Alternately, if the github colab file doesn't work, the notebook is also contained here: https://colab.research.google.com/drive/1P2YTDX2loffeIV7R9bhBUT6kB4ou2uX0?usp=sharing

# Consent 
To Adam Czajka, you have my consent to forward this work to Amazon Robotics. I don't think it's particularly high-quality work, but if you feel they would find it useful or interesting, please feel free to use it. 
