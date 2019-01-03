---
layout: post
title: Fraud Detection
date: 2019-01-03 13:00:00
description: Project that detects credit card fraudulent transactions
---

Many credit card transactions are fraudulent, the purpose of this project is to detect them based on a set of historical data that already has such a classification (whether it was fraud or not). Each transaction has some variables and based on them we can identify which attributes have the greatest influence to find out which are frauds or not. The model will be based on a Machine Learning technique called supervised learning and can be used for future fraud detection.

The first step will be pre-processing the data to deal with the imbalance, which in other words, is the fact that in credit card transactions the fraud rate is much lower than the true rate. Therefore, we will use a sub-sampling technique to solve this impasse. Then, we will test some algorithms as Ensemble Methods (Random Forest, XGBoost), Support Vector Machines and Logistic Regression, verifying which one will have better performance and results. The final model will be chosen based on some metrics (see in **Evaluation metrics**) and will have its hyperparameters optimized by GridSearch.

### **Evaluation metrics**
<br/>
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

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_01.jpg" alt="" title="Figure 1 - ROC"/>
</div>
<div class="col three caption">
	Figure 1 - ROC
</div>
<br/>

All positive values above the limit (greater than 0.5) will be True Positives (TP), and all negative values above the limit will be False Positives (FP), since they were incorrectly classified as positive values. Below the limit, all negative values will be True Negatives (TN) and positive False Negatives (FN), since they were incorrectly classified as negative. This concept is best demonstrated in Figure 2.

<div style="display: flex; justify-content: center;">
<img src="/img/projects/project001_02.jpg" alt="" align="center" title="Figure 2 - TN, TP, FN, FP"/>
</div>
<div class="col three caption">
	Figure 2 - TN, TP, FN, FP
</div>
<br/>

The AUC measures the entire two-dimensional area under any ROC curve. AUC provides an aggregate measure of performance in all possible classification limits. One way to interpret AUC is as the probability of the model classifying a random positive example more than a random negative example. A model whose predictions are 100% wrong has an AUC of 0.0; while one whose predictions are 100% correct has an AUC of 1.0. According to Figure 3

<div style="display: flex; justify-content: center;">
<img src="/img/projects/project001_03.jpg" alt="" title="Figure 3 - ROC AUC"/>
</div>
<div class="col three caption">
	Figure 3 - ROC AUC
</div>
<br/>

Another metric used to choose the best model will be the use of the Classification Report of sklearn, it is a report table with the main evaluation metrics for each class, being: recall, precision and f1_score.
Precision is the ratio of the True Positives to all positive ones, whether they are classified correctly or not. Mathematically represented by:

\begin{align}
\dot{Precision} & = 	\frac{TP}{TP+FP} 
\end{align}

The f1_score is the harmonic mean that considers both: precision and recall, its formula being expressed by:

\begin{align}
\dot{f1 score} & = 2 \cdot	\frac{precision \cdot recall}{precision + recall} 
\end{align}

### **Data Exploration**
<br/>
The dataset refers to transactions of European card users in September 2013, obtained from Kaggle. Possessing 284,807 lines and 31 features, of which 28 are dependent variables that due to confidentiality are the results of PCA transformations. We can check each feature in detail in Table 1

<div style="display: flex; justify-content: center;">
<img src="/img/projects/project001_12.jpg" alt="" title="Table 1 - Description of the Dataset"/>
</div>
<div class="col three caption">
	Table 1 - Description of the Dataset
</div>
<br/>

There are no missing values in the dataset, however the data are unbalanced as we can see in Figure 4, where the target class variable has binary values, 0 for normal transactions and 1 for fraudulent, in which the second represents 0.17% of the data.

<div style="display: flex; justify-content: center;">
<img src="/img/projects/project001_04.jpg" alt="" title="Figure 4 - Unbalanced data"/>
</div>
<div class="col three caption">
	Figure 4 - Unbalanced data
</div>
<br/>

We verified how the Amount variable distribution behaves, in which we can see in Figure 5 that it has an asymmetric distribution on the right (positive skewed)

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_05.jpg" alt="" title="Figure 5 - Amount Asymmetric Distribution"/>
</div>
<div class="col three caption">
	Figure 5 - Amount Asymmetric Distribution
</div>
<br/>

### **Data Visualization**
<br/>

