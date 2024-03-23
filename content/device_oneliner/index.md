---
title: Setting device in Torch
description: Simplest method to have device specific Pytorch code
slug: device_oneline
date: 22-07-2023 00:00:00+0000
categories:
    - Python
    - Pytorch
tags:
    - python
    - torch
---

A simple onliner to set device in PyTorch. 
```python
import torch

device = (
    "cuda"
    if torch.cuda.is_available()
    else "mps"
    if torch.backends.mps.is_available()
    else "cpu"
)
print(f"Using {device} device")
```