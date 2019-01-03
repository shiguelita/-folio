---
layout: post
title: Fraud Detection
date: 2019-01-02 20:00:00
description: 
img: 
---

Many credit card transactions are fraudulent, the purpose of this project is to detect them based on a set of historical data that already has such a classification (whether it was fraud or not). Each transaction has some variables and based on them we can identify which attributes have the greatest influence to find out which are frauds or not. The model will be based on a Machine Learning technique called supervised learning and can be used for future fraud detection.


The first step will be pre-processing the data to deal with the imbalance, which in other words, is the fact that in credit card transactions the fraud rate is much lower than the true rate. Therefore, we will use a sub-sampling technique to solve this impasse. Then, we will test some algorithms as Ensemble Methods (Random Forest, XGBoost), Support Vector Machines and Logistic Regression, verifying which one will have better performance and results. The final model will be chosen based on some metrics (see Evaluation metrics) and will have its hyperparameters optimized by GridSearch.

<h3>Evaluation metrics</h3>

Because our set is unbalanced, accuracy is not the best metric to use for model evaluation. For this study we will mainly use the AUC ROC to verify the quality of the algorithm.

The recall, also called ***True Positive Rate (TPR)***, is the ratio of True Positives to all positives correctly classified and false negatives. Mathematically represented by:



<math>
	Recall =  <box>TP<over>TP + FN</box>

</math>



To understand the AUC it is necessary to first understand the ***Receiver Operating Characteristic (ROC) *** curve, which is a graph that shows the performance of a classification model in all classification limits. This curve shows two parameters: the TPR and the False Positive Rate (FPR).

$$ FRP = \frac{FP}{TP+TN}   $$

A ROC curve traces TPR vs. FPR at different classification thresholds. Decreasing the classification threshold classifies more items as positive, thus increasing both false positives and true positives. In other words, a model predicts the probability of a class being 1 or 0, using these probabilities it is possible to plot a distribution graph as in Figure 1, with the red curve representing 0 and the green curve for 1, with 0.5 being the limit between two classes



<img  src="/img/projects/project001_01.jpg" alt="" title="Figure 1 - ROC"/>

<div class="col three caption">
	Figure 1 - ROC
</div>

All positive values above the limit (greater than 0.5) will be True Positives (TP), and all negative values above the limit will be False Positives (FP), since they were incorrectly classified as positive values. Below the limit, all negative values will be True Negatives (TN) and positive False Negatives (FN), since they were incorrectly classified as negative. This concept is best demonstrated in Figure 2.


<img src="/img/projects/project001_02.jpg" alt="" title="Figure 2 - TN, TP, FN, FP"/>

<div class="col three caption">
	Figure 2 - TN, TP, FN, FP
</div>

The AUC measures the entire two-dimensional area under any ROC curve. AUC provides an aggregate measure of performance in all possible classification limits. One way to interpret AUC is as the probability of the model classifying a random positive example more than a random negative example. A model whose predictions are 100% wrong has an AUC of 0.0; while one whose predictions are 100% correct has an AUC of 1.0. According to Figure 3


<img src="/img/projects/project001_03.jpg" alt="" title="Figure 3 - ROC AUC"/>

<div class="col three caption">
	Figure 3 - ROC AUC
</div>

Another metric used to choose the best model will be the use of the Classification Report of sklearn, it is a report table with the main evaluation metrics for each class, being: recall, precision and f1_score.
Precision is the ratio of the True Positives to all positive ones, whether they are classified correctly or not. Mathematically represented by:

$$ Precision = \frac{TP}{TP+FP}  $$

The f1_score is the harmonic mean that considers both: precision and recall, its formula being expressed by:

$$ f1 score = 2 \cdot \frac{precision \cdot recall}{precision + recall} $$

 ### Data Exploration

 The dataset refers to transactions of European card users in September 2013, obtained from Kaggle. Possessing 284,807 lines and 31 features, of which 28 are dependent variables that due to confidentiality are the results of PCA transformations. We can check each feature in detail in Table 1

 | Name | Description | Type  | Values |
| :------------: | :------------:|:------------: | :------------: | 
| Time |  Number of seconds elapsed between this transaction and the first transaction in the dataset | Quantitative | - |
| V1-V28 |  Dependent anonymous variables due to confidentiality  | Quantitative  | - |
| Amount |  Transaction amount  | Quantitative  | - |
| Class |  Variable response. If the transaction was fraudulent  | Qualitative  | 0 (false), 1 (true) |

<div class="col three caption">
	Table 1 - Description of the Dataset
</div>

There are no missing values in the dataset, however the data are unbalanced as we can see in Figure 4, where the target class variable has binary values, 0 for normal transactions and 1 for fraudulent, in which the second represents 0.17% of the data.

<img src="/img/projects/project001_04.jpg" alt="" title="Figure 4 - Unbalanced data"/>

<div class="col three caption">
	Figure 4 - Unbalanced data
</div>

We verified how the Amount variable distribution behaves, in which we can see in Figure 5 that it has an asymmetric distribution on the right (positive skewed)

<img  src="/img/projects/project001_05.jpg" alt="" title="Figure 5 - Amount Asymmetric Distribution"/>

<div class="col three caption">
	Figure 5 - Amount Asymmetric Distribution
=======
---
layout: post
title: Credit Card Fraud Detection
description: Project that detects fraudulent transactions in a real dataset from Kaggle
img: /img/2.jpg
---

