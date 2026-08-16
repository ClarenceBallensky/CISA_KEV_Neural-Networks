# CISA Known Exploited Vulnerabilities Neural Networks
This repository contains my capstone portfolio for Codecademy's **Engineer Neural Networks with PyTorch and Transformers** skill path. 
Using the Cybersecurity and Infrastructure Security Agency's [Known Exploited Vulnerabilities (KEV) catalog](https://github.com/cisagov/kev-data), I fine-tuned four DistilBERT models: two models using a standard data split with learning rates of 1e-4 and 4e-5, and two models using a chronological data split with learning rates of 1e-4 and 4e-5. This project demonstrates data processing, model selection, model fine-tuning, model evaluation, and model refinement using Python.

## Former Work on the KEV Dataset
**Exploratory Analysis**
<br>
I previously performed an exploratory data analysis on the KEV dataset to identify trends in known exploited vulnerabilities, remediation-timelines, and ransomware-associated weaknesses. Find my exploratory analysis [here](https://github.com/ClarenceBallensky/CISA_KEV_Analysis).

My findings were as follows:
- Microsoft has the largest number of cataloged vulnerabilities.
- Input validation and use-after-free are the most common weaknesses.
- Ransomware vulnerabilities show similar remediation timelines to the broader catalog.
- Most remediation deadlines are exactly 21 days.
- Vulnerability additions peaked in 2022.

**Machine Learning**
<br>
I also built several machine learning models to classify ransomware status--including a logistic regression, random forest, and support vector machine model. I then compared performance. 
Find my machine learning models [here](https://github.com/ClarenceBallensky/CISA_KEV_Machine-Learning).

My models ranked as follows:
1) Logistic regression
2) Random forest
3) Support vector machine

My best model, logistic regression, produced the following scores:
<br>
Accuracy score: 0.8
<br>
Precision score: 0.5016611295681063
<br>
Recall score: 0.45481927710843373
<br>
f1 score: 0.47709320695102686
<br>

Note that accuracy alone is not a meaningful benchmark here, since a model that always predicts 'Unknown' would already score ~80%. The logistic regression model's real gains were in precision and recall--catching a meaningfully higher share of actual ransomware-associated CVEs than a naive baseline would.

## Design Decisions
Similarly to how I used the `class_weight='balanced'` argument in each of my models during my machine learning project, I included the `stratify` argument when I created my training and testing dataframes. This tells `train_test_split` to preserve the original ~80/20 class ratio in both the training and testing sets, so the model doesn't end up trained or evaluated on a split that is over-representative or under-representative of the minority class ("Known").

Using a slower learning rate (4e-5) proved to be more beneficial than using a faster learning rate (1e-4) across all metrics pertaining to both my standard split DistilBERT models and my chronological split DistilBERT models. 

## Model Performance Comparison
<img width="80%" alt="image" src="https://github.com/user-attachments/assets/90acc0e1-a201-438e-b3b1-078b3e0d35a7" />

*Figure 1. Comparison between the most successful versions of the logistic regression, standard DistilBERT, and chronological DistilBERT models.*

## Outcomes
Since both my standard and chronological fine-tuned models performed better across all metrics with a slower learning rate of 4e-5, I will only be discussing the models with a learning rate of 4e-5 from here on out. 

In the KEV dataset, "Known" values account for only about 20% of the data in the known_ransomware_campaign_use column; 80% is the default accuracy score for a model that defaults to "Unknown". Therefore, a high accuracy score is a misleading indicator of success. Recall score is a better reflection of model performance for my use case. 

The base model classified 0 out of the 70 known instances of ransomware campaign use in the test set. The standard model correctly classified 25 of the 70 instances in the test set, and the chronological model classified 18 of the 35 instances in the chronological test set. These demonstrate that fine-tuning the model was effective at improving the model's performance. However, the improvement is so slight that neither model could be reasonably deployed commercially. 

The plot above shows a comparison between my Logistic Regression model from my machine learning project, my DistilBERT model with a standard stratified split, and my DistilBERT model with a chronological stratified split. My standard DistilBERT model slightly surpassed my logistic regression model in accuracy and precision, but failed to match my logistic regression model's recall and f1 scores. Interestingly, my chronological DistilBERT model underperformed the other two models across all metrics except recall. This is pertinent because recall is the single most important metric for my use case. However, my chronological DistilBERT model's recall score only surpassed my logistic regression model's recall score by ~6 points, and the loss in other metrics is too significant to discount. My logistic regression model continues to surpass all other models in terms of well-roundedness. It is my preferred model for identifying ransomware status. 

Although neural networks are broadly considered more advanced than classic machine learning models, they do not always produce better outcomes. Machine learning still has a place in the field of data-based prediction. One limitation of the KEV dataset is that it is relatively small, with only 1665 entries. Neural networks thrive on vast amounts of data, with amount of data directly correlating to model performance. Additionally, the Trainer was run without a held-out validation set during training, so there was no way to monitor for overfitting or underfitting across epochs; future work could incorporate an evaluation split to track this.

Every problem is unique, and experimentation with different models is advisable.

## Skills Demonstrated
- Python 3
- Pandas
- Numpy
- PyTorch
- Transformers
- Scikit-Learn
- Data processing
- Model selection
- Model fine-tuning
- Model evaluation
- Interpreting and communicating results
- Jupyter Notebook

## How to Run
### Requirements
Internet access (the notebook downloads the dataset and pretrained model weights on first run)

**Software:**
- Jupyter Notebook
- Python 3

**Packages:**
```
pip install pandas
pip install numpy
pip install matplotlib
pip install scikit-learn
pip install torch
pip install transformers
pip install datasets
```
### Windows (Miniconda)
1. Clone this GitHub repository.
2. Install [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main).
3. Open **Anaconda Prompt**.
4. Install Jupyter Notebook: `conda install jupyter`.
5. Install the dependencies listed above.
6. Launch Jupyter Notebook: `jupyter notebook`.
7. Open `CISA_KEV_Neural-Networks.ipynb`.
8. Run all notebook cells from top to bottom. (Note: the neural network code may take up to 24 hours to run.)
