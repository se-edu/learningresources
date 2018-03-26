# Introduction to computer vision (CV)
Authors: [Nguyen Quoc Bao](https://github.com/bqnguyen94)

* [What is CV](#what-is-cv)
   * [Applications of CV](#applications-of-cv)
   * [Typical CV tasks](#typical-cv-tasks)
* [Core problems in CV](#core-problems-in-cv)
   * [Image Transformations](#image-transformations)
   * [Image Classification](#image-classification)
   * [Object Detection](#object-detection)
   * [Object Tracking](#object-tracking)
* [GPU-accelerated CV](#gpu-accelerated-cv)
* [What's next](#whats-next)
* [References](#references)


## What is CV
Computer vision (CV) is a field of study of computer science concerning with the theories and technologies in building computer systems that can derive useful information from visual data. CV is a prominent field of study nowadays as it allows computers to autonomously solve problems that otherwise require human sight.<sup>[[1]](#footnote1)</sup>

In computer vision, an image is represented by a number matrix, or a set of matrices, with each number in the matrix corresponds to the color value or intensity value of a pixel in the image.<sup>[[2]](#footnote2)</sup> With this representation, linear algebra can be exploited for many CV operations from the most basic like transformation to very complex like feature extraction and motion tracking.

Additionally, with deep learning, a lot of new applications of computer vision have been introduced, including facial recognition, and object detection that is widely used in self-driving cars.<sup>[[3]](#footnote3)</sup>

### Applications of CV
Many systems and applications rely on computer vision, as they work extensively with image and video input for:

- Navigation, e.g. autonomous cars to keep track of the road, detect pedestrians, and avoid collision with other vehicles.<sup>[[3]](#footnote3)</sup>
- Facial Detection, e.g. smartphones' camera software to focus on faces.<sup>[[4]](#footnote4)</sup>
- Visual Surveillance, e.g. detecting movements in security footage.<sup>[[4]](#footnote4)</sup>
- Automatic Inspection, e.g. measuring structures, modelling environment.<sup>[[4]](#footnote4)</sup>
- Searching, e.g. image search engines.<sup>[[5]](#footnote5)</sup>

### Typical CV tasks
For a typical computer vision system, the tasks it aims to perform may include, but are not limited to, the following:<sup>[[6]](#footnote6)</sup>

- Geometric Image Transformation
- Image Filtering
- Image Classification
- Structural Analysis and Shape Descripting
- Feature Detection
- Object Detection
- Object Localization
- Object Tracking

## Core problems in CV
Most problems requiring computer vision can be boiled down to one or a combination of the following core problems: image transformation, classification, localization, detection, and tracking.<sup>[[6]](#footnote6)</sup>

### Image Transformations

In many applications, image transformation is often the first step that standardizes the input and allows the application to subsequently perform more complex analysis.

![CamScanner](image_transformation.png "CamScanner Image Wrapping")

*[Affine Transformation](https://www.camscanner.com/)*

The most common transformations are scaling - resizing of image, translation - shifting of selected zones in image, rotation of image for an angle, and affine transformation - a combination of scaling, rotating, and translation, that reserves parallel lines in the original image.<sup>[[7]](#footnote7)</sup>

### Image Classification

Image classification refers to the task of identifying the object present in an image. Typically, a predetermined set of possible objects is given and the classification of the image is the object that is most likely to exist in it.<sup>[[6]](#footnote6)</sup>

![Classification](image-classification-1.png "Cat Classified")

*[source](http://cs231n.github.io/classification/)*

Image classification is often paired with object localization, which is the task of finding where in the image the object is. Many more complex problems in computer vision, such as object detection and segmentation, can be reduced to image classification and localization.<sup>[[6]](#footnote6)</sup>

![Classification](image-classification-2.png "Cat Localized")

*[Fei-Fei Li, Andrej Karpathy & Justin Johnson (2016) cs231n, Lecture 8 - Slide 8, Spatial Localization and Detection (01/02/2016)](http://cs231n.github.io/classification/)*

At the moment, there already exist classification/localization models that surpass trained humans. For example, in the 2014 ImageNet Large Scale Visual Recognition Challenge (ILSVRC),
> Our results indicate that a trained human annotator is capable of outperforming the best model (GoogLeNet) with approximately 1.7% chance (p = 0.022).<sup>[[8]](#footnote8)</sup>

### Object Detection

Object detection, as the name suggests, detects objects within an image. It combines image classification and localization in a sense that it gives the classification and bounding box (localization) of object in an image, but it does this to all instance of every known type of objects found in the image, instead of assuming only one type is present.<sup>[[9]](#footnote9)</sup>

![Detection](object-detection.png "Object detected")

*[source](https://github.com/tensorflow/models/tree/master/research/object_detection)*

Obviously, object detection requires tremendous computing power compared to classification/localization; a naïve approach is to repeatedly classify and localize for every pixel in the image. In recent years, breakthroughs have been made towards a quicker and more efficient detecting algorithms, such as sharing computation on a whole image as can be seen in YOLO, SSD, and R-FCN. Notably, in 2016, the YOLO system named 'YOLO9000' was able to achieve real-time detection on motion pictures, and even able to detect object categories that it had never been trained for.<sup>[[9]](#footnote9)</sup>

### Object Tracking

Object tracking is the process of following and establishing the movement of a specific object of interest in a sequential set of images - typically a video clip or a real-time camera feed.<sup>[[10]](#footnote10)</sup> It is crucial for autonomous driving systems such as self-driving cars or unmanned aerial vehicles (UAVs).

![Tracking movement of cars with OpenCV](object-tracking.png "Object tracked")

*[Tracking movement of cars with OpenCV](https://docs.opencv.org/3.3.1/)*

Object tracking algorithms can be divided into 2 categories: those applied to still backgrounds, and those to moving image frames such as those from a car camera.<sup>[[6]](#footnote6)</sup> Algorithms in the former category typically compare subsequent images, extract the difference and deduce the movements, if any. Notable in this category are Optical Flow and the Waterfall algorithms.<sup>[[6]](#footnote6)</sup> Moving image sources on the other hand require the tracking system to learn the relationships between each object's motion and the surroundings.<sup>[[4]](#footnote4)</sup> In any case, all tracking systems require an initial detection of the object to track.<sup>[[6]](#footnote6)</sup>

## GPU-accelerated CV

For computer vision systems, digital images are often translated into matrices, whose nature depends on the chosen color presentation. For example, an image in the RGB (Red, Green, Blue) color space will be presented by 3 matrices, each contains the color intensity of the individual pixels with regards to red, green, or blue. There are other color representations, such as the CMYK or the HSV that annotates the pixels' Hue, Saturation, and Value.<sup>[[1]](#footnote1)</sup>

Underneath, all computer vision algorithms deal with images as matrices, thus employ theories in Linear Algebra to process images. In computer hardware, there is a class of computing units that specializes in matrix operations called the Graphics Processing Units (GPUs) - their main purpose is to speed up heavy graphical tasks such as video rendering, gaming, or 3D modelling. Therefore, GPUs are well-suited to serve as computer vision computing units.<sup>[[11]](#footnote11)</sup>

Most prominent computer vision libraries such as OpenCV, dlib, or VisionWorks support the use of GPUs in the computing process. For example, OpenCV has APIs that allow a kernel process running on the CPU to transfer image matrices to one or more GPUs to perform heavy functions such as Gaussian filtering or image stitching.<sup>[[12]](#footnote12)</sup> Hardware manufacturers have even tried to ease the image transferring process between CPU and GPU, by introducing system memory regions accessible to both CPU and GPU.

## What's next

There is an abundance of resources to learn and apply computer vision; however, not all of them are free or beginner-friendly.  The following are some great courses and tutorials freely accessible and aim for the masses:

Before diving into computer vision, it is recommended that learner acquire basic familiarity in Python, C++, or Java, as most tutorials and courses in CV use one of those languages:

[Python For Beginners](https://www.python.org/about/gettingstarted/)

[Learn C++](http://www.learncpp.com/)

[Java Tutorial](https://www.tutorialspoint.com/java/index.htm)

It is also advisable that learner has some basic understandings in Linear Algebra.

[Linear Algebra by Khan Academy](https://www.khanacademy.org/math/linear-algebra)

**Computer vision:**

This Udacity course pairs theoretical parts with very practical hands-on exercises:
[Udacity: Introduction to computer vision
by Georgia Tech](https://www.udacity.com/course/introduction-to-computer-vision--ud810)

A great blog from a self-taught computer vision developer:
[PyImageSearch](https://www.pyimagesearch.com/start-here-learn-computer-vision-opencv/)

Learning from those who implemented the wheel is always a good idea: [OpenCV Tutorials for C++](https://docs.opencv.org/3.4.0/d9/df8/tutorial_root.html)
or the Python version: [OpenCV Tutorials for Python](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_tutorials.html)

## References

<a name="footnote1">[1]</a>: https://en.wikipedia.org/wiki/Computer_vision<br />
<a name="footnote2">[2]</a>: https://courses.cs.washington.edu/courses/cse576/book/ch2.pdf<br />
<a name="footnote3">[3]</a>: https://blogs.nvidia.com/blog/2017/07/23/future-of-computer-vision/<br />
<a name="footnote4">[4]</a>: http://web.stanford.edu/class/cs231m/<br />
<a name="footnote5">[5]</a>: https://cloud.google.com/vision/<br />
<a name="footnote6">[6]</a>: http://cs231n.stanford.edu/<br />
<a name="footnote7">[7]</a>: https://www.cis.rit.edu/class/simg782/lectures/lecture_02/lec782_05_02.pdf<br />
<a name="footnote8">[8]</a>: http://karpathy.github.io/2014/09/02/what-i-learned-from-competing-against-a-convnet-on-imagenet/<br />
<a name="footnote9">[9]</a>: http://image-net.org/challenges/LSVRC/2016/%23det<br />
<a name="footnote10">[10]</a>: https://en.wikipedia.org/wiki/Video_tracking<br />
<a name="footnote11">[11]</a>: http://www.gipsa-lab.grenoble-inp.fr/summerschool/gpu/fichiers/GIPSA_talk.pdf<br />
<a name="footnote12">[12]</a>: https://docs.opencv.org/2.4/modules/gpu/doc/introduction.html<br />
<a name="footnote13">[13]</a>: https://devblogs.nvidia.com/beyond-gpu-memory-limits-unified-memory-pascal/<br />
