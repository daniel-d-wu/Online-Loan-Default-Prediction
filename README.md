# Can a Machine replace a Loan Officer?

In this project, I attempt to predict loan default using statistical learning methods. I fit three models on Lending Club's loan performance data: A (Regularized) Logistic Regression, Random Forest (RF) and Extreme Gradient Boosting (XGBoost). In the first part of this project, the objective is to test which model performs the best. In the second part, I incorporate text information from loan descriptions as features to use for prediction to test whether this boosts performance. Specifically, I incorporate the bigrams with the highest discriminating power based the following simple naive bayes calculation:   

![text](https://latex.codecogs.com/svg.latex?\frac{P(bigram|defaulted)}{P(bigram|repaid)}) 

## Results and Main Insight:

In the first part, I find that Random Forest (somewhat) surprisingly outperformed XGBoost. This is likely because charge-offs are relatively rare events. In the 2007-2011 Lending Club sample, there are 5,627 out of 39,717 loans that are charged off (~14%), requiring me to upsample charged off datapoints. XGBoost is likely overfitting to the noise, whereas RF is less prone to this issue. 

In the second part, I ask whether soft information in the form of text descriptions can be used to boost prediction. Specifically, I incorporate bigrams that are most indicative of charge-off and repayment. Before running the models, I find some interesting patterns in the text descriptions of borrowers that default versus borrowers that do not. Bigrams associated with high odds of default mention two key things: 1) <b>family</b> and 2) <b>uncertainty</b>. For instance, words such as "<i><b>stress</b></i>", "<i><b>emergency</b></i>", "<i><b>surgery</b></i>", "<i><b>home</b></i>", "<i><b>family</b></i>", "<i><b>worry</b></i>" and "<i><b>wife</b></i>" are featured in the top 25 bigrams associated with high odds of charge-off. <br> <br> In contrast, the bigrams associated with low odds of defaulting use specific terminology related to repayment, with words such as "<i><b>efficient</b></i>", "<i><b>reduce</b></i>", "<i><b>save</b></i>", "<i><b>cover</b></i>", "<i><b>sell</b></i>", "<i><b>completion</b></i>" and "<i><b>working</b></i>". Additionally, a simple compairson of bigrams suggests that some types of loans, such as  home improvements are more likely to repay back. The top bigram associated with repayment is <b><i>("energy", "efficient")</b></i>. 

![default_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/chgoff_bigrams.jpg)

![paid_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/paid_bigrams.jpg)

Incorp

The main insight from this project is that incorporating bigrams as features in each model improves ``Recall``, but decreases ``Precision``. The implication is that using text features will "over-predict" charge-offs at the expense of foregone applicants that are otherwise creditworthy.    
 
 ### Part 1 Prediction without Text information
  
  
  
  
![logit_1](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/Logit_Confusion_Matrix_notext.jpg)
  
<b>Precision</b>:  0.56 <br>
<b>Recall</b>:  0.43 <br>
  
![rf_1](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/Random_Forest_Confusion_Matrix_notext.jpg)

<b>Precision</b>:  0.97 <br>
<b>Recall</b>:  0.82 <br>

![xgb_1](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/XGBoost_Confusion_Matrix_notext.jpg)

<b>Precision</b>:  0.84 <br>
<b>Recall</b>:  0.66 <br>
  
  
  
  
  
  
  
  
  
  

  
  
  
