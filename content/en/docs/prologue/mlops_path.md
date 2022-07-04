---
title: "The life cycle of a AI project"
description: "What are the main stages of AI project cycle?"
lead: "What are the main stages of AI project cycle?"
date: 2022-07-03
lastmod: 2022-07-03
draft: false
images: []
menu:
  docs:
    parent: "Section 1: Introduction"
weight: 5
toc: true
---
{{< alert icon="♻️" context="info" text="The AI project life cycle can generally be divided into three main stages: data preparation, model creation, and deployment. All three of these components are essential for creating quality models that will bring added value to your business." />}}

We will now review the main stages of any IA project cycle. As we will see throughout the website, any of the published notes can be framed within one of the following stages.

## Data Preparation

### Collect and process raw data
Raw data is rarely in a format that is easy to train a model on. Usually, it requires processing to remove aberrant data points such as null values and faulty data values. Other times, you might have to process the raw data to extract only the information you need among all of the noise.

### Analyze the data
This step involves looking at the data points and understanding their characteristics. How is it structured? What does the distribution of the data points look like? Are there any identifiable trends or biases in the data? This step is crucial because it dictates how you are going to approach the problem. If you already have a trained model you are looking to update, it also tells you if there are any new trends in the data that your model should be updated to consider. If you identified any “useless features” that don’t really influence the output, you might drop them and train a new model to improve training speed while possibly boosting performance.

### Process the data for training
In this step, you could be scaling the data to a more appropriate range and perhaps removing any outliers and/or anomalies that could interfere with model performance. Furthermore, you could also be applying feature engineering to create new features from existing data points and perhaps give your model more or a better context during training. This is also where you create training and testing data sets, though optimal practice is to make training, testing, and validation data sets.

## Model Creation

### Construct, train, and test the model
In this step, you are creating the model, setting hyperparameters, and training the model. In the case of deep learning, you can also select a subsection of the training set to be a data validation set. The purpose of this set is to have the model be evaluated on it at the end of every epoch or full forward pass of the data through the model. By comparing the model performance on data it’s seen many times over during training versus data it hasn’t seen at all (or rather, data that has no effect on weight adjustment), you can see if the model is truly learning to generalize or if it’s just overfitting.

### Overfitting check
Overfitting is when a model performs significantly better on a training set compared to data that it has never seen before. As just discussed, one way to give an early indication of overfitting is to set aside a portion of the training set as validation data during the training phase. This can give you an early indication of overfitting without having to find out after the training process has finished, which can take anywhere from minutes to days depending on the depth of the model and the equipment used. And so, it follows that overfitting can also be observed when the model is evaluated on the testing data or validation data, and discrepancies in model performance can be observed between these sets and the training set.

This phenomenon of overfitting could partially result from the model not receiving enough data points during training to reflect the variety it is expected to see, so fixing the training set by introducing more variety or even increasing the number of data points can help. Additionally, including methods such as regularization or dropout into the model’s architecture can also help combat overfitting in the case of deep learning models.

### The purpose of the testing and validation sets
An important thing to discuss is the purpose of the testing and validation sets. Testing sets are reserved for evaluating a model’s performance on data it’s never seen before.• Validation sets are reserved for helping select models, select model architecture, tune hyperparameters, or simply to give an indication of model performance on data it’s never seen during the training process.• An example of validation is k-fold cross-validation, where it generates k random partitions of test-train data from validation data and can be used to train/evaluate the model on all of them to give an idea of the best performance it can attain with various hyperparameter settings. Of course, we can also use k-fold cross validation to perform the other functions that validation helps with. 

Coupling this technique with a script that has a set of hyperparameters can result in an optimal model with proper hyperparameters. From there, the model can be retrained and evaluated again on the test set to get a final performance benchmark.

The specific order this is done in can differ, though. For example, trained models can also be evaluated first and then validated, compared to the other way around. This is because the training process is likely to be repeated with altered hyperparameters anyway after the evaluation stage reflects some form of performance discrepancy or if validation data during the training process reveals that possible overfitting is occurring. Either way, it really depends, but good practice is to at least incorporate both testing and validation data to best tune the model.

### Validate and tune the model
As previously discussed, the validation set can be another “testing” set that the model has never seen before. Once your model has reached an acceptable level of performance on the validation set and is retrained and evaluated again, you can look at deploying the model.

## Deployment
### Deploy and monitor the model
In this step, the model has finally left the hands of the machine learning/data science team. It is now the job of engineering and operational teams to integrate this model into the application and put it into service. Operational teams are in charge of constantly monitoring the performance of the model, with dips in performance possibly indicating that this entire process may need to be repeated to update the model to understand new trends. Operational teams are also responsible for reporting any bugs and unexpected model predictions to the data science team, feedback that also contributes to the start of this whole cycle as the model needs to be fixed.

Hopefully, it’s clear just how work-intensive the entire process can get, especially since it will most likely need to be repeated multiple times. While it is possibly easier the second time around since you’re only updating the model on new data patterns and trends, it is still a problem that can take up hours of manual labor that can be better spent elsewhere. After all, maintenance of applications in the software development process is usually where most of the money and resources go, not the initial construction and release of the application. The same can apply to machine learning models, worsening the overall maintenance costs because the costs for deployed machine learning models are added on top of the costs for the software application utilizing the services of the models. Imagine if you could simply automate this entire process away, allowing you to take full advantage of high-performance machine learning models without all of that hassle. 

### MLOps stage
This is where MLOps comes in, something that can be thought of as the intersection between machine learning and DevOps practices. DevOps, or developmental operations, refers to a set of practices that combines the work processes of software developers with those of operational teams to create a common set of practices that functions as a hybrid of the two roles. As a result, the developmental cycle of software is expedited, and continuous delivery of software products is ensured. Total costs also go down because maintenance costs are reduced as a result of the increase in efficiency of the workflow in maintaining the software applications. 


Software development teams typically adopt the Agile methodology of software development, which is summarized above through the planning, building, and testing stages. Operational teams are in charge of deploying, maintaining, and collecting feedback in the form of bugs and user feedback and relaying this information to the development teams. From there, the development team enters the maintenance phase of the application, where they plan, build, test, and push the next patch/update for the application. 

Furthermore, automating the process of testing and deploying allows for continuous integration and delivery of software products, something we will expand upon later in this chapterSimilarly, MLOps adopts DevOps principles and applies them to machine learning models in place of software, uniting the developmental cycles followed by data scientists and machine learning engineers with that of operational teams to help ensure continuous delivery of high-performance machine learning models. 

### Why a MLOps is crucial
Unfortunately, maintaining models once deployed also drains resources, as every new update requires reintegration into the application. This means that even if the model is deployed, all teams have their work cut out for them. For these reasons, most models simply never make it past the prototype phase. 

Until the emergence of MLOps principles, deploying solutions created using the latest in machine learning technology served as a significant challenge to businesses due to the amount of resources that would be required. This is why MLOps is so crucial. It makes it significantly easier to deploy and maintain your machine learning solutions by automating most of the hard parts for you, massively expediting the development and maintenance processes. With a fully automated setup, teams can keep up with the latest in machine learning technology and deploy new models quickly. Services can maintain their high level of performance and perhaps even improve on this front as teams can deploy newer, more promising model architectures.

##### Bibliography
* _Beginning MLOps with MLFLow_; S. Sridhar and S. Kalyan, 2021.