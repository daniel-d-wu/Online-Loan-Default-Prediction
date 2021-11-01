# Can a Machine replace a Loan Officer?

In this project, I fit three models to predict charge-offs in Lending Club loans: A (Regularized) Logistic Regression, Random Forest (RF) and Extreme Gradient Boosting (XGBoost). The purpose of this project is twofold: First, it is to see how well each model performs in comparison to one another. Secondly, I incorporate <b><i>soft information</i></b> in the form of text descriptions provided by the applicant to see whether that improves model performance. The main insight from this project is that incorporating bigrams as features in each model improves ``Recall``, but decreases ``Precision``. The implication is that using text features will "over-predict" charge-offs at the expense of foregone applicants that are otherwise creditworthy.    
 
 ### Prediction without Text information
  
  Somewhat surprisingly, I find that Random Forests perform the best. This may be due to charge-offs being a relatively rare event. In my sample, 5,627 out of 39,717 loans are charged off (~14%), requiring me to rebalance the classes. I obtain the following results for each model:
  
  
  ![text](https://github.com/daniel-d-wu/Online-Loan-Default-Prediction/figures/Logit_Confusion_Matrix_notext.jpg)

  
  
  
  
  
  
  
  
  
  
  

  
  
  
