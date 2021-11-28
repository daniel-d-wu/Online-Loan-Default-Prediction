# Loan Performance Prediction: Can a Machine replace a Loan Officer?

### Objective:

The purpose of this project is twofold. First, I design statistical learning models to accurately predict binary loan performance indicators (charge-offs and paid off) using LendingClub's early loans (2007-2011). Secondly, I engineer additional text features to "test" how well textual information improves loan performance prediction. To that end, I choose predictive models that have methods to infer, or at least interpret, the impact of features on prediction performance, including Logistic Regression, Random Forests and Gradient Boosting.  

### Data preprocessing and EDA:

Before designing the models, I take a few steps to address missing values and outliers. I drop columns with missing values and impute values for rare events such as Bankruptcies and Collections with 0 instead of the mean. Eventually, all of these imputed variables have such low variance that I decide not to include them as features. For variables with considerably large variance such as income, number of accounts and revolving balance, I winsorize at the 1% and 99% levels in order to reduce the effect of outliers. Additionally, I explore LendingClub's free form text field that allows the applicant to write a note to prospective investors. I construct bigrams following ``Netzer, Lemaire and Herzenstein 2019``, and calculate the odds of default for each bigram with the following formula:

![text](https://latex.codecogs.com/svg.latex?\frac{P(bigram|defaulted)}{P(bigram|repaid)}) 

For the top 50 bigrams with the highest odds of default, bigrams involving family <b>(('husband', 'worked'), ('emergency', 'family'))</b>, uncertainty <b>(('free', 'worry'), ('like', 'stress'))</b> and extremities <b>(('always', 'stay')</b>, <b>('cards', 'always'))</b> appear, while the top 50 bigrams with the lowest odds of default feature specific credit terminology and verbs <b>(('rate', 'good')</b>, <b>('save', 'enough')</b>, <b>('reduce', 'overall'))</b>.

![default_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/chgoff_bigrams.jpg)

![paid_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/paid_bigrams.jpg)


### Feature Engineering:

I exclude columns that are not helpful, such as Example IDs, and columns with high collinearity with other variables, leaving me with important credit and personal characteristics such as DTI, Loan Amount, Loan Purpose and Years of Employment. 

In addition to Financial features, I create an expanded set of features using the top 200 most common bigrams found in the free form field, along with the top 50 bigrams with the highest (and lowest) odds of default constructed in the EDA section. I run a simple decision tree classifier with only text features, in order to determine the (feature) importance of each bigram, and select the top 50 bigrams with the highest importance.

### Performance Results:

The best model was Random Forest with an accuracy of 83%. XGBoost had an accuracy of 73% and Logistic regression had an accuracy of 54%.

### Text Enhancement Results:

The AUC for the XGBoost model improved by 1.5% after the addition of text features, while AUC only increase by roughly 0.3% for Random Forests. This suggest although Random Forests perform better with baseline characteristics, additional improvements that incorporate non-traditional/alternative data may be better incorporated in a gradient boosting model.


![ROC_curve_rf](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/ROC_curve_rf.jpg)

![ROC_curve_xgb](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/ROC_curve_xgb.jpg)






  

  
  
  
