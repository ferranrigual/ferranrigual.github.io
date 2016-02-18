---
layout: post
title:  "Visualizing ConvNet filters"
date:   2016-01-26 10:58:05
category: work
tags: work ai convnet
---

Understanding what happens within a convolutional network (convnet for short) is not trivial, but I think it would be very useful for obtaining better models.

I have an idea to visualize the filters of a convnet and I'm going to describe my process while implementing it.

## The objective

Being able to visualize the filters of a convnet in a way that a human can comprehend.

## The process

In a regular convnet there are several convolutional layers, each of them has a varying number of filters of different depths. Normally, the filters of the first layer have 3 channels and are easy to visualize mapping each of the channels to one of the R, G, B channels of an RGB image.

For the following layers the number of channels is rapidly increased, and thus visualizing them is difficult, since we would be looking to an array of an arbitrary number of matrices. Normally 32, 64 or 128 or any other `n^2`.

Here comes my idea: I want to visualize the second layer of filters by superimposing the filters of the first layer multiplied by it's response on the image.

*Now I think this doesn't make sense :( but let's try just to be sure.*

## Problems found

# Can't import Caffe in python

Check path and make sure `make pycaffe` has been executed in your caffe directory.

# Can't find model

Check model exists and path is correct. If model does not exist, run `python scripts/download_model_binary.py PATH_OF_MODEL_DIR`. The directory must include a `readme.md` file.


