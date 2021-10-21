# Online-Loan-Default-Prediction

This repo stores my progress in fitting a logistic regression and XGBoost model to predict loan default in online personal loans. I use 2007-2011 Lending Club data to test these two models. The motivation for this exercise comes from a desire to understand exactly how much cutting-edge ML models improve performance compared to traditional statistical models. It turns out that XGBoost indeed does a much better job than Logit in the same feature space. The performance is as follows:

Logit:

Precision:  0.5469662921348315 <br>
Recall:  0.5528467595396729 <br>
AUC:  0.5613728170916938

XGBoost:

Precision:  0.8299625468164794 <br>
Recall:  0.7542545949625595 <br>
AUC:  0.7853303611829847

