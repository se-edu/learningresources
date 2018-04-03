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

Learning is 'supervised' as the algorithm is told whether it has made a correct prediction. Unsupervised learning algorithms do not require labels.
These different types of algorithms provide solutions to different problems. 

Categorization for instance can be conducted with both supervised learning and unsupervised learning algorithms. 

Supervised learning techniques helps us group data to predetermined labels. 
All training data must be labelled, and the labelling process may be costly.
We do however, fit items into easily interpretable categories. 
One simple use case is image classification, to determine the type of input image.

![image classification](https://i.imgur.com/mKjIS0C.png)  
(samples from from [cifar10 dataset](https://www.cs.toronto.edu/~kriz/cifar.html))

Unsupervised learning techniques helps us group data to unknown labels.  
No labelling of data is required, but it is difficult to explain the relationship between the items in each group.   
This approach can be used by e-commerce sites to identify similar products, where a clear and interpretable label for similar products is not required.

![image clustering](https://cdn-images-1.medium.com/max/900/1*xTvsgpDfja05SRMt-H5ylA.png)  
(T-SNE of Products Shape and Colour by [Eddie Bell](https://twitter.com/ejlbell/status/698309469965516800))

Popular resources for learning ML algorithms:
- Machine Learning: [Stanford CS229](cs229.stanford.edu/)
- Convolutional Neural Networks for Visual Recognition: [Stanford CS231n](http://cs231n.stanford.edu/)
- Natural Language Processing with Deep Learning: [Stanford CS224n](cs224n.stanford.edu/)
- A practical approach towards neural networks: [University of San Francisco, FastAI](http://www.fast.ai/)

Note that there is one more category of ML algorithms, called reinforcement learning. 
Reinforcement learning is quite different from the above mentioned categories and is not yet ready for production usage. It will not be discussed in this introductory piece.

### Types of Data
Data is broadly split into 2 categories, structured and unstructured. 

Structured data refers to data in a standardized format with categories and values. Structured data tends to be formatted like the data in relational databases. A good example would be sales records, with fields like date, quantity sold, location and such.  

Unstructured data refers to data without a predefined data model, such as images, audio and textual data.

The distinction between the types of data is important as ML algorithms are not always compatible with both data types. [Decision trees](https://en.wikipedia.org/wiki/Decision_tree) for instance cannot be used on unstructured data. The data must be modified to into a structured form for decision tree related algorithms.

## Machine Learning (Prototyping to Production)
Bringing a machine learning algorithm to production requires a workflow which differs greatly from that of software engineering. This is due to a focus on prototyping.   

Prototyping is required as machine learning algorithms vary in efficacy when used in different domains. As mentioned in the introduction, machine learning algorithms model unknown functions. This modelling is never 100% correct. Statistical analysis helps with evaluating the performance of the algorithms.

The quantity, quality and type of data differs greatly across domains, sometimes containing errors such as corruption or mislabelling. Consequently, data is often pre-processed. This pre-processing is discussed in a later section.

Algorithms can only approximate the unknown function that produces desired output.
The algorithms contain various parameters that can be tuned to improve efficacy. Experimentation is required to discover the algorithm and parameters which gives the best performance.

### Prototyping Platform and Tools
The prototyping phase in ML is usually done on [Jupyter notebooks](http://jupyter.org/), or [RStudio](https://www.rstudio.com/products/rstudio/). The highlight of such programming languages like Python and R, as well as mentioned accompanying software is their strong support for experimentation and visualization of results.  

Interpreted languages such as Python and R speed up prototyping iterations as they are less verbose than compiled languages like Java or C. 
Visualizations such as bar charts, graphs or just displaying the data assist in analysis and sharing of findings. 

Code style, clarity and maintainability are less of a priority at this stage. 

A machine learning algorithm is also referred to as a 'model', after the algorithm is provided data to learn from.

### Prototyping Workflow
Prototyping can be broken down into the following phases:  
1) Basic data preprocessing  
2) Partitioning of data   
3) Model training, evaluation and data analysis  
4) Repeating step 3  

### Basic Data Preprocessing
Basic data preprocessing is required to remove unwanted noise from the data. Examples of unwanted noise are repeated content and corrupted images. The unwanted content is usually discarded. Training a model on such data hinders its ability to learn the actual patterns in uncorrupted data. 

Statistics of the data is also collected to identify data imbalances. 

![bar_chart](https://upload.wikimedia.org/wikipedia/commons/3/35/Incarceration_Rates_Worldwide_ZP.svg)  
(image from wikipedia)

Assume that the above bar chart indicates the number of data points present for each country in the data. The number of data points for United States is much larger than that of Japan. Class imbalance is present.

Class imbalance is known to negatively impact model performance. One easy remedy is oversampling the classes with fewer instances during training, up till the point where data-points from each class has the same probability of being picked. This [paper](https://arxiv.org/abs/1710.05381v1) details oversampling and other methods for tackling class imbalance when using Convolutional Neural Networks. The literature review section in the paper also links to techniques effective on other machine learning algorithms.

### Partitioning of Data
Data is usually split into 3 sets after preprocessing: the test set, validation set and training set.

![Test, Validation, Train](https://dziganto.github.io/assets/images/train-validate-test.png?raw=true)  
(by [David Zigano](https://dziganto.github.io/cross-validation/data%20science/machine%20learning/model%20tuning/python/Model-Tuning-with-Validation-and-Cross-Validation/))

#### Test Set
ML algorithms are never 100% accurate. Testing ML algorithms on real world data is essential for estimating their efficacy. 
Furthermore, ML models run the risk of overfitting the data they were trained on. 
Overfitting is a modelling error which occurs when models learn patterns unique to a subset of data. 
A model which overfits training data performs well on training data but is unable to translate this performance to real world data.

For a more thorough explanation of noise and overfitting, see [here](https://elitedatascience.com/overfitting-in-machine-learning).

A test set solves the above problem. A test set is created by partitioning the available data. (10-50%, dependent on the total amount of data available) The test set is used only for testing model performance, and will not be touched during model training. The test set should also resemble real world data as much as possible to be a good indicator of real world performance.

When the data is balanced, the test set can be created by randomly sampling the data without replacement. When the data is imbalanced, stratified sampling is performed to ensure all classes of data are adequately represented. More complex schemes for creating a test set, such as cross validation, also exist. Good splitting of data is essential in ensuring that test performance is a good indicator of real world performance. 

#### Validation Set
A validation set is then created from the remaining data in a similar fashion as the test set. The validation set is used to evaluate the performance of adjusting model parameters. This process of adjusting model parameters and verifying performance is not conducted on the test set as it may make the model overfit the test set and fail to generalize on real world data.

Guidelines on picking a size for the validation set for the statically inclined:  
See Lesson 7 of FastAI's ML MOOC for a more in-depth explanation (MOOC is at unofficial release at time of writing)

The following driving factors are suggested for deciding the validation set size.  

- Business Concerns   
4 mistakes in fraud detection is a much bigger issue than 4 mistakes in [cucumber classification](https://cloud.google.com/blog/big-data/2016/08/how-a-japanese-cucumber-farmer-is-using-deep-learning-and-tensorflow). Assuming a validation set of 1000, 4 mistakes equates to 0.2% difference in accuracy. If working on a business problem like cucumber classification, business concerns dictate that comparing models with a small performance difference (0.2% in this case) will require a bigger validation set.   

- Statistical Stability  
Each class should have at least 22 data points, so that the validation set follows an approximate normal distribution. This allows easier statistical analysis for the next driving factor.

- Standard Error   
The standard error of a model shows that the performance of the model is reliable and not due to chance.
Standard error is used to estimate the standard deviation of a sampling distribution, and is calculated with statistical analysis. This shows if the performance difference of a model is statistically significant as compared to another, helping us determine the better model.

#### Training Set
The remaining data forms the training set. Data in the training set is used for training the model.

Practitioners may also choose to create a sample set out of the training set, with a much smaller amount of data. They then train their models on the sample set rather than training set during this prototyping phase. This allows for quicker testing and refinement iterations. Training on the entire training set occurs only after they discover a set of parameters they feel confident with. 

A good article on test and validation sets can be found [here](http://www.fast.ai/2017/11/13/validation-sets/).

### Model Training, Evaluation & Data Analysis
Models are evaluated on the validation set after training.
Practitioners perform data analysis on the results to gain insights on the data.
These insights guide the practitioner in tweaking the data and model for better performance.

An example would be noticing that the model consistently fails to perform for low resolution images. A potential fix could be to downscale all images so no image is at a low resolution, training the model on them, and then continuing to train on the images at their initial preprocessed sizes. 

Looking for highly correlated fields and investigating their impact is also a common procedure when analyzing structured data. The model can then be retrained with correlated fields removed if they do not contribute much to the model's predictions. Removal of such fields usually improves model performance. Tools for statistical analysis, such as confusion matrices and Spearman's rho are often used here. 

![confusion matrix](http://scikit-learn.org/stable/_images/sphx_glr_plot_confusion_matrix_001.png)  
(image from scikit-learn's documentation)

The confusion matrix above shows that the model has predicted 6 instances of 'versicolor' to be 'virginica'. These misclassifications can be investigated  insights model performance and pre-procssed data. 

Model training and refinement continues for multiple cycles until the practitioner is content with a particular model's performance. 
Finally, the model is tested on the test set and the model's performance is recorded. 
The model should not be adjusted from this point onwards. This test set score is used to help select between different models at the production stage.

### Production
The best model discovered is now rewritten for production. They are usually rewritten in performance focused languages like C++, or in lower level machine learning frameworks such as Tensorflow. 
Models are retrained on a regular basis when more data is available, allowing the model to learn new patterns from the data.

Techniques such as model compression must also be used to ensure that the models are within the desired size limit and perform fast enough. 

Models are often trained against adversarial attacks for security reasons as well. 
An article detailing adversarial attacks on deep learning models can be found [here](https://blog.openai.com/adversarial-example-research/).  

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

The above guide covers only the process of bringing a machine learning algorithm from prototyping to production, with no mention of specific algorithms. 
This process is mostly similar for all machine learning algorithms, from classical to new and emerging deep learning algorithms.

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
  
Keeping up with machine learning research
- [https://arxiv.org/](https://arxiv.org/) (repository of electronic preprints of scientific papers)
- [https://www.arxiv-sanity.com](www.arxiv-sanity.com) (provides a better browsing experience than Arxiv)
- [https://openreview.net/](https://openreview.net/) (peer reviews of research papers submitted to conferences)
