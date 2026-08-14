# CISA Known Exploited Vulnerabilities Machine Learning
This repository contains my capstone portfolio for Codecademy's **Build a Machine Learning Model** skill path. 
Using the Cybersecurity and Infrastructure Security Agency's [Known Exploited Vulnerabilities (KEV) catalog](https://github.com/cisagov/kev-data), I developed three classification machine learning models: a logistic regression model, a random forest model, and a support vector machine model. I selected the models based on my findings in my [exploratory data analysis project](https://github.com/ClarenceBallensky/CISA_KEV_Analysis), which I completed on the same dataset. This project demonstrates data processing, model selection, model evaluation, and model refinement using Python.

## Design Decisions
I added the `class_weight='balanced'` argument to each model. This tells `scikit-learn` to weigh the minority class ("Known", ~20% of the data) more heavily during training, so misclassifying it costs more than misclassifying the majority class. This tradeoff is worthwhile for my use-case because, in a cybersecurity context, false negatives are often more costly than false positives. False positives may result in time wasted, while false negatives may result in an undetected ransomware campaign. The former is an annoyance, while the latter is a security risk.

Using a stratified cross validation data split instead of a single data split proved to be beneficial across my logistic regression, decision forest, and support vector machine models--although it sometimes negatively impacted the f1 score. I favored the stratified cross validation split across all models for its balanced numbers. 

Increasing the decision threshold produced varying results across the three models. I implemented a threshold of 0.6 where appropriate. I left the remaining models at their default threshold of 0.5. 

## Model Performance Comparison
<img width="80%" alt="image" src="https://github.com/user-attachments/assets/8966f5be-5eb9-400a-8b5e-9bf989e857a6" />

*Figure 1. Comparison between the most successful versions of the logistic regression, random forest, and support vector machine models.*

## Outcomes
My models ranks as follows, in order of best scores to worst scores: 
1) Logistic regression
2) Random forest
3) Support vector machine

Accuracy alone is not a meaningful benchmark here, since a model that always predicts 'Unknown' would already score ~82%. The logistic regression model's real gains are in precision and recall--catching a meaningfully higher share of actual ransomware-associated CVEs than a naive baseline would.

One shortcoming of my models is that they give each vulnerability equal weight regardless of the `date_added` and `due_date` column. Giving more weight to recent vulnerabilities would better simulate the real deployment scenario of predicting the ransomware risk for new vulnerabilities.

## Skills Demonstrated
- Python 3
- Pandas
- Scikit-learn
- Model selection
- Data processing
- Classification model implementation
- Jupyter Notebook
- Interpreting and communicating results

## How to Run
### Required software:
- Jupyter Notebook
- Python 3
### Required packages:
```
pip install pandas
pip install matplotlib
pip install seaborn
pip install scikit-learn
pip install nltk
pip install beautifulsoup4
```
### Windows (Miniconda)
1. Clone this GitHub repository.
2. Install [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main).
3. Open **Anaconda Prompt**.
4. Install Jupyter Notebook: `conda install jupyter`.
5. Install the dependencies listed above.
6. Launch Jupyter Notebook: `jupyter notebook`.
7. Open `CISA_KEV_Machine-Learning.ipynb`.
8. Run all notebook cells from top to bottom.