Many credit card transactions are fraudulent, the purpose of this project is to detect them based on a set of historical data that already has such a classification (whether it was fraud or not). Each transaction has some variables and based on them we can identify which attributes have the greatest influence to find out which are frauds or not. The model will be based on a Machine Learning technique called supervised learning and can be used for future fraud detection.


The first step will be pre-processing the data to deal with the imbalance, which in other words, is the fact that in credit card transactions the fraud rate is much lower than the true rate. Therefore, we will use a sub-sampling technique to solve this impasse. Then, we will test some algorithms as Ensemble Methods (Random Forest, XGBoost), Support Vector Machines and Logistic Regression, verifying which one will have better performance and results. The final model will be chosen based on some metrics (see Evaluation metrics) and will have its hyperparameters optimized by GridSearch.

 ### Evaluation metrics

Because our set is unbalanced, accuracy is not the best metric to use for model evaluation. For this study we will mainly use the AUC ROC to verify the quality of the algorithm.

The recall, also called ***True Positive Rate (TPR)***, is the ratio of True Positives to all positives correctly classified and false negatives. Mathematically represented by:

\begin{align}
\dot{Recall} & = 	\frac{TP}{TP+FN} 
\end{align}

To understand the AUC it is necessary to first understand the ***Receiver Operating Characteristic (ROC) *** curve, which is a graph that shows the performance of a classification model in all classification limits. This curve shows two parameters: the TPR and the False Positive Rate (FPR).

\begin{align}
\dot{FRP} & = 	\frac{FP}{TP+TN} 
\end{align}

A ROC curve traces TPR vs. FPR at different classification thresholds. Decreasing the classification threshold classifies more items as positive, thus increasing both false positives and true positives. In other words, a model predicts the probability of a class being 1 or 0, using these probabilities it is possible to plot a distribution graph as in Figure 1, with the red curve representing 0 and the green curve for 1, with 0.5 being the limit between two classes



<div class="img_row">
	<img class="col one" src="{{ site.baseurl }}/img/projects/project001_01.jpg" alt="" title="Figure 1 - ROC"/>
</div>
<div class="col three caption">
	Figure 1 - ROC
</div>

All positive values above the limit (greater than 0.5) will be True Positives (TP), and all negative values above the limit will be False Positives (FP), since they were incorrectly classified as positive values. Below the limit, all negative values will be True Negatives (TN) and positive False Negatives (FN), since they were incorrectly classified as negative. This concept is best demonstrated in Figure 2.

<div class="img_row">
	<img class="col three" src="{{ site.baseurl }}/img/projects/project001_02.jpg" alt="" title="Figure 2 - TN, TP, FN, FP"/>
</div>
<div class="col three caption">
	Figure 2 - TN, TP, FN, FP
</div>

The AUC measures the entire two-dimensional area under any ROC curve. AUC provides an aggregate measure of performance in all possible classification limits. One way to interpret AUC is as the probability of the model classifying a random positive example more than a random negative example. A model whose predictions are 100% wrong has an AUC of 0.0; while one whose predictions are 100% correct has an AUC of 1.0. According to Figure 3


<div class="img_row">
	<img class="col three" src="{{ site.baseurl }}/img/projects/project001_03.jpg" alt="" title="Figure 3 - ROC AUC"/>
</div>
<div class="col three caption">
	Figure 3 - ROC AUC
</div>

Another metric used to choose the best model will be the use of the Classification Report of sklearn, it is a report table with the main evaluation metrics for each class, being: recall, precision and f1_score.
Precision is the ratio of the True Positives to all positive ones, whether they are classified correctly or not. Mathematically represented by:

\begin{align}
\dot{Precision} & = 	\frac{TP}{TP+FP} 
\end{align}

The f1_score is the harmonic mean that considers both: precision and recall, its formula being expressed by:

\begin{align}
\dot{f1 score} & = 2 x	\frac{precision x recall}{precision + recall} 
\end{align}

 ### Data Exploration

 The dataset refers to transactions of European card users in September 2013, obtained from Kaggle. Possessing 284,807 lines and 31 features, of which 28 are dependent variables that due to confidentiality are the results of PCA transformations. We can check each feature in detail in Table 1

 | Name | Description | Type  | Values |
| :------------: | :------------:|:------------: | :------------: | 
| Time |  Number of seconds elapsed between this transaction and the first transaction in the dataset | Quantitative | - |
| V1-V28 |  Dependent anonymous variables due to confidentiality  | Quantitative  | - |
| Amount |  Transaction amount  | Quantitative  | - |
| Class |  Variable response. If the transaction was fraudulent  | Qualitative  | 0 (false), 1 (true) |

<div class="col three caption">
	Table 1 - Description of the Dataset
</div>

There are no missing values in the dataset, however the data are unbalanced as we can see in Figure 4, where the target class variable has binary values, 0 for normal transactions and 1 for fraudulent, in which the second represents 0.17% of the data.

<div class="img_row">
	<img class="col three" src="{{ site.baseurl }}/img/projects/project001_04.jpg" alt="" title="Figure 4 - Unbalanced data"/>
</div>
<div class="col three caption">
	Figure 4 - Unbalanced data
</div>

We verified how the Amount variable distribution behaves, in which we can see in Figure 5 that it has an asymmetric distribution on the right (positive skewed)

<div class="img_row">
	<img class="col three" src="{{ site.baseurl }}/img/projects/project001_05.jpg" alt="" title="Figure 5 - Amount Asymmetric Distribution"/>
</div>
<div class="col three caption">
	Figure 5 - Amount Asymmetric Distribution
>>>>>>> 0cf6550f92c81bc19936480de0b04c1affd2b7e7
</div>