## Loan Performance Prediction: Can a Machine replace a Loan Officer?

### 1. Goal:

There are two objectives in this project.

1) Design a statistical learning model to predict binary loan performance indicators (charge-offs and paid-off) using LendingClub's unsecured personal loans from 2007-2011. 

2) Test how well text features enhance loan performance prediction. 

### 2. Defining a Performance metric:

Accurately predicting loan performance is an important function to all stakeholders. On one hand, it is important to extend credit to those that are creditworthy, in order to not only maximize revenue, but also provide individuals with opportunities and prevent credit rationing. On the other, it is also critical to screen out uncreditworthy applicants, to avoid losses and adverse welfare effects to the delinquent borrower <b><i>(Stiglitz and Weiss 1981)</i></b>. ``Accuracy``, ``AUROC`` and ``F1`` appear to be great candidates for performance metrics since they take into account both concerns. However, with imbalanced datasets, these composite measures are misleading. A naive model that blindly predicts the majority label can achieve a high score. For instance, in 2007-2011 Lending Club data, roughly 14% of datapoints are charged-off, and a naive model would be able to achieve 86% accruacy. Thus, I choose ``Recall'', the rate at which charged-off borrowers are correctly predicted by the model, as my performance metric.     

### 3. Data preprocessing and EDA:

Before designing the models, I take a few steps to address missing values and outliers. I drop columns with missing values and impute values for rare events such as Bankruptcies and Collections with 0 instead of the mean. Eventually, all of these imputed variables have such low variance that I decide not to include them as features. For variables with considerably large variance such as income, number of accounts and revolving balance, I winsorize at the 1% and 99% levels in order to reduce the effect of outliers. Additionally, I explore LendingClub's free form text field that allows the applicant to write a note to prospective investors. I construct bigrams following <b>Netzer, Lemaire and Herzenstein 2019</b>, and calculate the odds of default for each bigram with the following formula:

![text](https://latex.codecogs.com/svg.latex?\frac{P(bigram|defaulted)}{P(bigram|repaid)}) 

For the top 50 bigrams with the highest odds of default, bigrams involving family <b>(('husband', 'worked'), ('emergency', 'family'))</b>, uncertainty <b>(('free', 'worry'), ('like', 'stress'))</b> and extremities <b>(('always', 'stay')</b>, <b>('cards', 'always'))</b> appear, while the top 50 bigrams with the lowest odds of default feature specific credit terminology and verbs <b>(('rate', 'good')</b>, <b>('save', 'enough')</b>, <b>('reduce', 'overall'))</b>.

![default_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/chgoff_bigrams.jpg)

![paid_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/paid_bigrams.jpg)


### 4. Feature Engineering:

I exclude columns that are not helpful, such as Example IDs, and columns with high collinearity with other variables, leaving me with important credit and personal characteristics such as DTI, Loan Amount, Loan Purpose and Years of Employment. 

In addition to Financial features, I create an expanded set of features using the top 200 most common bigrams found in the free form field, along with the top 50 bigrams with the highest (and lowest) odds of default constructed in the EDA section. I run a simple decision tree classifier with only text features, in order to determine the (feature) importance of each bigram, and select the top 50 bigrams with the highest importance.

### 5. Performance Results:

The best model was Random Forest with a ``Recall`` of 82.4%. XGBoost had ``Recall`` of 62% and Logistic regression had a ``Recall`` of 54%.

### 6. Text Enhancement Results:

``Recall`` for the XGBoost model improved by 2.0% after the addition of text features, while ``Recall`` increased by 1.2% for the Random Forest model. This suggest although Random Forests performed better overall, additional improvements that incorporate non-traditional/alternative data may be better incorporated in a gradient boosting model.

