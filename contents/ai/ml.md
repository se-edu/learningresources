# Introduction to Machine Learning (ML)
Authors: [Alex Fong](https://github.com/alexfjw)

* [What is ML](#what-is-ml)
   * [Types of ML tasks](#types-of-ml-tasks)
   * [Types of ML algorithms](#types-of-ml-algorithms)
   * [Types of Data](#types-of-data)
* [Machine Learning Workflow (Prototyping to Production)](#machine-learning-workflow-prototyping-to-production)
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
Machine learning is a subfield in artificial intelligence whereby computers learn from data to perform a task. Machine learning algorithms are powerful because they can discern complex pattern within data, and utilize them to model functions to produce desired outputs. An example would be machine translation, whereby the function modeled takes in text in one language and outputs text in another.

### Types of ML tasks
Machine learning algorithms perform well for a large variety of tasks, from computer vision to natural language processing. Here is a non-exhaustive list:

- Sentiment analysis 
- Machine translation 
- Image generation ([SuperRes](https://github.com/alexjc/neural-enhance), [Texture Synthesis](http://bethgelab.org/deeptextures/))
- Voice recognition, generation ([DeepSpeech](https://github.com/mozilla/DeepSpeech))
- Noise supression ([RNNoise](https://people.xiph.org/~jm/demo/rnnoise/))
- Optical Character Recognition
- Data analytics (identifying trends, predicting sales)
- Recommender systems (recomending products based on user profiles)
- Instance segmentation (object detection & recognition to the pixel level [MaskRCNN](https://github.com/matterport/Mask_RCNN)) 
- Classification (often used for document indexing and retrieval)

### Types of ML Algorithms
There are two broad categories of machine learning algorithms, supervised learning and unsupervised learning. 

In supervised learning, the data used for training is accompanied with the desired output labels. Learning is 'supervised' as the algorithm is told whether it has made a correct prediction. In unsupervised learning, no labels are given to the algorithm.

Popular resources for learning ML algorithms:
- Machine Learning: [Stanford CS229](cs229.stanford.edu/)
- Convolutional Neural Networks for Visual Recognition: [Stanford CS231n](http://cs231n.stanford.edu/)
- Natural Language Processing with Deep Leraning: [Stanford CS224n](cs224n.stanford.edu/)
- A practical approach towards neural networks: [University of San Francisco, FastAI](http://www.fast.ai/)

### Types of Data
Data is broadly split into 2 categories, structured and unstructured. 

Structured data refers to data in a standardized format with categories and values. Structured data tends to be formatted like the data in relational databases. A good example would be sales records, with fields like date, quantity sold, location and such. Unstructured data refers to data without a predefined data model, such as images, audio and textual data.

The distinction between the types of data is important as different ML algorithms perform better on different types of data. In particular, recent advances in neural networks has shown that they perform extremely well on unstructured data.

## Machine Learning Workflow (Prototyping to Production)
Bringing a machine learning algorithm in production requires a workflow which differs greatly from that of software engineering. This is due to a focus on prototyping. Prototyping is required as machine learning algorithms vary in efficacy when used in different domains. The quantity, quality and type of data differs greatly as well, sometimes containing errors such as corruption. Machine learning algorithms contain various parameters that can be tuned to improve performance.

The prototyping phase in ML is usually done on [Jupyter notebooks](http://jupyter.org/), or [RStudio](https://www.rstudio.com/products/rstudio/). The highlight of such software is their strong support for visualization and quick iterations. Visualizations are essential as they help practitioners analyze data and share findings with coworkers. Code style and clarity is less of a priority at this stage. 

A machine learning algorithm is also refered to as a model after the algorithm is provided data to learn from.

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

Assume that the above barchart indicates the number of data points present for each country in the data. The number of data points for United States is much larger than that of Japan, clearly indicating the presence of class imbalance. The importance of identifying class imbalance is highlighted in the next phase.

### Partitioning of Data
Data is usually split into 3 sets, the training set, validation set and test set.

#### Test Set
ML models are hardly 100% accurate due to noise in data. Testing the algorithms on real world data is essential for estimating their efficacy. Furthermore, ML models run the risk of overfitting the data they were trained on. Overfitting is a modeling error which occurs when models learn patterns unique to a subset of data. A model which overfits training data performs well on training data but is unable to translate this performance to real world data.

A test set solves the above problem. A test set is created by partitioning the available data. (20-50%, dependent on the total amount of data available) The test set is used only for testing model performance, and will not be touched during model training. The test set should also resemble real world data as much as possible to be a good indicator of real world performance.

When the data is balanced, the test set can be created by randomly sampling the data without replacement. When the data is unbalanced, stratified sampling is performed to ensure all classes of data are adequately represented. This process ensures that no class is skipped during testing, in turn ensuring that test performance is a good indicator of real world performance. 

#### Validation Set
A validation set is then created from the remaining data in a similar fashion as the test set. The validation set is used to evaluate the performance of adjusting model parameters. As mentioned, models posess a large number of parameters This process of adjusting model parameters is not conducted on the test set as it may make the model overfit the test set and fail to generalize on real world data.

#### Training Set
The remaining data forms the training set. Data in the training set is used for model training. 

Practitioners may also choose to create a sample set out of the training set, with a much smaller amount of data. They then train their models on the sample set rather than training set during this prototyping phase. This allows for quicker testing and refinement iterations. Training on the entire training set occurs only after they discover a set of parameters they feel confident with. 

A good article on test and validation sets can be found [here](http://www.fast.ai/2017/11/13/validation-sets/).

### Model Training, Evaluation & Data Analysis
The model is evaluated on the validation set after it is trained.
Practitioners also perform data analysis on the results to gain further insights on the data.
These insights guide the practitioner in tweaking the data and model for better performance.

An example would be noticing that the model consistently fails to perform for low resolution images. All images have been resized to a standard size at the preprocessing phase. A potential fix could be to downscale all images so no image is at a low resolution, training the model on them, and then continuing to train on the images at their initial preprocessed sizes. 

Looking for highly correlated fields and investigating their impact is also a common procedure when analyzing structured data. The model can then be retrained with correlated fields removed if they do not contribute much to the model's predictions. Removal of such fields usually improves model performance. Tools for statistical analysis, such as confusion matrices and Spearman's rho are often used here. 

![confusion matrix](http://scikit-learn.org/stable/_images/sphx_glr_plot_confusion_matrix_001.png)  
(image from scikit-learn's documentation)

The confusion matrix above shows that the model has predicted 6 instances of 'versicolor' to be 'virginica'. These misclassifications can be investiagated to obtain insights on both model performance and data.

This process of model training and refinement continues for multiple cycles until the practitioner is content with the model's performance. Finally, the model is tested on the test set and the model's performance is recorded. The model should not be adjusted from this point onwards. This test set score is used to help select between models for production.

### Production
The best model discovered is now rewritten for production. They are usually rewritten in C++, or in lower level machine learning frameworks such as Tensorflow. Models are often retrained on a regular basis when more data is available.

Techniques such as model compression must also be used to ensure that the models are within the desired size limit and perform fast enough. Deep learning models are often trained against adverserial attacks for security resasons as well. 

An article detailing adverserial attacks on deep learning models can be found [here](https://blog.openai.com/adversarial-example-research/).  
Also, here are some articles detailing the process of deploying machine learning algorithms for production.  
Dropbox: [OCR and automatic document rotation](https://blogs.dropbox.com/tech/2017/04/creating-a-modern-ocr-pipeline-using-computer-vision-and-deep-learning/)  
Mozilla: [DeepSpeech for voice recognition](https://hacks.mozilla.org/2017/11/a-journey-to-10-word-error-rate/)

### Tips and Tricks
Practitioners find all sorts of ways to speed up their prototyping workflow.
One commonly used trick for Python is running `%prun fn(..)`, to profile slow running functions. Profiling helps identify inefficiencies in the code which can sometimes be quickly addressed to speed up functions.

A plethora of plugins exist for Jupyter. Even Vim aficionados can get their fix of Vim on Jupyter through plugins. Links for
[general plugins](https://github.com/ipython-contrib/jupyter_contrib_nbextensions), [Vim](https://github.com/lambdalisue/jupyter-vim-binding).

### Concluding Remarks 
The process of coding a machine learning algorithm for production is an process fairly different from regular software development. 

The above guide currently only covers the process of bringing a machine learning algorithm from prototyping to production, with no mention of specific algorithms. This process however, is mostly similar for all machine learning algorithms, from classical to new and emerging deep learning algorithms.
