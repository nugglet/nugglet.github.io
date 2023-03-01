---
title: 'Deep Learning Inference Frameworks and Why We Need Them'
date: 2023-02-28
permalink: /posts/2023/02/dl_inference_frameworks_and_why_we_need_them
tags:
  - MLOps
  - DL
  - AI
  - SWE
excerpt_separator: "<!--more-->"

toc: true
toc_sticky: true
---

There are two types of deep learning frameworks.
<!--more-->
1. Training frameworks: 
	These frameworks are commonly known for providing tools to design and build a machine learning or deep learning model. These libraries are feature-rich to allow you to define and train your model. The goal of development during model training would be to produce accurate results (within a reasonable time frame).
	e.g. Tensorflow, PyTorch, Caffe, Theano, MXNet etc.
	
2. Inference frameworks:
	Inference frameworks are a part of MLOps development, and are used to optimize your model during deployment. In AI, inference refers to a trained model making predictions. Assuming your model is satisfactorily accurate, inference frameworks would help you take the next step in speeding up the time your model takes to produce an accurate prediction. From an end-user UX perspective, this is incredibly important. 
	e.g. OpenVINO, ONNX, NCNN, MXNet etc.
	
In production, there are 2 main priorities with regards to model deployment:
1. Making fast predictions
2. Using minimal computing resources


## Optimising your Inference Framework

There are a number of great inference frameworks that work to optimize you model deployment. In the paper [_Deep Learning Inference Frameworks Benchmark_](https://arxiv.org/abs/2210.04323), Pierrick Pochelu proposes a number of metrics to benchmark the performance of your framework. These include:
1. Throughput (predictions per second)
2. Load time (time for nn to reach a state in memory that is ready to make predictions)
3. Memory consumption (memory on disk used to process the current batch of features)
4. Power consumption (watt-seconds of the system to predict a fixed number of data samples)

Section V (Experimental Results) of the paper further expounds on the details and significance of these metrics, and what to look out for in greater nuance. 

However, for absolute beginners, in general you would want to:
1. Minimize load time (fast model happy users!)
	- How: This is hardware dependent. You could use a fast service setup (i.e. elastic cloud), and/or try caching
2. Minimize inference speed (reduces power consumption and, again, fast model happy users!)
	- How: See section V part A of the paper linked above
3. Minimize memory consumption if you need to manage 

	(a) more models &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(b) a more complex model


## Selecting an Inference Framework for your Project
The following list features just a few of the more popular inference frameworks.

### [ncnn](https://github.com/Tencent/ncnn) (Tencent)

ncnn is an inference framework optimized for mobile devices. Hence, the framework is static, lightweight and does not use any third party dependencies (except [OpenMP](https://www.openmp.org)), which makes it easy to package and ship. ncnn is most commonly used with CNN networks for a wide variety of tasks, such as detection, segmentation and pose-estimation. It is also good with light-weight models, presumably for embedded or mobile devices.

Cons: ncnn was [reported](https://github.com/Tencent/ncnn/issues/2750) to use more memory than other inference frameworks.

### [OpenVINO](https://docs.openvino.ai/) (Intel)

OpenVINO (Open Visual Inference and Neural Network Optimization) is a framework optimized for Intel hardware and computer vision tasks, and is able to run inferences on both CPU and specialized intel devices (e.g. intel GPU, FPGA, VPU etc.). This is a comprehensive toolkit for quickly developing applications related to computer vision, and was developed with a focus on IOT/Edge devices. 

Cons: OpenVINO works with [Intermediate Representation](https://docs.openvino.ai/2021.4/openvino_docs_MO_DG_prepare_model_convert_model_Converting_Model.html) (IR) format models. Converting your `.pb`, `.h5`, `.pkl` (or whatever other ML file format) to the OpenVINO IR format may cause a change in model performance. OpenVINO also only works with Intel hardware (so RIP to your RTX 4080).

> Note: I know next to nothing about IOT/Edge, so perhaps you may want to check out [this](https://towardsdatascience.com/a-quick-intro-to-intels-openvino-toolkit-for-faster-deep-learning-inference-d695c022c1ce) article for a better explanation of OpenVINO.


### [TensorRT](https://developer.nvidia.com/tensorrt) (NVIDIA)

TensorRT is like the NVIDIA version of OpenVINO, made to optimize inference of TF models on NVIDIA GPUs. TensorRT is built on CUDA, and can perform up to 36X faster than CPU-only inference platforms. TensorRT is incredibly fast as it allocates memory for each tensor only for the duration of its usage, avoiding memory allocation overhead, which results in more efficient memory management and execution. [This](https://medium.com/@abhaychaturvedi_72055/understanding-nvidias-tensorrt-for-deep-learning-model-optimization-dad3eb6b26d9) article provides an excellent explanation of how TensorRT works.

Cons: Does not work with CPUs or non-NVIDIA hardware


### [ONNX](https://onnx.ai) (Microsoft)

ONNX Runtime is a cross-platform inference and training accelerator. It boasts the most backend acceleration options of any framework (to date) and is designed to boost interoperability. ONNX's community of partners means that the `.onnx` format is supported in many prominent training libraries (e.g. TensorFlow, PyTorch, SKLearn, MXNet, MATLAB, Chainer, SparkML etc.), unlike OpenVINO (Why IR intel? sad). This standardized file format also means that development and deployment stage models can be differentiated, and further allows the model to be easily deployed across a wide range of hardware (e.g. CPU, GPU, FPGA, VPU , Mobile etc.)


### [MXNet](https://mxnet.apache.org/versions/1.7/api/python/docs/tutorials/deploy/inference/index.html) (Apache)

You may have noticed MXNet was put under both training and inference frameworks earlier in this article. In general, MXNet is usually considered a training framework despite it offering inference tools. This is mostly due to its inference capabilities being outperformed by many other frameworks when handling larger datasets. However, MXNet is often used (especially by Amazon) in production for its development speed, portability (on hardware), and scalability (to multiple GPUs, to allow more complex models and handling bigger datasets). Compared to other frameworks, MXNet is user friendly and relatively easy to pick up. MXNet is also flexible, and supports many imperative and declarative languages. On GPU, MXNet offers faster calculation and better resource utilisation than Tensorflow.

Cons: Performance drops off on CPU compared to other frameworks, and has a smaller active community so support and updates to MXNet are less than others.