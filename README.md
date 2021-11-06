# Can a Machine replace a Loan Officer?

In this project, I attempt to predict loan default using statistical learning methods. I fit three models on Lending Club's loan performance data: A (Regularized) Logistic Regression, Random Forest (RF) and Extreme Gradient Boosting (XGBoost). In the first part of this project, the objective is to test which model performs the best. In the second part, I incorporate text information from loan descriptions as features to use for prediction to test whether this boosts performance. Specifically, I include the top 200 most common bigrams and the top 25 bigrams with the highest discriminating power based the following simple calculation:   

![text](https://latex.codecogs.com/svg.latex?\frac{P(bigram|defaulted)}{P(bigram|repaid)}) 

## Results and Main Insight:

In the first part, I find that Random Forest (somewhat) surprisingly outperformed XGBoost. This is likely because charge-offs are relatively rare events. In the 2007-2011 Lending Club sample, there are 5,627 out of 39,717 loans that are charged off (~14%), requiring me to upsample charged off datapoints in the training data. XGBoost is likely overfitting to the noise, since upsampling does not provide additional variation from which systematic patterns can be learned. RF is less prone to overfitting, since independent trees are learned in parallel.

In the second part, I ask whether soft information in the form of text descriptions can be used to boost prediction. Specifically, I incorporate bigrams that are most indicative of charge-off and repayment. Before running the models, I find some interesting patterns in the text descriptions of borrowers that default versus borrowers that do not. Bigrams associated with high odds of default mention two key things: 1) <b>family</b> and 2) <b>uncertainty</b>. For instance, words such as "<i><b>stress</b></i>", "<i><b>emergency</b></i>", "<i><b>surgery</b></i>", "<i><b>home</b></i>", "<i><b>family</b></i>", "<i><b>worry</b></i>", "<i><b>wife</b></i>" and "<i><b>husband</b></i>" are featured in the top 25 bigrams associated with high odds of charge-off. <br> <br> In contrast, the bigrams associated with low odds of defaulting use specific terminology related to repayment, with words such as "<i><b>efficient</b></i>", "<i><b>reduce</b></i>", "<i><b>save</b></i>", "<i><b>cover</b></i>", "<i><b>sell</b></i>", "<i><b>completion</b></i>" and "<i><b>working</b></i>". Additionally, a simple comparison of bigrams suggests that some types of loans, such as  home improvements are more likely to repay. The top bigram associated with repayment is <b><i>("energy", "efficient")</b></i>. 

![default_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/chgoff_bigrams.jpg)

![paid_bigrams](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/paid_bigrams.jpg)

After including text features, I find that ``Precision`` is substantially improved in each model, but the change in ``Recall`` differs across models. In XGBoost, ``Precision`` improves from 0.65 to 0.75, but ``Recall`` decreases from 0.83 to 0.80. The trend is similar in the Logistic Regression model, where ``Precision`` improves substantially from 0.42 to 0.56, but ``Recall`` only improves from 0.55 to 0.57. In Random Forest Model ``Precision'' increases from 0.81 to 0.91 and ``Recall`` improves from 0.97 to 0.99, proving to be the best model for loan performance prediction.

These results suggest that text features are able to effectively identify creditworthy borrowers, reducing the opportunity cost of wrongfully labelling foregone applicants. However, an increase in the rate of mislabelling uncreditworthy applicants is a risk, as shown in the decrease in ``Recall`` of XGBoost (More type 2 errors, but fewer type 1 errors). For ease of communication, I abstract away a rigorous framework, and prescribe the following business recommendation for loan companies that have text data to potentially inform their decisions:

### Recommendation:
1) If the foregone cost of incorrectly labelling creditworthy applicants is proportionally great, it is favorable to include text features to help with predicting creditworthy applicants.

3) If the company's objective is aligned with social welfare, or if charge-offs are proportionally expensive, it is favorable to <b> not </b> include text features in their decision algorithms. <br>
 
 
 ### Part 1: Performance with only (Hard) Financial information
  
![logit_1](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/Logit_Confusion_Matrix_notext_11.6.21.jpg)
  
<b>Precision</b>:  0.42 <br>
<b>Recall</b>:  0.55 <br>
  
![rf_1](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/Random_Forest_Confusion_Matrix_notext_11.6.21.jpg)

<b>Precision</b>:  0.81 <br>
<b>Recall</b>:  0.97 <br>

![xgb_1](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/XGBoost_Confusion_Matrix_notext_11.6.21.jpg)

<b>Precision</b>:  0.65 <br>
<b>Recall</b>:  0.83 <br>
  
![roc](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/ROC_curve.jpg)
  
### Part 2: Performance with (Hard) Financial information and (Soft) Textual information
  
![logit_2](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/Logit_Confusion_Matrix_text_text.11.6.21.jpg)
  
<b>Precision</b>:  0.56 <br>
<b>Recall</b>:  0.57 <br>
  
![rf_2](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/Random_Forest_Confusion_Matrix_text.11.6.21.jpg)

<b>Precision</b>:  0.91 <br>
<b>Recall</b>:  0.99 <br>

![xgb_2](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/blob/main/figures/XGBoost_Confusion_Matrix_text.11.6.21.jpg)

<b>Precision</b>:  0.75 <br>
<b>Recall</b>:  0.80 <br> 
  
  
  
  

  
  
  
