---
title:  "Building Pandas friendly versions of ML pipelines using custom transformers"
classes: wide
header:
  teaser: /assets/images/pipelines_blogpost/data_preprocessing_in_ml.PNG
  overlay_image: /assets/images/pipelines_blogpost/data_preprocessing_in_ml.PNG
  overlay_filter: 0.7 # same as adding an opacity of 0.5 to a black background

excerpt: "How to use custom transforming using classes in a way that our transformations are pandas friendly and we don't lose tract of key information about our dataset"
published: false
---

# Introduction

Data processing is one of the key steps in any ML project. Being able to grab data from different sources and transform it so it can be fed to our models is something that most of the Data Scientist I know have done and struggled with at some point. Mistake like using pandas functions to perform transformations on the pandas training dataset only to realize that the same set of pandas transformations in the test set resulted in a different number of variables or imputing values such as median or average using values obtained from the test set are not rare when data handling is disorganized and non-modular. 

Today I will talk about my main takeaways from sklearn Pipelines, a module that allows data scientist to keep data transformations in one place, in a flexible and easy to understand sequence, which eliminates the errors described above and facilitates the development of scalable and reproducible model. In addition, I will show briefly custom transformers can be used to keep our data in an easy to read object, such as a Pandas dataframe. Finally, I will introduce FeatureUnion, a transformer that unites all our transformations in a single place.

For this blog post, I will use the [Credit Card Approval Prediction dataset from Kaggle](https://www.kaggle.com/datasets/rikdifos/credit-card-approval-prediction). I encourage the reader to take a look to the dataset because I'm not going to spend time explaining each data point in the dataset. Helpful resources that helped me to comprehend Pipelines and how to use them  efficiently are:

- [Kevin Goetsch - Deploying Machine Learning using sklearn pipelines](https://www.youtube.com/watch?v=URdnFlZnlaE)
- [FILIPK - Feature Engineering with sklearn Pipelines](https://www.kaggle.com/code/fk0728/feature-engineering-with-sklearn-pipelines/input)

# Why are Pipelines used anyway?

With Python, it is possible to perform all the transformations necessary to create your input data for an ML model. Doing this manually applying pandas functions to the input data have 2 drawbacks (at least when I tried this approach). First, it gets complicated and disorganized quite quickly making your code hard to read and harder to debug. The other problem relates to memory. Keeping key variables not linked to an specific object in a global environment can lead to bugs and data leakage. The need for a better way to chain transformations and keeping key estimators in just one place becomes evident, at least if you want to keep your ML models scalable and reproducible. Sklearn pipelines is a class that allows data scientist to perform step-by-step transformation and fitted estimators into one object. From standardizing variables to performing imputations, pipelines allow us to define the sequence in which a data field will be converted so I can be used primarily in ML algorithms.

# Pipeline: The basics



