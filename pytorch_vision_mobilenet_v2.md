---
layout: hub_detail
background-class: hub-background
body-class: hub
title: MobileNet v2
summary: Efficient networks optimized for speed and memory, with residual blocks
category: researchers
image: pytorch-logo.png
author: Pytorch Team
tags: [vision]
github-link: https://github.com/pytorch/vision/blob/master/torchvision/models/mobilenet.py
featured_image_1: mobilenet_v2_1.png
featured_image_2: mobilenet_v2_2.png
---

```python
import torch
model = torch.hub.load('pytorch/vision', 'mobilenet_v2', pretrained=True)
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

The MobileNet v2 architecture is based on an inverted residual structure where the input and output of the residual block are thin bottleneck layers opposite to traditional residual models which use expanded representations in the input. MobileNet v2 uses lightweight depthwise convolutions to filter features in the intermediate expansion layer. Additionally, non-linearities in the narrow layers were removed in order to maintain representational power.

| Model structure | Top-1 error | Top-5 error |
| --------------- | ----------- | ----------- |
|  mobilenet_v2       | 28.12       | 9.71       |


### References

 - [MobileNetV2: Inverted Residuals and Linear Bottlenecks](https://arxiv.org/abs/1801.04381)