Our dataset has 28 variables transformed via PCA without being with their true labels, because of this the creation of hypotheses about them becomes a little bigger challenge. The assumption is, due to this transformation, they do not have correlation with each other, since PCA creates an orthogonality between these features, this fact is proven in the HeatMap in Figure 6. The variables that we know are Amount, Time in seconds (Time) and Class (Class), in Figure 7 we can evaluate how these three features behave among themselves, not being possible to notice any pattern.

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_06.jpg" alt="" title="Figure 6 - Heatmap: Correlation between variables"/>
</div>
<div class="col three caption">
	Figure 6 - Heatmap: Correlation between variables
</div>
<br/>
<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_07.jpg" alt="" title="Figure 7 - Amount vs. Time per Class"/>
</div>
<div class="col three caption">
	Figure 7 - Amount vs. Time per Class
</div>
<br/>

### **Algorithms and Techniques**
<br/>
In this project we will test four algorithms of the sklearn library, in the intention to choose for the one that obtains a better result in the analyzes. The models used in this study are:

* **Support Vector Machines (SVM)**: uses a technique called kernel to transform data and finds an optimal boundary between possible outputs. It has a high accuracy, is not sensitive to data non-linearity, low risk of overfitting using correct kernels, works well with high dimensional data.

* **Logistic Regression (RL)**: estimates the probability associated with the occurrence of a given event due to some features. It has a high degree of reliability and it is not necessary to assume normality and multicollinearity.

* **Random Forest (RF)**: an ensemble method used to construct a model based on multiple Decision Trees during the training phase. It has a low risk of overfitting, it is usually quick to train, however slow to predict. Better scalability when the dataset is smaller.

* **XGBoost (XGB)**: is an advanced implementation of Gradient Boosting, faster and with high predictive power. It has a variety of regularizations that reduce overfitting and improve overall performance.

The techniques used in this project are:

* **GridSearch**: Optimizes hyperparameters to find the most optimized model possible.

* **Under-sampling**: Removes majority class lines from modeling to avoid unbalance.

* **Over-sampling**: Replicates minority class rows or generates synthetic data to avoid unbalance.

* **Synthetic Minority Over-Sampling Technique (SMOTE)**: Used to synthesize data in over-sampling.

* **Log Transformation**: used to normalize data with asymmetric distribution.

* **MinMaxScaler**: standardizes features on the same scale.

### **Benchmark**
<br/>
Using the same dataset in the most voted study in Kaggle, the author used a Logistic Regression to create the model that obtained an accuracy and recall about to 93% and, according to Figure 8, obtained the ROC AUC at about 0.95. 

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_08.jpg" alt="" title="Figure 8 - Benchmark"/>
</div>
<div class="col three caption">
	Figure 8 - Benchmark
</div>
<br/>

### **Preprocessing**
<br/>
Due the asymmetric distribution of Amount, with some data being 0, we have to perform a log transformation(x + 1), then moved on to Feature Scaling, which consists of rescaling the data to obtain a distribution with average 0 and standard deviation 1. The technique used for this step was MinMaxScaler from sklearn, in the feature column transformed by log, making the data on a scale of 0 to 1. A new dataset was created to store the original data plus the transformed one and the features Time and Amount original.

Once we have the data ready to analyze, it is necessary to divide the data between training and test sets, here we use a proportion of 70% for training and 30% for test. Finally, to treat the data imbalance, it was necessary to resample them, using only the training data, in this step six new datasets with different resampling techniques were created, as can be seen in Table 2.

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_13.jpg" alt="" title="Table 2 - Resampling Techniques"/>
</div>
<div class="col three caption">
	Table 2 - Resampling Techniques
</div>
<br/>

### **Implementation**
<br/>
In this step, a function was created to apply a classifier algorithm without any optimized parameters, showing the results of the metrics mentioned in the chapter **Evaluation metrics** and the time that the algorithm uses to train and predict the data, based on the resulting datasets of resampling. The purpose of this step was to verify which algorithm and resampling technique has the best performance in training and test data to find the best model.

In Table 3 are all algorithms separated by resampling techniques, showing the performance of each model ordered by the highest value in ROC AUC, the metric being the main influencer in the choice of the model, followed by f1_score for class 1 and then class 0.

We can conclude by observing Table 3 that the RL algorithm with the Smote with Tomek Links technique with equal class balancing obtained the best ROC AUC score (0.947), however the f1_score for class 1 was very low, being 0.110, so the algorithm chosen was the one that performed the best balanced results between the two metrics, being the RF algorithm with the same resampling technique of the above model, obtaining ROC AUC at 0.905 and f1_score at 0.850.	

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_14.jpg" alt="" title="Table 3 - Models Performance"/>
</div>
<div class="col three caption">
	Table 3 - Models Performance
