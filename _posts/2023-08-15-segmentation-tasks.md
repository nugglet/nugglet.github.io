---
title: 'Segmentation Tasks'
date: 2023-08-15
permalink: /posts/2023/08/segmentation_tasks
tags:
  - DL
  - AI
  - vision
excerpt_separator: "<!--more-->"
classes: wide

toc: true
toc_sticky: true
---


Segmentation is a popular vision problem that is concerned with analysis and object or scene understanding of the contents of a given input scene. Most commonly, this is used with images, but segmentation can also be performed on a range of datatypes such as videos or 2D and 3D point clouds. The ability of segmentation models to detect objects and identify their location makes these models incredibly useful in projects involving autonomous machines and mapping and localization.<!--more-->


## Types of Segmentation


There are a few types of segmentation tasks, each concerning a different level of granularity within it's classification objective. The main difference between these tasks are the types of datasets (particularly how they are labelled), and their end application use. This section provides a brief overview of the different kinds of segmentation: Semantic, Instance and Panoptic.

![image-center]({{ site.url }}{{ site.baseurl }}/post_images/2023-08/segtask_1.png){: .align-center}
<sub>Output comparisons between different visual perception networks (source: [University of Waterloo](https://wiki.math.uwaterloo.ca/statwiki/index.php?title=User%3AX93ma))</sub>



### Semantic Segmentation


Most prominent is Semantic Segmentation, which  performs pixel-wise classification on an image, and identifies the location of objects within the image through masks. This sets it apart from object detection, which only says 'This image has sheep and dogs, and maybe cats, but no horses', but does not tell you where these objects are. This is due to object detection models performing a sort of pattern matching (over a group of pixels) through learnt feature maps. 

A semantic segmentation model functions as such:
1.  Extract features in training data
2. Localize objects through bounding boxes
3. Training to group pixels by learning the segmentation mask of objects. 

In this way, objects are identified by local groups in an image, but does not distinguish individual instances of the actual object. Semantic segmentation is often used to study objects that are more amorphous.

Applications of Semantic Segmentation: 
1. Autonomous Driving: Detecting lanes, pedestrians etc.
2. Geo Sensing: Satellite imagery and monitoring areas of deforestation and urbanization
3. Medical: Detecting abnormalities in medical scans


### Instance Segmentation


Instance Segmentation combines object detection and semantic segmentation. It identifies all instances of a class, and can demarcate each object as separate instance of the same class. Compared to semantic segmentation, the output of instance segmentation is more detailed, able to recognize objects as sheep, and also able to distinguish each sheep as a separate sheep, not simply a blob of sheep pixels (which may in fact include multiple sheep). However, this also means that in essence, each sheep is treated as a separate sheep class on its own.

An Instance segmentation model functions as a combination of 2 methods:
1. Object Detection - find bounding boxes for every object
2. Semantic Segmentation - find object masks for each object

In implementation, there are 2 approaches:
1. Top-down: Learn semantic point features first, then group into separate instances
	- Uses approach similar to Mask R-CNN, where instances are detected as bounding boxes, then mask segmentation is performed on each box individually.
2. Bottom-up: Detect object instances then refine semantic mask
	- Uses Contrastive Learning; points are mapped to a high-dimensional feature space where features of the same instance are close together, and far apart from others.

Applications of Instance Segmentation: 
1. Medical: Identify tumors in MRI scans etc.
2. Satellite Imagery: Counting cars, detecting ships for maritime security etc.
3. Robotics: Self-supervised learning to segment observations into individual objects


### Panoptic Segmentation


[Panoptic Segmentation](https://arxiv.org/abs/1801.00868) is concerned with scene understanding, and combines semantic and instance segmentation. It identifies objects with respect to its class labels, and also identifies all individual instances of the object in the scene. These class labels includes both countable objects with properly defined geometry (i.e. people, cars, animals), as well as things that have no defined geometry but are easily identified by texture (i.e. sky, road, water).

A Panoptic Segmentation model functions as such:
1. Semantic Segmentation - Separate each object in the image into independent individual part, then label each part
2. Instance Segmentation - Classify the objects


![image-center]({{ site.url }}{{ site.baseurl }}/post_images/2023-08/segtask_2.png){: .align-center}
<sub>Comparison between panoptic segmentation and others (source: [Panoptic Segmentation](https://arxiv.org/abs/1801.00868))</sub>


## Summary


In general, the different visual perception tasks can be characterized as such:

| Image Classification/Recognition | Predict some class label(s) that describe the contents of input |
| Object Detection | Image classification + find bounding boxes of each object |
| Semantic Segmentation | Pixel-wise classification of input, then group pixels into objects. |
| Instance Segmentation | Object detection + semantic segmentation (find individual objects, then within each box, differentiate object from background and learn its mask) |
| Panoptic Segmentation | Semantic + instance segmentation |


## Links and Resources

- [Segmentation Models GitHub](https://github.com/qubvel/segmentation_models/tree/94f624b7029deb463c859efbd92fa26f512b52b8)
- [The Complete Guide to Panoptic Segmentation - V7Labs](https://www.v7labs.com/blog/panoptic-segmentation-guide)
- [Semantic Segmentation vs. Instance Segmentation: Explained - Roboflow](https://blog.roboflow.com/difference-semantic-segmentation-instance-segmentation/)
- [StatWiki - University of Waterloo](https://wiki.math.uwaterloo.ca/statwiki/index.php?title=User%3AX93ma)