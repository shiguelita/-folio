---
layout: post
title: Detecting employee turnover
date: 2019-08-19 15:00:00
author: Talita Shiguemoto
description: PART 1 - Exploratory data analysis and statistical modeling 
comments: true
ref: employeeTurnover
lang: en
---

According to the Forbes website (2018) in a survey conducted by BambooHR in the United States of America, 17% of workers quit in the first six months of hiring say that “a smile from a co-worker could have made all the difference. ”(Forbes, 2018), another 23% say that more effective communication with the manager could have changed their decision to leave, and another 21% blame inefficient training.

The goal of this project is to statistically analyze what are the possible reasons for spontaneous dismissal and create an algorithm to detect which professionals can have positive turnover. With this information a company has the possibility to understand the reasons of the turnover rates, being possible to create actions to prevent the unwanted cases.

This project will be divided into two parts, the first one referring to exploratory data analysis, which will discovery the hypotheses that will be statistically tested. All procedure will be done in R language.

The second part will be the creation of the machine learning model to predict turnover employees. In this part the analysis will be done in python language.


### **Database and variables description**
<br/>

The dataset refers to fictitious employee turnover information created by IBM data scientists, consisting of 1,470 observations and 35 variables. Data can be obtained from [Kaggle](https://github.com/shiguelita/IBM_HR_analysis).

#### **Qualitative variables**
<br/>
**Nominals**

- Attrition: shows if employee left the company spontaneously.
- Department: employee departament. 
- EducationField: education field. 
- EmployeeNumber: employee ID.
- Gender: gender. 
- JobRole: job role. 
- MaritalStatus: marital status. 
- Over18: if employee is 18 years old or more.
- OverTime: if employee do overtime work.

**Ordinals**

- BusinessTravel:  frequency level the employee travels for work.
- Education: education level.
- EnvironmentSatisfaction: satisfaction level with work environment.
- JobInvolvement: involvement level with the work. 
- JobLevel: job hierarchy level.
- JobSatisfaction: satisfaction level with work.
- PerformanceRating: performance level.
- RelationshipSatisfaction: nsatisfaction level with relationship between employess.
- StockOptionLevel: stock purchase level of the company.
- WorkLifeBalance: balance between personal and work life.

#### **Quantitative Variables**
<br/>
**Continuous**

- Dailyrate: daily rate paid.
- DistanceFromHome: distance from work to home.
- HourlyRate: hour value paid.
- MonthlyIncome: monthly income.
- MonthlyRate: monthly allowance.
- PercentSalaryHike: percentage of salary increase.
- StandardHours: number of standard hours worked.


**Discrete**

- Age: age of the employee.
- EmployeeCount: employee count on observation.
- NumCompaniesWorked: number of companies the employee have worked for.
- TotalWorkingYears: total years worked.
- TrainingTimesLastYear: total training in last year.
- YearsAtCompany: total years in the company.
- YearsInCurrentRole: total years in current position.
- YearsSinceLastPromotion: total years since last promotion.
- YearsWithCurrManager: total years with the current manager.


### **Exploratory Data Analysis (EDA)**
<br/>
The first item checked was if there was any missing value in the dataset. As we can see in **Chart 1**, there is no feature with missing values.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project002_01_pt.png" alt="" title="Chart 1 - Missing Values check"/>
</div>
<div class="col three caption">
	Chart 1 - Missing Values chek
</div>
<br/>

Through the descriptive analysis of all quantitative variables, seen in **Table 1**, it can be concluded that:
1. There are no minors in the database as the minimum age is 18 years old;
2. The variables EmployeeCount and StandardHours have a standard deviation of 0, in other words, there is no variation, all observations of these features are equal (1 and 80 respectively), so we can remove these variables from the analysis;
3. The variables “DailyRate”, “DistanceFromHome”, “MonthlyRate”, “NumCompaniesWorked” and “PercentSalaryHike” have a coefficient of variation above 25%, and the features with greater variability, in a future analysis we will verify if this variation stays among people with positive and negative “Attrition”.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project002_02_pt.png" alt="" title="Table 1 - Descriptive statistics"/>
</div>
<div class="col three caption">
	Table 1 - Descriptive statistics
</div>
<br/>

In order to predict which employees are leaving the companie, we need to detect the classes of the feature “Attrition”, in **Chart 2** we can notice that the relative frequency difference between two classes is high, being 83.9% for “No” and 16.1% for “Yes”, such behavior is something companies want, with the smallest possible positive response.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_03.png" alt=""  title="Chart 2 - Attrition variable balancing"/>
</div>
<div class="col three caption">
	Chart 2 - Attrition variable balancing
</div>
<br/>

The market trend for large cities is that workers are looking for opportunities closer to their home, a formulated hypothesis would be if the distance from work to home interferes with company turnover. We can note from **Chart 3** that when the turnover is positive the distance in the third quartile is greater by approximately 5 units, this assumption will be tested later.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_04.png" alt="" title="Chart 3 - oxplot Distance from Home by Turnover"/>
</div>
<div class="col three caption">
	Chart 3 - Boxplot Distance from Home by Turnover
</div>
<br/>

Looking at **Chart 4**, we found that for positive turnover, 50% of people are in their first job, leading to the hypothesis that people leaving the company have on average fewer previous jobs.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_05.png" alt="" title="Chart 4 - Boxplot Number of companies worked by Turnover"/>
</div>
<div class="col three caption">
	Chart 4 - Boxplot Number of companies worked by Turnover
</div>
<br/>

Another formulated hypothesis that shows it when we are exploring the monthly income by turnover in **Chart 5** is that the lowest earner is more likely to leave the company, since only 50% of employees who stay earn up to 5000 while almost 75% of those who leave get paid the same value.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_06.png" alt="" title="Chart 5 - Boxplot Monthly Income by Turnover"/>
</div>
<div class="col three caption">
	Chart 5 - Boxplot Monthly Income by Turnover
</div>
<br/>

Most ordinal variables are classified by numbering, to facilitate the exploratory analysis of the data, the description of each in the following tables was documented, also showing the absolute and relative frequency that each class appears.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_07.png" alt="" title="Table 2 – Education frequency table "/>
</div>
<div class="col three caption">
	Table 2 – Table de frequência Education
</div>
<br/>

To better understand the universe of the analyzed data, we see in **Table 2** that the largest concentration of professinals is at the Bachelor educational level followed by the Master while analyzing **Table 3**, we see that the education field largest part of employees is in "Life Sciences" and "Medical".

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_08.png" alt="" title="Table 3 - EducationField frequency table"/>
</div>
<div class="col three caption">
	Table 3 - EducationField frequency table
</div>
<br/>

When we look at employee satisfaction, there are three dimensions captured in the dataset, namely: EnvironmentSatisfaction, JobSatisfaction, and RelationshipSatisfaction. **Table 4** shows the description of satisfaction levels, with 1 being the lowest and 4 being the highest. Observing the frequency of the classes it is possible to notice that the behavior between the three variables is similar.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_09.png" alt="" title="Table 4 - Satisfaction variables frequency table"/>
</div>
<div class="col three caption">
	Table 4 - Satisfaction variables frequency table
</div>
<br/>

When analyzing the levels of satisfaction with work turnover in **Figure 1** it can be seen that the class with the lowest satisfaction has a higher frequency of “Yes” in all Charts presented, such exploratory analysis gives helps to formulate the hypothesis that if there is any statistical difference between employee satisfaction levels and turnover. This possibility will be check in statistical modeling.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_10.png" alt="" title="Figure 1 - Charts de Satisfação vs Rotatividade"/>
</div>
<div class="col three caption">
	Figure 1 - Charts de Satisfação vs Rotatividade
</div>
<br/>

As we dig deeper into the variables that refer to how employees engage with their work, there are three that actually show this scenario: JobLevel, JobInvolvement and e PerformanceRating. Such features are described in **Table 5**.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_11.png" alt="" title="Table 5 - Frequência de variáveis sobre cargo e comportamento"/>
</div>
<div class="col three caption">
	Table 5 - Frequência de variáveis sobre cargo e comportamento
</div>
<br/>

Given the frequency shown and job levels in **Table 5**, we can conclude that 73% of employees are in the lowest positions, performance has only two classes, being excellent and non-standard, in other words, good classes. Finally, 69% of employees have a high or very high job involvement.

Positions with higher levels usually receive more, such point is also seen in the dataset, shown in **Chart 6**. The highest position (5) is the one with the highest monthly income and is held by older people, starting at approximately 40 years.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_12.png" alt="" title="Chart 6 - RMonthly Income vs Age by Job Level"/>
</div>
<div class="col three caption">
	Chart 6 - Monthly Income vs Age by Job Level
</div>
<br/>

Even though the performance level is high for both levels, most employees have level 3, which is 85% of the total. Many companies have a policy of the highest increase salary for the top performers. When we look at **Chart 7** we notice that the data studied also follows this pattern, with the percentage of salary increases considerably higher for the performance level. 4


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_13.png" alt="" title="Chart 7 - Boxplot Performance vs Salary Increase (%)"/>
</div>
<div class="col three caption">
	Chart 7 - Boxplot Performance vs Salary Increase (%)
</div>
<br/>

As we explore the relationship between personal life and work, in the variable “WorkLifeBalance” in **Table 6**, it can be seen that few have the best level of balance, yet 61% are in the second best class. In the same table we see that most employees do not work overtime and hardly travel for work.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_14.png" alt="" title="Table 6 - WorkLifeBalance, travel frequency and overtime"/>
</div>
<div class="col three caption">
	Table 6 - WorkLifeBalance, travel frequency and overtime
</div>
<br/>

By analyzing the frequency of employees who work overtime due to turnover in **Chart 8**, it is possible to realize that over 50% of those who leave work had a positive result if they worked longer hours, leading to the hypothesis that working overtime can impact in turnover.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_15.png" alt="" title="Chart 8 - Overtime Frequency by Turnover"/>
</div>
<div class="col three caption">
	Chart 8 - Overtime Frequency by Turnover
</div>
<br/>

In **Table 7** we have the frequency per department, most of which are in Research & Development and even having only three areas in the dataset, there are a variety of positions.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_16.png" alt="" title="Table 7 - Departament and Job Role frequency table"/>
</div>
<div class="col three caption">
	Table 7 - Departament and Job Role frequency table
</div>
<br/>

**Table 8** shows data related to individuals as persons. The first point highlighted is that all employees are over 18 years old, a fact already confirmed in **Table 1**, so we can remove this variable from the analysis. Another interesting point is that there is a certain gender balance, not having a very big difference between men and women. Most marital status has 46% of married people with regard to the possibility of buying shares, 43% are at level 0, ie without purchase rights, and another 41% can buy only the most basic level of shares.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_17.png" alt="" title="Table 8 - Gender, Marital status, Over 18 and Stock Options"/>
</div>
<div class="col three caption">
	Table 8 - Gender, Marital status, Over 18 and Stock Options
</div>
<br/>

<br/>

### **Statistical Modeling**
<br/>

Through exploratory data analysis five hypotheses were formulated:
1. Is the average distance between work and home statistically equal between employees with positive and negative turnover?
2. Is there a statistical difference in the average satisfaction levels (with the environment, work or relationship) between the positive and negative employee turnover?
3. Is the average of previously worked companies statistically the same among employees who leave the company or not?
4. Is there a statistical difference in average monthly income among employees with turnover?
5. Do overtime employees have a higher statistical chance of leaving the company?

#### **Analysis of normality of distributions**
<br/>
To choose which statistical method would be used, it was first necessary to verify the data distribution, if the assumption of normality was true. Because of this, the following steps were as suggested by the author Pino (2014):

> “To check if the distribution is normal, the first thing to do is a Observation Frequency Chart, to examine if there are asymmetries. In suspecting the existence of nonnormality, the next step is to test the null hypothesis that the distribution of observations is normal against the alternative hypothesis that it is not. Adherence (or adjustment) testing is called the problem of testing the hypothesis that a given sample comes from a population with a specific density function. ”

As we can analyze in **Chart 9** and **Chart 10**, the dependent variables of this study do not have a normal distribution, and in the first Chart, all features are ordinal, while in the next Chart the histograms have a positive asymmetric format.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_18.png" alt="" title="Chart 9 - Satisfaction distribution variables"/>
</div>
<div class="col three caption">
	Chart 9 - Satisfaction distribution variables
</div>
<br/>

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_19.png" alt="" title="Chart 10 - Distribuição variáveis"/>
</div>
<div class="col three caption">
	Chart 10 - Distribution variables
</div>
<br/>

The adherence test used was the Shapiro-Wilk test, which according to Pino (2014), is a regression and correlation-based test, which uses the ratio of a weighted least squares estimate and another estimate without sample variance bias. Such test has as equation:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_20.png" alt="" title="Shapiro-Wilk equation"/>
</div>
<div class="col three caption">
	Shapiro-Wilk equation
</div>
<br/>

where c'= (c1, ... cn) and V are, according to Pino (2014) respectively, vectors of expected values and the covariance matrix of standard standard order statistics.

The results of the Shapiro-Wilk test for the 6 variables can be seen in **Table 9**, and the null hypothesis (Ho) that the distribution is normal is rejected for all tests due to p-value < 0.05.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_21.png" alt="" title="Table 9 - Shapiro-Wilk Test results"/>
</div>
<div class="col three caption">
	Table 9 - Shapiro-Wilk Test results
</div>
<br/>


It is possible to transform the data so that it obtains a normal distribution, since such transformations “can improve the approximation of the likelihood functions normality, as well as the accuracy of theory-based significance levels and confidence intervals for large samples.”(Pino, 2014).

As all variables analyzed are positive, the transformation chosen was the logarithmic, and its equation:

\begin{align}
\dot{Y} & = 	{log⁡(x+c)}
\end{align}

where if the minimum value presented is 0, c will be equal to 1, otherwise c = 0.

Even after transformation, when plotting the histograms according to **Chart 11** and  **Chart 12**, data don't have a normal distribution.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_23.png" alt="" title="Chart 11 - Histogram satisfaction variables in log"/>
</div>
<div class="col three caption">
	Chart 11 - Histogram satisfaction variables in log
</div>
<br/>


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_24.png" alt="" title="Chart 12 - Histogram variables in log"/>
</div>
<div class="col three caption">
	Chart 12 - Histogram variables in log
</div>
<br/>


The Shapiro-Wilk test results for the 6 log-transformed variables can be seen in Table 10, and the null hypothesis (Ho) that the distribution is normal is rejected for all tests due to p-value <0 .05.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_25.png" alt="" title="Table 10 - Shapiro-Wilk Test Results on log"/>
</div>
<div class="col three caption">
	Table 10 - Shapiro-Wilk Test Results on log
</div>
<br/>

#### **Mann-Whitney U test**
<br/>

The Mann-Whitnet U test according to Milenovic (2011) is a nonparametric statistical technique, used to analyze if there is a difference between the medians of two independent samples, for when the distribution of the response variable is not normal, which invalidates the use Student's t test.
In order to apply the Mann-Whitnet U test there are four assumptions that need to be met, which according to Laerd (2013) are:
1. The dependent variable must be ordinal or continuous;
2. The independent variable must be categorical, with two independent classes;
3. Observations need to be independent between classes, in other words, there can be no relationship between the observations of each group tested, as for example there must be different people in each group;
4. Data distribution is not normal and should have the same format as in **Figure 2**.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_26.png" alt="" title="Figure 2 - Distribution shapes"/>
</div>
<div class="col three caption">
	Figure 2 - Distribution shapes
	<br/>
	<a href="https://statistics.laerd.com/spss-tutorials/mann-whitney-u-test-using-spss-statistics.php">Fonte: Laerd, 2013</a>

</div>
<br/>

As noted in the previous, the dependent variables do not have a normal distribution, beeing RelationshipSatisfaction, JobSatisfaction and EnvironmentSatisfaction are ordinals and DistanceFromHome, NumCompaniesWorked and MonthlyIncome are continuous.

The hypothesis tests will be tested with the independent variable Attrition, and all observations are independent between classes, as well as each class is independent of each other.
The last assumption that distributions need to have the same format has been proven through **Chart 13** and **Chart 14**.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_27.png" alt="" title="Chart 13 - Distribution of satisfaction variables by Attrition"/>
</div>
<div class="col three caption">
	Chart 13 - Distribution of satisfaction variables by Attrition
</div>
<br/>


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_28.png" alt="" title="Chart 14 - Distribution of continuous variables by Attrition"/>
</div>
<div class="col three caption">
	Chart 14 - Distribution of continuous variables by Attrition
</div>
<br/>
Once all assumptions were ok, the Mann-Whitney U test was applied for all hypotheses, with the results shown in **Table 11**.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_29.png" alt="" title="Table 11  - Mann-Whitney U test results"/>
</div>
<div class="col three caption">
	Table 11  - Mann-Whitney U test results
</div>
<br/>

The null hypothesis (Ho) that RelationshipSatisfaction is statistically the same in both Attrition groups is accepted, since we reject the alternative hypothesis (Ha) because of p-value = 0.102, that is, greater than 0.05.

In the other two categories of JobSatisfaction and EnvironmentSatisfaction, the p-value of both is 0.000, so the null hypothesis (Ho) is rejected and the model is significant. In **Chart 15** we see that the average satisfaction rate for those with negative Attrition is higher in both cases.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_30.png" alt="" title="Chart 15 - Satisfaction Average by Attrition"/>
</div>
<div class="col three caption">
	Chart 15 - Satisfaction Average by Attrition
</div>
<br/>


The model that verifies whether NumCompaniesWorked is statistically different between groups is not significant, with p-value = 0.242 we reject the alternative hypothesis (Ha) in favor of the null hypothesis (Ho).

The null hypothesis (Ho) that the DistanceFromHome is statistically equal among employees who leave the company or is not rejected because the p-value in the test results in 0.002, ie p-value. <0.05, being the significant model. When analyzing the average MonthlyIncome between the two groups, the p-value <0.05 rejects the null hypothesis, showing that there is significance in the model and that the groups with positive and negative turnover have the monthly income. different.

**Chart 16** shows that the positive turnover group lives on average farther than the other group and receive a lower average monthly income.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_31.png" alt="" title="Chart 16 - Average by Attrition"/>
</div>
<div class="col three caption">
	Chart 16 - Average by Attrition
</div>
<br/>


#### **Pearson's Chi-Square Test and Odds Ratio**
<br/>

Pearson's Chi-square test is a nonparametric test that according to author McHugh (2013), is one of the most powerful hypothesis tests for two qualitative variables, using a contingency table. The Pearson Chi-Square test assumptions that need to be met, which according to McHugh (2013) are:
1. Dependent and independent variables must be qualitative;
2. Observations need to be independent between groups, just as such groups need to be independent of each other;
3. All contingency table cells must have a minimum count of 1;
4. All expected cells must have a minimum count of 5;
	* If this assumption is not met, the Fishers test can be used.

The last hypothesis to be tested is whether employees who work overtime are more likely to leave the company. Being the hypotheses:
- Ho: Chance of turnover being positive for working overtime equals chance of negative turnover
- Ha: The chance of turnover being positive for working overtime is different from the chance of negative turnover

Both variables are nominal, both having the value Yes and No, which meets assumption number 1. All groups as observations of them are independent, meeting assumption 2. The third assumption is met in **Table 12**, where no cell has a value less than 1.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_33.png" alt="" title="Table 12 - Contingency Table"/>
</div>
<div class="col three caption">
	Table 12 - Contingency Table
</div>
<br/>

By applying Pearson's Chi-square test, the results can be seen in **Table 13**. Due to p-value <0.05 the null hypothesis is rejected, being the significant model and we have evidence that there is statistical difference between the groups that work overtime and leave the company.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_34.png" alt="" title="Table 13 - Pearson Chi-Square Test Result"/>
</div>
<div class="col three caption">
	Table 13 - Pearson Chi-Square Test Result
</div>
<br/>

**Table 14** shows the contingency table of expected values, and no cell has a value less than five, so the model meets the last assumption described by McHugh (2013), and can then rely on the health of the model.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_35.png" alt="" title="Table 14 - Expected Values Contingency Table"/>
</div>
<div class="col three caption">
	Table 14 - Expected Values Contingency Table
</div>
<br/>

Since we can confirm that there is a statistical difference between the groups, to answer the question of what is the chance of overtime employees leaving the company compared to the other group, we need to use the test called Odds Ratio (OR).

The Odds Ratio is according to Colosimo (2006), the chance of an event occurring between observations exposed to a factor, divided by the chance of that same event occurring between observations not exposed to that factor. The equation beeing represented by:

\begin{align}
\dot{OR} & = 	\frac{P(A|B)/P(A'|B)}{P(A|B')/P(A'|B')} 
\end{align}

where A = event occurs, A '= event does not run, B = factor exposed and B' = not factor exposed.

**Table 15** shows the Odds Ratio test result, which concludes that the chance of the overtime employee leaving the company is 3.77 times greater than the chance of the non-overtime employee .

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_36.png" alt="" title="Table 15 - Odds Ration result"/>
</div>
<div class="col three caption">
	Table 15 - Odds Ration result
</div>
<br/>

### **Reference**

BAUER, David F. **Constructing confidence sets using rank statistics**. Journal of the American Statistical Association 67, 687–690. 1972. 

COLOSIMO. Enrico A. P**rincípios de Bioestatística - Medidas de efeito: Risco Relativo e Razao de Chances**. Universidade Federal de Minas Gerais UFMG. 2006.

FORBES. **How AI Can Help Redesign The Employee Experience**. 2018. Disponível em: <https://www.forbes.com/sites/insights-intelai/2018/11/29/how-ai-can-help-redesign-the-employee-experience/#6293c9c4b34b>. Acesso em: 01 de agosto de 2019.

LAERD, Statistics. **Mann-Whitney U Test using SPSS Statistics**. 2013. Disponível em: <https://statistics.laerd.com/spss-tutorials/mann-whitney-u-test-using-spss-statistics.php>. Acesso em: 01 de agosto de 2019.

MCHUGH, Mary L. **The Chi-square test of independence**. Rev. Biochemia Medica, Califórnia, v. 23, n. 2, p. 143-149,abri-jun. 2013.

MILENOVIC, Zivorad M. **Application of Mann-Whitney U Test in research of Professional Training of Primary School Teachers**. Faculty of Education. Leposavic, 2011.

PINO, Francisco Alberto. **A QUESTÃO DA NÃO NORMALIDADE: uma revisão**. Rev. de Economia Agrícola, São Paulo, v. 61, n. 2, p. 17-33, jul.-dez. 2014


### **PS:**
All the code you can find on my github [here.](https://github.com/shiguelita/IBM_HR_analysis). 

Thank you! See ya! ^^