</div>
<br/>

### **Refinement**
<br/>
Having the model and the resampling technique defined, the next step was to optimize the algorithm, using the GridSearch library of sklearn. Here it is important to note that since our evaluation metric will be ROC AUC, having a chosen algorithm, we will use its prediction with probability, which in the case of RF is predict_proba, because, as explained in the chapter on **Evaluation metrics**, the curve ROC AUC is made by varying the threshold to find the false positive / negative rates by calculating the probabilities for each class.

The result of the ROC AUC with predict_proba in the model without refinement was 0.9439, with the following hyperparameters:

```python
RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini'
            max_depth=None, max_features='auto', max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, n_estimators='warn', n_jobs=None
            oob_score=False, random_state=96, verbose=0, warm_start=False)
```

To improve the performance of the model GridSearch was used to optimize the hyperparameters according to Table 4.
<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_15.jpg" alt="" title="Table 4 - Description of Hyperparameters"/>
</div>
<div class="col three caption">
	Table 4 - Description of Hyperparameters
</div>
<br/>

The final result of the AUC ROC with predict_proba in the optimized model was 0.9823, which can be verified in Figure 10, with the following hyperparametres:
```python
RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini'
            max_depth=10, max_features='auto', max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=5, min_samples_split=2,
            min_weight_fraction_leaf=0.0, n_estimators=822, n_jobs=None,
            oob_score=False, random_state=None, verbose=0, warm_start=False)
```

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_09.jpg" alt="" title="Figure 9 - ROC AUC optimized model"/>
</div>
<div class="col three caption">
	Figure 9 - ROC AUC optimized model
</div>
<br/>

Checking the relevance of each feature in the model the result was that 5 attributes accumulated 70% of the importance of the data, as we can see in Figure 10. However, when using the same model with only 5 features the ROC AUC dropped to 0.9182, due to the great difference of the metric of both models, the optimized model was chosen with all the features.

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_10.jpg" alt="" title="Figure 10 - Features Importance"/>
</div>
<div class="col three caption">
	Figure 10 - Features Importance
</div>
<br/>

### **Conclusion**
<br/>
I believe that the project was successful in analyzing fraud detection by credit card, and managed to improve the result demonstrated in the benchmark. An important point was the need to find a model that had a good balance in the prediction of both classes, because of the four algorithms selected only the RF performed well (above 0.8) in both classes.
For a next step it would be ideal to try to improve the prediction of the model for the False Positives, without worsening the performance of the True Positives, since in this study we had 72 cases of False Positive, seen in Figure 11.

<div style="display: flex; justify-content: center;">
<img  src="/img/projects/project001_11.jpg" alt="" title="Figure 11 - Confusion Matrix"/>
</div>
<div class="col three caption">
	Figure 11 - Confusion Matrix
</div>
<br/>


### **Bibliography**
<br/>

Braga, Antonio Padua & Castro, Cristiano Leite de. 2011. Aprendizado Supervisionado com conjunto de dados desbalanceados

Castle, Nikki. 2017. Supervised vs. Unsupervised Machine Learning

Chen, Edwin. Choosing a Machine Learning Classifier

Donges, Niklas. 2018. The Random Forest Algorithm 

Google Developers. 2018. Classification: Precision and Recall

Google Developers. 2018. Classification: ROC and AUC 

Barella, Victor Hugo. 2015. Técnicas para o problema de dados desbalanceados em classificação hierárquica

Portella, Letícia.2018. Machine Learning Models - My Cheat List

Sachanm Lalit, 2015. Logistic Regression vs Decision Trees vs SVM: Part II

Statinfer, 2017. SVM : Advantages Disadvantages and Applications

Simplilean. 2018. KNN Algorithm

Singh, Aishwarya. 2018. A Comprehensive Guide to Ensemble Learning 

Souza, Jocelyn D’. 2018.  Let’s learn about AUC ROC Curve! 

### **PS:**
This is my conclusion project to Nanodegree in Machine Learning Engineer from Udacity, please, let me know with you have any doubts or sugestion. All code you can find in my Github [here.](https://github.com/shiguelita/credit_card_fraud_detection). 

Thanks for reading! See ya! ^^