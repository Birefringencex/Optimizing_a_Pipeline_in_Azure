# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary

This dataset contains data about the direct marketing campaigns (phone calls) of a Portuguese banking institution. The classification goal is to predict if the client will subscribe to a term deposit (variable y).
During the first part of the project, the Dataset was treated with an assigned Scikit learn model of Logistic Regression, which is a supervised learning linear model for classification. The goal was to test and choose the Hyperparameter variables for later use in the Hyperdrive. The metric to seek for the best run was  Accuracy, which results in 0.9101 for this experiment.
The second part dataset was treated with the AutoML, also doing a task of classification and looking a primary metric Accuracy. During this trial, the best performing model was soft VotingEnsemble  who had an Accuracy of 0.91683 


## Scikit-learn Pipeline

Within a Python file (train.py), the Bank marketing study Dataset was first retrieved from a web path and the transform in a TabularDataset using TabulaDatasetFactory, then the Data was treated with a given function named “clean_data”, its main purpose was to fix some irregularities within the data, like changing categorical values to numbers, get rid of missing values or fixing some columns.
With a clean Dataset, the Data was split into train and test set into a ratio of 70:30, this was done with the train_test split. Then the main function was used to declare the argument and set the model, in this first part the chosen model was LogisticRegression, and the model score was Accuracy. 
The next step was performed in a Jupyter Notebook, where at first basic connections were established and a Compute Cluster was created. Also, a Sklearn estimator used with the Python file (train.py)  was applied here to train the experiment and then to configure the  Hyperdrive. This Hyperdrive configuration was created using the estimator, hyperparameter sampler, and a chosen policy.  The tuning of the parameter was performed by changing the values of  C (Inverse of regularization strength. Smaller values cause stronger regularization) and max_iter(Maximum number of iteration to converge).
Finally, the Hyperdrive run was submitted and the details were showed with the widget. Not forgetting that the best run were fetched and then also saved with the corresponding resulting Accuracy.

**What are the benefits of the parameter sampler you chose?**

To find the best-performing model according to a defined performance metric (in this project is accuracy),  the data must be trained with different hyperparameter values. I decided to use a Random Sampling to randomly select a value for each hyperparameter, the values here are given on the python file (train.py) and those are C (Inverse of regularization strength) and max_iter( Maximum number of iteration to converge). I gave those values a search space for a discrete parameter using a choice from a list of explicit values, defined as a Python list.

**What are the benefits of the early stopping policy you chose?**

From the defined hyperparameter search space, the number of iterations possible could result in a large number of runs that don’t result in a better model than a combination that has already been tried. To avoid that and save time and resources, it is recommended to set an early termination policy to quit the run that is not coming to a better result than the previous run. I chose to use a bandit policy to stop a run if the target performance metric underperforms the best run so far by a specific margin, in this example the policy is applied for every iteration after the first five and abandons runs where the reported target metric is 0.1 or worse than the best performing run in an evaluation interval of 2, which is the frequency for applying the policy. The interval increases each time the training script logs the primary metric.

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**

The AutoML enables to try multiples algorithms and preprocessing transformation with the data, this helps to find the best performing model without the effort of trying the whole process manual process to chose the best parameters to obtain the best model.
AutoML in Azure Machine Learning trains a large number of models for different types of machine learning tasks like Classification, Regression, or Time Series Forecasting. By default, AutoML will randomly select from a full range of algorithms for the specified task. For this example, the dataset was retrieved from the same website as before, then was transformed to in a TabularDataset using TabulaDatasetFactory, then cleaned with the clean_data function, and finally split into train and test sets. To run AutoML a set of parameters were chosen in an AutoML Configuration, for this case a timeout time of 30 minutes was used, the task was defined as classification, the primary metric was again accuracy as it was in scklearn and the label column name y, which is the goal to predict and also the n_cross_validation which are the n_fold cross-validation to able to use in each model.
Here is screen shot  of a list of the algorithm used in the AutoML, the algorithm name of the best model was Voting Ensemble with an accuracy of 0.91683



## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**

architecture? If there was a difference, why do you think there was one?**
In general terms, there is only a slight difference in accuracy between the best run of  Scikit-learn Pipeline (0.9101) and the best model obtained with AutoML (0.91683). Neither the less the result with AutoML is better than with the Scikit-learn. AutoML tried different algorithms and can obtain better results because it has a large probability to find the best model because there it trains more models. The slight diference in accuracy could probably be caused because the dataset is not large enough to show the advantage of AutoML.
Here is a screen shot of the Scikit Pipeline:

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**

## Proof of cluster clean up
**If you did not delete your compute cluster in the code, please complete this section. Otherwise, delete this section.**
**Image of cluster marked for deletion**
