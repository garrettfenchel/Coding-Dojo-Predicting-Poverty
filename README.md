# Predicting Poverty

Purpose of this notebook is to complete final project for Coding Dojo Data Science and Machine Learning course. 

## Goal
Predict whether individuals from 7 different countries are below poverty line ($2.50/day), using household survey data. 

## Data 
Data originally comes from a Data Science competition hosted by Microsoft and Data Driven [(Data Science Capstone)](datasciencecapstone.org), and archived data was retrieved on Kaggle [(Kaggle Predicting Poverty)](https://www.kaggle.com/datasets/johnnyyiu/predicting-poverty?select=train_values_wJZrCmI.csv)  

Data containes 12,600 observations, with 59 predictor variables: ranging from age, sex, and education levels to financial activity, shocks, and income sources. 

Original outcome variable was the Poverty Probability Index, which is a probability (0 to 1) that the individual is below the poverty line, based on predictor variables. 



## Data Wrangling

### Outcome Variable

Instead of prediciting the Poverty Probaility Index, in order to tackle a different problem a new outcome variable was created (Poverty Bool) with a binary outcome: 0 ("Not Under Poverty Line"), or 1 ("Under Poverty Line"). This was done by assinging 0 to any observations under 0.50, and assigning 1 to any observations over 0.50. There were no observations exactly at 0.50. 


### Duplicates and Nulls 

No duplicate values were found. 

There were four columns (Bank, MM, MFI, and Other FSP interest rates) that contained more than 12,300 null observations. For this reason these columns were dropped from the analysis. After dropping these columns there still contained XXXX observations with null values. Due to the small quanity of null values, they were dropped from the analysis. 


### Data Transformation 

**Boolean**: All boolean variables that originally had values of "True" and "False", were converted to 1 and O respectively 

**Categorical**: All categorical variables were converted to integers, starting with 0 


### Combining and New Variable Creation

**Religion**: Due to the low frequency of "Religion" values "O" and "N", they were combined to one variable "O_N" 

**Math Skill**: There were 4 math skill boolean variables (can add, can divide, etc.), so a new variable "Math Skill" was created by adding up the boolean values (1 or 0) for each of the math skill variables

**Bank Skill**: There were 5 banking attribute boolean variables (active bank user, active mm user, etc.), so a new variable "Bank Skill" was created by adding up the boolean values (1 or 0) for each of the banking variables

**Savings**: There were 3 types of savings boolean variables (formal savings, informal savings, etc.), so a new variable "Savings" was created by adding up the boolean values (1 or 0) for each of the savings variables

**Age Groups**: A new variable was created grouping the ages into 4 different groups (15-18, 18-40,40-60, 60+)



## Variable Selection 

There were large amounts of colinearity amongst predictor variables, such as the different math skills or banking attributes. For this reason correlation charts were created by groups of variables with high colinearity to evaluate which of the variables should be selected based on their colinearity with the outcome variable (Poverty Bool). 

For example, the below Financial Variables were analyzed and eventually only "Number of Financial Activites" was chosen for modeling, due to its high colinearity with Poverty Bool and the other variables were excluded due to their high colinearity with the variable "Number of Financial Activities". 

![Corr Financial Variables](https://user-images.githubusercontent.com/106602444/198092092-50cd2d1d-4707-4eae-8163-bd78671a9d0c.png)

Financially the following predictor variables were selected for model creation: 

**Country**: One of 7 country values

**Is Urban**: Boolean, lives in urban area or no

**Age Group**: One of 4 age groups

**Female**: Boolean, is female or not

**Married**: Boolean, is married or not

**Religion**: One of 5 religions

**Relationship to HH Head**: One of X relationships with Head of Household

**Education Level**: One of 5 levels of education 

**Literacy**: Boolean, literate or not 

**Employment Type**: One of X types of employment

**Share of HH Income Provided**: Percent of household income provided by individual

**Income Agriculture or Livestock**: Boolean, if receieved income through agriculure or livestock sector

**Income Friends Family**: Boolean, if receieved income through friends or family

**Income Government**: Boolean, if receieved income from the governement

**Income Own Business**: Boolean, if receieved income from own business

**Income Private Sector**: Boolean, if receieved income through the private sector

**Income Public Sector Family**: Boolean, if receieved income through the public sector

**Number of Times Borrowed**: Number of times borrowed money last year

**Formal Savings**: Boolean, if has formal savings or not

**Has Insurance**: Boolean, if has insurance or not

**Has Investment**: Boolean, if has investments or not

**Number of Shocks**: Number of financial shocks last year

**Phone Technology**: One of XX levels of phone technology 

**Number of Financial Activities**: Number of financial activities last year

**Math Skill**: Combined number of math skills 


## Visualizations 

**Poverty Probability Histogram**: Histogram of PPI variable split by what was categorized as under or over the poverty line. Can note that more observations were categorized as Under the Poverty Line. 
![image](https://user-images.githubusercontent.com/106602444/198095070-2bd65e2c-b8c8-4e29-b89e-fed82fc8f4f0.png)

**Correlations**: Correlation of all numerical variables, important to note which have high levels of correlation with outcome variable (Poverty Bool)
![image](https://user-images.githubusercontent.com/106602444/198095607-cc609c4a-55ec-483f-8d12-64e6388960e9.png)

**Demographic Boolean Variables**: Frequency of demographic boolean variables. Note that most individuals are literate, rural living, married, and female. 

![image](https://user-images.githubusercontent.com/106602444/198096361-b6ed82f7-c592-48f5-84b3-5ad9b16bd2ee.png)

**Poverty Probability by Demographic Variables**: Box plots of Poverty Probability (PPI) by demographic variables. There is no large difference in gender, but there are significant differences between Married and Unmarried, Urban vs Rural, and Literate vs Iliterate observations. 

![image](https://user-images.githubusercontent.com/106602444/198109207-902cb825-430f-4f81-9adc-4067c907fca1.png)


**Average Poverty Status Categorical Variables**: Average of Poverty Bool by cateogrical variables, with size of circle correlated with frequencies of value. 

There is a large quantity of observations between 18-40, though the average of all each group is significantly under the poverty line. 

Country frequencies are generally even, though there is a large difference in average poverty status between countries A, C, and D vs countries F, G, I, and J. 

For income type there is a high frequeny of Agriculture and Livestock, Friends and Family, and Own Business, while Private and Public Sector incomes correlate with lower poverty levels. 

![image](https://user-images.githubusercontent.com/106602444/198113576-e6461c8d-de67-4c91-bc14-fdd3899a655e.png)


**Literacy Rate by Education Level**: Literacy rate correlates highly with Poverty Bool, so to analyze better Literacy Rate, it was compared with Education Levels and Age Groups. A

As expected a positive relationship between education levels and literacy rate. 

While a negative relationship between age and literacy rate, most likely reflecting recent trends in increased literacy among younger generations. 

![image](https://user-images.githubusercontent.com/106602444/198110451-724b4b39-7065-42cf-8c9c-cd78965767c0.png)


**Histogram All Predictor Variables**: Histogram of all predictor variables. 

![image](https://user-images.githubusercontent.com/106602444/198115016-e85aeeca-ea64-48f7-9ffa-4a56c7cae1da.png)


## Model Creation

Four different models were constructed: K-Nearest Neighbor (KNN), Decision Tree, Random Forest, and Logistic Regression (Logit). All models were created using the Scikit-learn library in Python. 

### Scaling and Splitting Data

First all variables values were standardized using Min-Max scaling to fall between 0 and 1. This was chosen over Standardization because for most of the variables the minimum and maximum values were known and set, due to the nature that the majority were boolean or categorical variables. 

Data was separated into 3 groups: Train, Test, and Validation. With 70% of the data randomly grouped to Train, 15% randomly grouped to Test, and 15% randomly grouped to Validation. This was to allow a test group to use for optimization, and a validation after the models had been optimized. 

### K-Nearest Neighbor 

A KNN model was created and then tested to optimize the number of neighbors. An ideal number of neighbors was selected at 9, which delivered a high accuracy tested on the test-group without overfitting the train-group. 

![image](https://user-images.githubusercontent.com/106602444/198116726-2aae5fa3-0b22-44ac-99fa-313bf106e6cc.png)


### Decision Tree

A Decicion Tree model was created and various iterations were run to optimize the max depth, based on model accuracy. An ideal depth of 6 was found, where the accuracy maximized and stabilized vs the test-group. 

### Random Forest

A Random Forest model was created, and using Grid Search the following parameters of the model were optimized: Number of Estimators, Bootstrap (True or False), and Max Features. After running the optimization, the following ideal parameters were selected for the final model: Number of Estimators = 5000, Boostrap = True, and Max Features = Squareroot. 

### Logit

A Logit model was created, and using Grid Search the following parameters of the model were optimized: C value, Class Weight (Balanced or Unbalanced), Fit Intercept (True or False), Penalty, Solver, Tol value, and Warm Start (True or False). After running the optimization, the following ideal parameters were selected for the final model: C= 1e-05, Class Weight= unbalanced, Fit Intercept= True, Penalty = None, Solver= lbfgs, Tol = 1, Warm Start= True. 


## Results 

All four models were tested vs the validation subset after all optimization was performed, and all models produced similar results when when comparing accuracy, precision, recall, and ROC AUC score. Though the Random Forest model in the end performed the best in all four metrics, and was chosen as the final model. 

**Table of Results**

![image](https://user-images.githubusercontent.com/106602444/198119775-63dec53c-6c44-4479-a98f-7d56f27d24d2.png)

**ROC Curve: Random Forest Model** 

![image](https://user-images.githubusercontent.com/106602444/198120297-0db9c760-6b45-4443-9094-8d90c76226fa.png)

**Confusion Matrix and Metrics by Poverty Status

![image](https://user-images.githubusercontent.com/106602444/198124752-2feeb7c4-484a-4c94-adf5-513ed28b97d6.png)

![image](https://user-images.githubusercontent.com/106602444/198124789-1e538722-2c7b-4459-baff-a6c8068d41e4.png)


## Conclusions 

This exercise was valubale to organize, visualize, analyzse data and in the end use it to create a model that could accurately predict if individuals lived below or above the poverty line. The final model achieved could predict 
