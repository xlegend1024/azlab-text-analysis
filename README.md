# azlab-text-analysis

## Architecture

![arch](./images/0.0.png)

## Scenario

## Prerequisite

1. Twitter Account

> Optional) _* Enable Twitter Developer [link](https://developer.twitter.com/)_

1. Azure Account and Subsciprtion

* Have contributor role of a subscription or a resource group


## Labs

### [0. Create Lab Environment](https://github.com/xlegend1024/azlab-text-analysis/tree/master/0.EnvironmentSetting)

* Create workshop environment

### [1. Logic App](https://github.com/xlegend1024/azlab-text-analysis/tree/master/1.LogicApp)

* Create a logic to collect tweet text from Twitter

### [2. Azure Databricks](https://github.com/xlegend1024/azlab-text-analysis/tree/master/2.ADB)

* Understand data and train data to select a model

### [3. Azure Machine Learning](https://github.com/xlegend1024/azlab-text-analysis/tree/master/3.AML)

* Register the model to Azure Machine Learning Service and deploy it as web service on AKS

### [4. Logic App](https://github.com/xlegend1024/azlab-text-analysis/tree/master/4.LogicApp)

* Get a real-time predictinon and save the result to CosmosDB

### Delete Resource Group