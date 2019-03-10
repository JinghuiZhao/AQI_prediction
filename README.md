# AQI prediction

This project is aimed at predicting the air quality using the pass information about air attributes, and implementing a efficient way of processing large dataset using mongoDB, EMR and SageMaker. The dataset we used is from Kaggle: https://www.kaggle.com/epa/epa-historical-air-quality

According to the concentration of the daily CO2, CO, O3, NO, NO2, PM2.5, SO2, PM10, besides that, we have air pressure, temperature, wind velocity. We created label like the Air quality index of the day before, We aggregated by site/city and computed the average of the pollutants concentration and the trend of the change of the concentrations. We also did feature engineering like summing the number of big cities near the measuring site. We ordered the data by date and did train test split of ratio 0.7 and 0.3. 



## System Workflow 
For this study, the data science pipeline was designed around scalability, cloud resources and distributed methods.Since Apache Spark on EMR allows fast processing of big data, and mondoDB is a NoSQL database that provides support for document-oriented storage systems and it provides a rich set of features, including full index support, sharding, and replication. Thus, we developed our pipeline by using Amazon Web Service (AWS), 

To be more specific, our preprocessed data was stored in AWS Simple Storage Service(S3) bucket, then data was transferred and loaded into the MongoDB on AWS Elastic Compute Cloud (EC2). And then the data was processed on Amazon SageMaker notebooks backed by Apache Spark in Amazon AWS Elastic MapReduce (EMR). 

Once we connect with AWS EMR, in the SageMaker Notebook the data was processed using the following six steps :
* Read data from mongoDB
* Spark Schema and DataFrame create
* DataFrame processing 
* Machine Learning model training
* Prediction
* Compare running time efficiency on different EMR instances.( Different machines for master/slave nodes)

![alt text](https://github.com/JinghuiZhao/AQI_prediction/blob/master/workflow.png)

The models we will build are:

```1. Multivariate Linear Regression:```

Multivariate regression is a technique that estimates a single regression model with multiple independent variables contributing to the dependent variable and hence multiple coefficients to determine and complex computation due to the added variables.

```2. Random Forest: ```

A Random Forest Regression is an ensemble technique capable of performing regression task with the use of multiple decision trees and a technique called Bootstrap Aggregation.
Due to the instability of a single regression tree, the random forest regression model estimates the desired Regression Tree on many bootstrap samples (re-sample the data many times with replacement and re-estimate the model) and make the final prediction as the average of the predictions across the trees.

```3. Gradient Boosting Tree:```

A Gradient Boosting Tree Regression is another ensemble technique. Comparing to Random Forest, Gradient Boosting Tree is based on weak learners(high bias and low variance). Therefore, weak learners are shallow trees. Boosting reduces error by reducing the bias.


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

