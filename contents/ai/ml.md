# Introduction to Machine Learning (ML)
Authors: [Alex Fong](https://github.com/alexfjw)

* [What is ML](#what-is-ml)
   * [Types of ML tasks](#types-of-ml-tasks)
   * [Types of ML algorithms](#types-of-ml-algorithms)
   * [Types of Data](#types-of-data)
* [Machine Learning (Prototyping to Production)](#machine-learning-prototyping-to-production)
   * [Basic Data Preprocessing](#basic-data-preprocessing)
   * [Partitioning of Data](#partitioning-of-data)
      * [Test Set](#test-set)
      * [Validation Set](#validation-set)
      * [Training Set](#training-set)
   * [Model Training, Evaluation &amp; Data Analysis](#model-training-evaluation--data-analysis)
   * [Production](#production)
   * [Tips and Tricks](#tips-and-tricks)
   * [Concluding Remarks](#concluding-remarks)

## What is ML
Machine learning is a subfield in artificial intelligence whereby computers learn from data to perform a task. Machine learning algorithms are powerful because they can discern complex patterns within data, and utilize them to model unknown functions and produce desired outputs. 

An example would be machine translation, whereby the function modelled takes in text in one language and outputs text in another. With data, machine learning algorithms can approximate functions to translate text.  

A machine learning algorithm is also referred to as a 'model', after the algorithm is provided data to learn from.

### Types of ML tasks
Machine learning algorithms perform well for a large variety of tasks, from computer vision to natural language processing. Here is a non-exhaustive list:

- Sentiment analysis ([NLTK example on Kaggle](https://www.kaggle.com/ngyptr/python-nltk-sentiment-analysis?scriptVersionId=904608))
- Machine translation ([NMT](https://github.com/tensorflow/nmt), by Google)
- Image generation ([SuperRes](https://github.com/alexjc/neural-enhance), [Texture Synthesis](http://bethgelab.org/deeptextures/))
- Voice recognition, generation ([DeepSpeech](https://github.com/mozilla/DeepSpeech))
- Noise suppression ([RNNoise](https://people.xiph.org/~jm/demo/rnnoise/))
- Optical Character Recognition ([Tesseract-OCR](https://github.com/tesseract-ocr/tesseract))
- Data analytics (identifying trends, predicting sales)
- Recommender systems (recommending products based on user profiles)
- Instance segmentation (object detection & recognition to the pixel level [MaskRCNN](https://github.com/matterport/Mask_RCNN)) 
- Classification (often used for document indexing and retrieval)

### Types of ML Algorithms
There are two broad categories of machine learning algorithms, supervised learning and unsupervised learning. 

In supervised learning, the data used for training is accompanied with the desired output labels. For the case of machine translation, the input might be a word in the source language. The desired output is then the corresponding word in the target language. 

Learning is 'supervised' as the algorithm is told whether it has made a correct prediction.
Consequently, all training data must be labelled, and the labelling process may be costly.
Platforms like [Amazon Mechanical Turk](https://www.mturk.com/) are used for manual labelling of data.  

One simple use case is image classification, to match the input image to a known label.

<img src="https://i.imgur.com/mKjIS0C.png" width="500">  
(samples from [cifar10 dataset](https://www.cs.toronto.edu/~kriz/cifar.html))

Unsupervised learning algorithms do not require labels but it is difficult to explain the relationship between the items in each group.  
This approach can be used by e-commerce sites to identify similar products, where a clear and interpretable label for similar products is not required.

<img src="https://cdn-images-1.medium.com/max/900/1*xTvsgpDfja05SRMt-H5ylA.png" width="500">  
(T-SNE of Products Shape and Colour by [Eddie Bell](https://twitter.com/ejlbell/status/698309469965516800))

These different types of algorithms provide solutions to different problems. 

Popular resources for learning ML algorithms:
- Machine Learning: [Stanford CS229](cs229.stanford.edu/)
- Convolutional Neural Networks for Visual Recognition: [Stanford CS231n](http://cs231n.stanford.edu/)
- Natural Language Processing with Deep Learning: [Stanford CS224n](cs224n.stanford.edu/)
- A practical approach towards neural networks: [University of San Francisco, FastAI](http://www.fast.ai/)

Note that there is one other category of ML algorithms, called reinforcement learning. 
Reinforcement learning is quite different from the above mentioned categories and will not be discussed in this introductory piece. More information on Reinforcement Learning can be found in this following article on Medium. ([link](https://hackernoon.com/reinforcement-learning-part-1-d2f469a02e3b))

### Types of Data
Data is broadly split into 2 categories, structured and unstructured. 

Structured data refers to data in a standardized format with categories and values. Structured data tends to be formatted like the data in relational databases. A good example would be sales records, with fields like date, quantity sold, location and such.  

Unstructured data refers to data without a predefined data model, such as images, audio and textual data.

The distinction between the types of data is important as ML algorithms are not always compatible with both data types. [Decision trees](https://en.wikipedia.org/wiki/Decision_tree) for instance cannot be used on unstructured data. The data must be modified to into a structured form for decision tree related algorithms.

## Machine Learning (Prototyping to Production)
Bringing a machine learning algorithm to production requires a workflow which differs greatly from that of software engineering. This is due to a focus on prototyping.   

Prototyping is required as machine learning algorithms vary in efficacy when used in different domains. Machine learning algorithms model unknown functions and this modelling is never 100% correct. Statistical analysis is used to evaluate the performance of the algorithms.

The quantity, quality and type of data differs greatly across domains, sometimes containing errors such as corruption or mislabelling. Consequently, data is often pre-processed.

The algorithms contain various parameters that can be tuned to improve efficacy. Experimentation is required to discover the algorithm and parameters which give the best performance.

The entire prototyping process is similar to scientific experiments, where many model configurations and differently data is experimented on. Practitioners come up with a hypothesis on whether configuration may improve performance, and verify if performance is as expected.

### Prototyping Platform and Tools
The prototyping phase in ML is usually done on [Jupyter notebooks](http://jupyter.org/), or [RStudio](https://www.rstudio.com/products/rstudio/). The highlight of programming languages like Python and R, as well as mentioned accompanying software is their strong support for experimentation and visualization of results. 

Interpreted languages such as Python and R speed up prototyping iterations as they are less verbose than compiled languages like Java or C. 
Visualizations such as bar charts, graphs or just displaying the data assist in analysis and sharing of findings. 

Code style, clarity and maintainability are less of a priority at this stage. A good example of Juptyer notebooks in use can be found in the following link from Kaggle. ([link](https://www.kaggle.com/pmarcelino/comprehensive-data-exploration-with-python))

### Prototyping Workflow
Prototyping can be broken down into the following phases:  
1) Basic data preprocessing  
2) Partitioning of data   
3) Model training, evaluation and data analysis  
4) Repeating step 3  

### Basic Data Preprocessing
Basic data preprocessing is required to remove unwanted noise from the data. Examples of unwanted noise are repeated content and corrupted images. The unwanted content is usually discarded, manually or through aid of scripting. Training a model on such data hinders its ability to learn the actual patterns in uncorrupted data. 

Other areas of concern are class imbalance (for supervised learning), where there are more datapoints in one class as compared to another.

Statistics of the data are collected to identify class imbalance. This [paper](https://arxiv.org/abs/1710.05381v1) details various techinques for tackling class imbalance when using Convolutional Neural Networks. The literature review section in the paper links to techniques effective for other machine learning algorithms.

### Partitioning of Data
Data is usually split into 3 sets after preprocessing: the test set, validation set and training set. 

The FastAI ML MOOC is a great source of information on data partitioning.  
(MOOC is in unofficial release at time of writing)   

<img src="https://dziganto.github.io/assets/images/train-validate-test.png?raw=true" width="500"/>  
(by [David Zigano](https://dziganto.github.io/cross-validation/data%20science/machine%20learning/model%20tuning/python/Model-Tuning-with-Validation-and-Cross-Validation/))

#### Test Set
ML algorithms are never 100% accurate. Testing ML algorithms on real world data is essential for estimating their efficacy. 
Furthermore, ML models run the risk of overfitting the data they were trained on.  

Overfitting is a modelling error which occurs when models learn patterns unique to a subset of data. 
A model which overfits training data performs well on training data but is unable to translate this performance to real world data.
For a more thorough explanation of noise and overfitting, see the following link. ([link](https://elitedatascience.com/overfitting-in-machine-learning))

A test set solves the above problem. A test set is created by partitioning the available data, with size dependent on the total amount of data available. The test set is used only for testing model performance, and will not be touched during model training. The test set should also resemble real world data as much as possible to be a good indicator of real world performance.

Different schemes for partitioning must be used for data with different characteristics. A good article on splitting test and validation sets (discussed below) can be found in the following link. ([link](http://www.fast.ai/2017/11/13/validation-sets/))  

#### Validation Set
A validation set is then created from the remaining data in a similar fashion as the test set. The validation set is used to evaluate the performance of adjusting model parameters. This process of adjusting model parameters and verifying performance is conducted on the validation set to prevent overfitting the test set.

Guidelines on picking a size for the validation set can be found in Lesson 7 of FastAI's ML MOOC. (MOOC is in unofficial release at time of writing)

The following driving factors are suggested for deciding the validation set size.  

- Business Concerns   
4 mistakes in fraud detection is a much bigger issue than 4 mistakes in [cucumber classification](https://cloud.google.com/blog/big-data/2016/08/how-a-japanese-cucumber-farmer-is-using-deep-learning-and-tensorflow). A large enough validation set tells us if the difference in performance is significant enough for the given problem. 

- Statistical Stability  
Each class should have at least 22 data points, so that the validation set follows an approximate normal distribution. This allows easier statistical analysis for the next driving factor.

- Standard Error   
The standard error of a model shows that the performance of the model is reliable and not due to chance. The standard error is found with statistical analysis. When combined with information from business concerns, we can determine if a model is truly better than another.

#### Training Set
The remaining data forms the training set.  
Data in the training set is used for training the model.

Practitioners may choose to create a sample set out of the training set, with a much smaller amount of data. This allows for quicker testing and refinement iterations. Training on the entire training set occurs only after they discover a set of parameters they feel confident with. This action is justified, as seen in recent research. ([link](https://blog.acolyer.org/2018/03/28/deep-learning-scaling-is-predictable-empirically/))

### Model Training, Evaluation & Data Analysis
Models are evaluated on the validation set after training.
Practitioners perform data analysis on the results to gain insights on the data.
These insights guide the practitioner in tweaking the data and model for better performance.

<img src="http://scikit-learn.org/stable/_images/sphx_glr_plot_confusion_matrix_001.png" width="500"/>  
(image from scikit-learn's documentation)

The confusion matrix is a commonly used tool for data analysis. The image above shows that the model has predicted 6 instances of 'versicolor' to be 'virginica'. These misclassifications can be investigated to obtain insights on model performance and data. 

An example of investigation for convolutional neural networks (an emerging class of ML algorithms) is using class activation maps. It reveals what a convolutional neural network is looking at when it makes its predictions. The practitioner may choose to perform further data preprocessing to help the neural network focus on the right areas. 

<img src="http://cnnlocalization.csail.mit.edu/example.jpg" width="500"/>  
(Class Activation Maps, more information at http://cnnlocalization.csail.mit.edu)

Model training and refinement continues for multiple cycles until the practitioner is content with a particular model's performance. The model is tested on the test set and its performance is recorded. The model is not adjusted from this point onwards.

This test set score is used for selecting between different algorithms at the production stage.

### Production
The best model discovered is rewritten for production. They are usually rewritten in performance focused languages like C++, or in lower level machine learning frameworks such as Tensorflow. 

Models are retrained on a regular basis when more data is available, allowing the model to learn new patterns from the data.

Techniques such as model compression must also be used to ensure that the models are within the desired size limit and perform fast enough. The following paper contains a survey of existing compression techniques for neural networks. ([link](https://arxiv.org/abs/1710.09282))

Models are often trained against adversarial attacks for security reasons as well. 
An article detailing adversarial attacks on deep learning models can be found in the following link. ([link](https://blog.openai.com/adversarial-example-research/))

Here are some articles detailing the process of deploying machine learning algorithms for production.  
Dropbox: [OCR and automatic document rotation](https://blogs.dropbox.com/tech/2017/04/creating-a-modern-ocr-pipeline-using-computer-vision-and-deep-learning/)  
Mozilla: [DeepSpeech for voice recognition](https://hacks.mozilla.org/2017/11/a-journey-to-10-word-error-rate/)  
PyData's [Youtube Channel](https://www.youtube.com/user/PyDataTV/videos), with many videos on machine learning in production 

### Tips and Tricks
Practitioners find all sorts of ways to speed up their prototyping workflow.
One commonly used trick for Python is running `%prun fn(..)`, to identify slow running functions. 
This identifies inefficiencies in code which can sometimes be quickly addressed to speed up the prototyping workflow.  

A plethora of plugins exist for Jupyter. ([link](https://github.com/ipython-contrib/jupyter_contrib_nbextensions))  
Even Vim aficionados can get their fix of Vim on Jupyter through plugins.
([link](https://github.com/lambdalisue/jupyter-vim-binding))

### Concluding Remarks 
The process of coding a machine learning algorithm for production is an process fairly different from regular software development. 

This chapter covers only the process of bringing a machine learning algorithm from prototyping to production, with little mention of specific algorithms. 
The process is mostly similar for all machine learning algorithms, from classical to emerging algorithms from deep learning.

## Supplementary Resources

Commonly used libraries for machine learning
- General Machine Learning
  - [Numpy](http://www.numpy.org/), provides support for large, multi-dimensional arrays, matrices & mathematical functions for operating these data structures
  - [Pandas](https://pandas.pydata.org/), data analysis tool in Python
  - [Scikit-Learn](http://scikit-learn.org/stable/), contains a big variety of machine learning algorithms (excluding deep learning)
- Deep Learning
  - [TensorFlow](https://www.tensorflow.org/), by Google
  - [Stanford CS20: Tensorflow for Deep Learning Research](http://web.stanford.edu/class/cs20si/), up to date best practices for Tensorflow
  - [Pytorch](http://pytorch.org/), by Facebook
  
Popular resources for keeping up with machine learning research
- [https://arxiv.org/](https://arxiv.org/) (repository of electronic preprints of scientific papers)
- [https://www.arxiv-sanity.com](www.arxiv-sanity.com) (provides a better browsing experience than Arxiv)
- [https://openreview.net/](https://openreview.net/) (peer reviews of research papers submitted to conferences)
