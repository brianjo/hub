---
layout: hub_detail
background-class: hub-background
body-class: hub
title: GoogLeNet
summary: GoogLeNet was based on a deep convolutional neural network architecture codenamed "Inception" which won ImageNet 2014.
category: researchers
image: pytorch-logo.png
author: Pytorch Team
tags: [vision]
github-link: https://github.com/pytorch/vision/blob/master/torchvision/models/googlenet.py
featured_image_1: googlenet1.png
featured_image_2: googlenet2.png
---

```python
import torch
model = torch.hub.load('pytorch/vision', 'googlenet', pretrained=True)
model.eval()
```

All pre-trained models expect input images normalized in the same way,
i.e. mini-batches of 3-channel RGB images of shape `(3 x H x W)`, where `H` and `W` are expected to be at least `224`.
The images have to be loaded in to a range of `[0, 1]` and then normalized using `mean = [0.485, 0.456, 0.406]`
and `std = [0.229, 0.224, 0.225]`.

Here's a sample execution.

```python
# sample execution (requires torchvision)
from PIL import Image
from torchvision import transforms
input_image = Image.open('dog.jpg')
preprocess = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
])
input_tensor = preprocess(input_image)
input_batch = input_tensor.unsqueeze(0) # create a mini-batch as expected by the model
output = model(input_batch)
# Tensor of shape 1000, with confidence scores over Imagenet's 1000 classes
print(output[0])
# The output has unnormalized scores. To get probabilities, you can run a softmax on it.
print(torch.nn.functional.softmax(output[0], dim=0))

```


### Model Description

GoogLeNet was based on a deep convolutional neural network architecture codenamed "Inception", which was responsible for setting the new state of the art for classification and detection in the ImageNet Large-Scale Visual Recognition Challenge 2014 (ILSVRC 2014). The 1-crop error rates on the ImageNet dataset with a pretrained model are list below.

| Model structure | Top-1 error | Top-5 error |
| --------------- | ----------- | ----------- |
|  googlenet       | 30.22       | 10.47       |



### References

 - [Going Deeper with Convolutions](https://arxiv.org/abs/1409.4842)
