---
layout: hub_detail
background-class: hub-background
body-class: hub
title: SqueezeNet
summary: Alexnet-level accuracy with 50x fewer parameters.
category: researchers
image: pytorch-logo.png
author: Pytorch Team
tags: [vision]
github-link: https://github.com/pytorch/vision/blob/master/torchvision/models/squeezenet.py
featured_image_1: squeezenet.png
featured_image_2: no-image
---

```python
import torch
model = torch.hub.load('pytorch/vision', 'squeezenet1_0', pretrained=True)
model = torch.hub.load('pytorch/vision', 'squeezenet1_1', pretrained=True)
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

Model `squeezenet1_0` is from the [SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size](https://arxiv.org/pdf/1602.07360.pdf) paper

Model `squeezenet1_1` is from the [official squeezenet repo](https://github.com/DeepScale/SqueezeNet/tree/master/SqueezeNet_v1.1).
It has 2.4x less computation and slightly fewer parameters than `squeezenet1_0`, without sacrificing accuracy.

Their 1-crop error rates on imagenet dataset with pretrained models are listed below.

| Model structure | Top-1 error | Top-5 error |
| --------------- | ----------- | ----------- |
|  squeezenet1_0  | 41.90       | 19.58       |
|  squeezenet1_1  | 41.81       | 19.38       |

### References

 - [Squeezenet: Alexnet-level accuracy with 50x fewer parameters and <0.5MB model size](https://arxiv.org/pdf/1602.07360.pdf).
