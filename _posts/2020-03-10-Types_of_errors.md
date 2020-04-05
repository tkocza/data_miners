---
title: Types of errors in model evaluation
date: 2020-03-10 00:00:00 +0100
categories: [Blogging, 'Machine Learning']
tags: ['Machine Learning', Evaluation]
toc: false
usemathjax: true
seo:
  date_modified: 2020-03-15 22:00:50 +0100
---

While resolving Machine Learning problems we aim at achieve the best results.
We dream our model will put all of samples to proper classes.
Why do we only dream? Because as people we will do mistakes and the same situation we have with algorithms.

# Classification metrics

The most often and base issue in Machine Learning is binary classification problem.
In simple words you works with two outcome classes.

Try to imagine you receive question (or hypothesis) which you have to confirm or reject.
The problem could be like "Is it a fish?" or "Regarding to conditions somebody may became a ballet dancer".
You prepare the best possible dataset, 
take a few already implemented algorithm for classification from well known package and "do magic".
Yeah, you have built model but... what next?
When you can say your model works fine (or not)?
You may use a bunch of models but how to find the best one model.
What is a metric which tell you which model are better or worse?

That is about this article.

## **Confusion matrix**
Resolving classification problem starts with put a question/hypothesis.

The model returns four kinds of results:  
1) The class for sample in the class -> **True Positive**  
2) The not-in-class sample in the not-in-class -> **True Negative**  
3) The class for sample in the not-in-class - **False Positive**  
4) The not-in-class for sample in the class - **False Negative**  
The representation of decisions made by model and actual results shown as a table.

{:class="table table-bordered"}
|:--:|:--:|:--:|
|                         | **Predicted: Class** | **Predicted: Not-in-Class** |
| **Actual: Class**        | TRUE POSITIVE       | FALSE NEGATIVE |
| **Actual: Not-in-Class** | FALSE POSITIVE      | TRUE NEGATIVE |


## **Specificity**
#### *True Negative Rate*
The rate of how many proper negative decisions taken by a model with regard to total actual negatives.

$$ Specificity = \frac{TN}{TN + FP} $$  

Higher value is better.

## **Precision**
#### *Positive Predictive Value*
The ratio of correct positive decisions with regards to all positives decisions (correct and incorrect) taken by a model. 

$$ Precision = \frac{TP}{TP + FP} $$  

Higher value is better.

## **Recall**
#### *Sensitivity*, *True Positive Rate*
probability of detection
The ratio of correct positive decisions taken by a model with regard to total actual positives. 

$$ Recall = \frac{TP}{TP + FN} $$  

Higher value is better.

## **Fallout**
#### *False Positive Rate*
 probability of false alarm
The ratio of incorrect positive decisions taken by a model with regard to total negatives.

$$ Fallout = \frac{FP}{FP + TN}$$  

or simpler  
  
$$ Fallout = 1 - Specificity $$  

Higher value is better.

## **Accuracy**
In simple words: how many times a model takes proper decision with regard to all decisions.
This measure is the most often used but the cost is losing detailed information about positives and negatives distribution.

$$ Accuracy = \frac{TP+TN}{TP + FP + FN + TN} $$

## **F Measure**
The ratio between precision and recall values.

$$ F = \frac{PPV * TPR}{PPV + TPR} = \frac{precision * recall}{precision + recall} $$  

## **For what the measures should be used?**

There is not one proper answer. Everything depends on the problems context.
Try to image a few problems:  
1) Is this message a spam?  (Class: spam, Not-in-class: important message)  
2) Is this transaction a fraud?  (Class: fraud, Not-in-class: ordinary transaction)  
3) Does someone have a serious disease?  (Class: disease, Not-in-class: health)  

??? ## **Matthew's Correlation Coefficient (MCC)**

## **ROC curve**
#### *Receiver Operating Characteristic*
The ratio between recall (True Positive Rate) and fallout (False Positive Rate).
Fallout as was mentioned above can be written as "1-Specificity".
This measure of separability shows models "approach", direction of decisions.
Value 1 means the model will always give element in Class for element in Class
Value -1 means the model will always give element in Not-in-class for element in Class 
The worst scenario is diagonal line what means taking completely random decisions.

![](../../assets/img/pictures/2020-03-10-Types_of_errors_ROC.png)

## **R²**
#### *Coefficient of determination*

## **Probability** / p-value
For multiclassification 

# **Regression metrics**
While solving prediction problems we will receive a numerical data as an output.
When you want to find the quality of your model you need compare results and actual data.
For all of below methods the lower value is more accurate and you will aim at value near zero.

Loss functions are used to find a difference between predicted points and actual values.
In formulas, there will be used:  
$$ \hat{y} $$ - predicted value  
$$ y $$ - actual value 

## **Mean Squared Error**
#### *MSE*
a mean value of squared differences between actual and predicted values. 

$$ MSE = \frac{1}{N} \sum_{i=1}^N (y_i - \hat{y}_i)^2 $$

## **Root Mean Squared Error**
#### *RMSE*
a square of MSE.

$$ RMSE = \sqrt{ \frac{ \sum_{i=1}^N (y_i - \hat{y}_i)^2 }{N} } $$

## **Mean Absolute Error**
#### *MAE*
a mean of absolute differences between actual and predicted values.

$$ MAE = \frac{1}{N} \sum_{i=1}^N |\hat{y}_i - y_i| $$

## **Mean Absolute Percentage Error**
#### *MAPE* 
a mean of absolute differences between actual and predicted value divided by actual value.

$$ MAPE = 100 \frac{1}{n} \sum_{i=1}^N \left|\frac{y_i - \hat{y}_i}{y_i}\right| $$

## **For what the measures should be used?**
All of above measures have their own pros and cons.
The good habit is using a few of them but remember you cannot compare different measures between each other.  

{:class="table table-bordered"}
|:--------:|:--:|:--:|
| **Error**| **Pros** | **Cons** |
| **MSE**  | One of the simplest loss function | Very sensitive on single high residual value |
| **RMSE** | Give a relatively high weight to large errors | Emphasize greater errors that smaller one |
| **MAE**  | Unambiguous and easy to explain measure | Insensitive for outliers |
| **MAPE** | Scale-independent and interpretable | When the actual value is equal zero the measure will fail (cannot divide by zero) |


<br/>

### Thanks for reading! 