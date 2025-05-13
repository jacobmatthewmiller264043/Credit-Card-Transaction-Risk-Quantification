## Summary

This notebook uses a deep variational autoencoder to establish risk classes for credit card transactions. 
The dataset consists of 284,807 transactions with PCA transformed features, of which 492 are fraudulent.
The dataset is well known and can be found here: https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

## Project Goal and Summarized Results

In order to implement non-intrusive but high quality security safeguards for electronic transactions and especially financial transactions, a tiered risk classification system is more useful than a single classifier; a tiered approach enables less intrusive action to verify lower risk transactions and more severe restrictions on higher risk transactions. We wish to create a security protocol with high precision with respect to fraud but minimal intrusivity on non-fraud transactions. This notebook establishes three nested risk classes; Class 3 highest, Class 2 middle, Class 1 lowest risk tier. A possible associated security protocol could be outright refusal of transactions in Class 3, two-step verification for transactions in Class 2 and follow up verification for transactions in Class 1. The model clusters 89% of frauds in Class 3, 91% of frauds in Class 2 or higher and 93% of frauds in Class 1 or higher. Under the proposed security sheme, 5% of non-frauds would be impacted by refusal, 5% of non-frauds would be impacted by two-step verification and 5% of non-frauds would be impacted by follow up verification while 93% of frauds would be identified illustrating a non-intrusive but high security protocol.


## Technical Details 

We implement a variational autoencoder with Guassian prior and mean field Gaussian posterior to learn a latent space representation of the features. We do not expose the model to fraudulent cases during training and therefore representations are only explicltly learned for non-fraud data. We reonstruct the train set using the generative model and we save the 85th (p_1), 90th (p_2) and 95th (p_3) percentiles of the distrubution of reconstruction scores from the train set, x. 

We then reconstruct the test set (containing all fraudulent cases and 5% of non-fraud cases) using the generative model and save the test reconstruction scores y. We assign risk tiers according to the following scheme:

p_1 < y < p_2: Class 1
P_2 < y < p_3: Class 2
p_3 < y : Class 3

