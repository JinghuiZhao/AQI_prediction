# AQI prediction


## Problem Description

Due to the rapid global Industrialization and urbanization process, environmental pollution issues such as air pollution have become more and more severe. The problem of air pollution is much severe in California.  Air quality forecasting is an effective way of protecting public health by providing an early warning against harmful air pollutants.

The Air Quality Index (AQI) is an index for reporting daily air quality and it can be used to warn the public when air pollution is dangerous. For instance,  as the AQI increases, the public is likely to experience increasingly severe adverse health effects. United States Environmental Protection Agency (US EPA) calculated the AQI for five major pollutants: Particulate Matter(PM), Sulphur dioxide (SO2), Carbon monoxide (CO), Nitrogen dioxide (NO2), ground-level Ozone (O3). 

In this study, we focus on using Big Data technology (MongoDB, Apache Spark, AWS) and machine learning methods to model and predict California daily AQI with better computation efficiency on the basis of air pollution data. By doing this, this study aims to give public warning in advance and let the public sector to engage in pre-event planning.


## System Workflow 

The data set will be dealing is from bigQuery Dataset on Kaggle: https://www.kaggle.com/epa/epa-historical-air-quality

For this study, the data science pipeline was designed around scalability, cloud resources, and distributed computing methods. In this case, we developed our pipeline by using Amazon Web Service (AWS), for AWS can provide high availability and scalability and makes its components including storage and processing engines to be compatible.  In general,  our preprocessed data was stored in AWS Simple Storage Service(S3) bucket, then data was transferred and loaded into the MongoDB on AWS Elastic Compute Cloud (EC2). Later, our data was processed using Apache Spark on AWS Elastic MapReduce (EMR).

![alt text](https://github.com/JinghuiZhao/AQI_prediction/blob/master/workflow.png)

The models we will build are:

**1. Multivariate Linear Regression:
Multivariate regression is a technique that estimates a single regression model with multiple independent variables contributing to the dependent variable and hence multiple coefficients to determine and complex computation due to the added variables.**

**2. Random Forest: 
A Random Forest Regression is an ensemble technique capable of performing regression task with the use of multiple decision trees and a technique called Bootstrap Aggregation.
Due to the instability of a single regression tree, the random forest regression model estimates the desired Regression Tree on many bootstrap samples (re-sample the data many times with replacement and re-estimate the model) and make the final prediction as the average of the predictions across the trees.**

**3. Gradient Boosting Tree: 
A Gradient Boosting Tree Regression is another ensemble technique. Comparing to Random Forest, Gradient Boosting Tree is based on weak learners(high bias and low variance). Therefore, weak learners are shallow trees. Boosting reduces error by reducing the bias.**


To achieve this,  this project included the following files: 
<ul>
<li> city_level_prediction.ipynb; group by city and do feature engineering like finding the trend of the past few days and number of big cities nearby. And implement random forest regressor, gradient boosted regressor, linear regression, with Spark ML </li>
<li> site_level_prediction.ipynb; group by observation site and implement random forest regressor, gradient boosted regressor, linear Regression with Spark ML </li>

<li> data_query.ipynb; Retriveing data using big query API and data preprocessing(join tables) </li>
</ul>

As for the evulation part, 

When using different models on city level and site level data, we got the RMSE comparison, we can see that gradient boosting regression is slightly better than the other 2 models.
![alt text](https://github.com/JinghuiZhao/AQI_prediction/blob/master/rmse.png)

When setting up different EMR machines, we got the running time comparison, and we can see that with larger machine and more cores, the running time will decrease.
![alt text](https://github.com/JinghuiZhao/AQI_prediction/blob/master/runtime.png)

