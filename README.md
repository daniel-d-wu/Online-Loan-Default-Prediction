# Online-Loan-Default-Prediction

This repo stores my progress in comparing Logit and XGBoost in predicting default in online loans. I use 2007-2011 Lending Club data to test these two models. So far, I find that XGBoost does a much better job at predicting default with the same features. The performance is as follows:

Logit:

Precision:  0.5469662921348315 <br>
Recall:  0.5528467595396729 <br>
AUC:  0.5613728170916938

XGBoost:

Precision:  0.8299625468164794 <br>
Recall:  0.7542545949625595 <br>
AUC:  0.7853303611829847

