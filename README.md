# Plagiarism Project, Machine Learning Deployment

The goal of this project is to build a plagiarism detector that examines a text file and performs binary classification; labeling that file as either *plagiarized* or *not*, depending on how similar that text file is to a provided source text. Detecting plagiarism is an active area of research; the task is non-trivial and the differences between paraphrased answers and original work are often not so obvious.

This project is broken down into three main notebooks:

**Notebook 1: Data Exploration**
* Load in the corpus of plagiarism text data.
* Explore the existing data features and the data distribution.

**Notebook 2: Feature Engineering**

* Clean and pre-process the text data.
* Define features for comparing the similarity of an answer text and a source text, and extract similarity features.
* Select "good" features, by analyzing the correlations between different features.
* Create train/test `.csv` files that hold the relevant features and class labels for train/test data points.

**Notebook 3: Train and Deploy the Model in SageMaker**

* Upload the train/test feature data to S3.
* Define a binary classification model and a training script.
* Train the model and deploy it using SageMaker.
* Evaluate the deployed classifier.

# Install

The project requires Python 3.6 and the following Python libraries installed   
  - [numpy](https://numpy.org/)
  - [pandas](https://pandas.pydata.org/)
  - [scikit-learn](https://scikit-learn.org/stable/)
  - [matplotlib](https://matplotlib.org/)

The latest version of PyTorch library also needs to be installed to implement a custom Neural Network Classifier.

# Data

The dataset used for this project is a modified version of a [dataset](https://ir.shef.ac.uk/cloughie/resources/plagiarism_corpus.html) created by Paul Clough (Information Studies) and Mark Stevenson (Computer Science), at the University of Sheffield. The corpus consists of short (200-300 words) answers to Computer Science questions in which plagiarism has been simulated. This corpus has been designed to represent varying degrees of plagiarism. 

Each text file is associated with one Task (task A-E) and one Category of plagiarism.

## Five Task types

Each text file contains an answer to one short question; these questions are labeled as tasks A-E.

## Four Category types of plagiarism

Each text file has an associated plagiarism label/category:

1. `cut`: An answer is plagiarized; it is copy-pasted directly from the relevant Wikipedia source text.
2. `light`: An answer is plagiarized; it is based on the Wikipedia source text and includes some copying and paraphrasing.
3. `heavy`: An answer is plagiarized; it is based on the Wikipedia source text but expressed using different words and structure. Since this doesn't copy directly from a source text, this will likely be the most challenging kind of plagiarism to detect.
4. `non`: An answer is not plagiarized; the Wikipedia source text is not used to create this answer.
5. `orig`: This is a specific category for the original, Wikipedia source text. These files will be used only for comparison purposes.

# Code

The project directory contains code and associated files for deploying a plagiarism detector using AWS SageMaker.

# Run

To open the .ipynb files in your browser and look at the output of the completed cells, use the following command in your terminal after changing the working directory to the project directory `project_plagiarism_detection`:
```
jupyter notebook <file_name>.ipynb
```

To run and execute all the cells from scratch, you need to create a Jupyter notebook instance in the Amazon Sagemaker service, configure the lifecycle of the notebook, and attache the [Github repository](https://github.com/wchowdhu/udacity-ml-engineer-nanodegree.git) to the instance. Once set up, you can run the cells to upload the data and save any model artifacts of the trained model in [AWS S3](https://aws.amazon.com/s3/) service. 

# Results

I implemented three different classifiers on the same dataset, the evaluation test results are displayed in the table below.

<a id='table'></a>

| Classifier | Training Time | Accuracy |
| --- | --- | --- |
| Feedforward Neural Network | 67 seconds | .80 | 
| LinearLearner | 57 seconds | .96 | 
| Naive Bayes | 55 seconds | .92 | 

Since the data set is small with less than 100 training documents that are fairly distributed over the 2 classes and contains small feature space per sample, simple models like LinearLearner and Naive Bayes classifiers gave better results, as there isn't enough data to estimate many parameters in more complex classifiers like Neural Networks. The LinearLearner performed best, with an accuary of 96%, which means that a linear function of input features is able to separate the two classes of data and effectively output labels as plagiarized or non-plagiarized.
