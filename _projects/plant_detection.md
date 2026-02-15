---
title: "Plant Detection"
permalink: /projects/plant_detection
layout: single
classes: wide
---

[Spectroscopy Prediction (Github)](https://github.com/AkkuRam/plant-detection)

## Dataset

This dataset was obtained from Kaggle, contained over 50k images. Moreover, for the
model training this dataset was split into training (~36k) and test images (~17k).

- https://www.kaggle.com/datasets/sebastianpalaciob/plantvillage-for-object-detection-yolo/data#


## Task

- The main task was to perform object detection to identify 38 different leaf types
- The ground truth bounding boxes, which were the YOLO annotation were given in the format
    - (centerx, centery, width, height)

In terms of preprocessing, the images were resized to 256x256 pixels, where Gaussian Blur was applied 
to preprocess the salt/pepper noise present in the image. Moreover, the images were normalized as a final step.

## Model

```python
self.regressor = 
nn.Sequential(
    nn.Linear(base_model.fc.in_features, 256),
    nn.ReLU(),
    nn.Linear(256, 128),
    nn.ReLU(),
    nn.Linear(128, 64),
    nn.ReLU(),
    nn.Linear(64, 32),
    nn.ReLU(),  
    nn.Linear(32, 4),
    nn.Sigmoid()
)
```

## Running the file

- To run the file execute this command in the terminal "python -m src.train_pipeline"


