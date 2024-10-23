## Explainable Deep Learning

### Introduction
This repository consists of a notebook which uses Grad-CAM to evaluate the ResNet50 pretrained model for each layer of the model. The images used come from a small version of the ImageNet library. For a given label, this will output the Grad-CAM overlayed images for each image of that label (or labels) in the dataset.

### Hypothesis Being Tested
$ H_0 $: Deeper layers of the ResNet50 model do not focus on more important and localized aspects of the image relating to the label being tested

$ H_1 $: The deeper layers of the ResNet50 model focuses on features of images more relating to the label being tested as opposed to shallower layers

### Approach
The approach used to test this hypothesis is to load in the TinyImageNet library and the pretrained ResNet50 model. We then assign a label to retrieve test images (in the case below we use the label 'wallaby' which contains 5 unique images of the animal), and for each layer of the model and for each of the images we run Grad-CAM and overlay the resulting Grad-CAM mask on top of the original image. Once complete, visual analysis of each image is performed to assess if Grad-CAM reports that deeper layers of the model focus on the important aspects of the image. For statistical analysis, we calculate the ratio of pixels in the heatmap produced by Grad-CAM that have a value > 0.85 and run the Kruskal-Wallis Test to calculate the p-value to determine if there's a statistical significance between the layers. The Kruskal-Wallis Test was chosen over Anova due to the ratio data violating the assumption of normality required by Anova.

### Findings
- Layer 1: The heatmap is very sparse, randomly scattered amongst images and rarely contains any yellow or red regions. While in some images it is able to pick up on features relating to the animal, the layer tends to also pick up noise in the background.
- Layer 2: The model is still picking up noise from the background, but the heatmap has become stronger (showing more red regions than the previous layer). The heatmap also highlights more regions relating to important features in images and, for some images, the mask looks almost like an outline of the animal. 
- Layer 3: In this layer the heatmap begins to become more localized. However, some images still focus mainly on the background and noise.
- Layer 4: This layer shows the most localized results. Almost all images have the heatmap centered on the subject of the image itself.
- Inspection of the box plots after running the Grad-CAM over the dataset shows a general increase in pixel ratios with values > 0.85 in the heatmap for deeper layers which validates the alternative hypothesis. 
- Results: It is clear visually that the deeper layers of the ResNet50 model do in fact focus on more important features of the images. Thus we reject the null hypothesis.
