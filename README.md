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

Instead of prediciting the Poverty Probaility Index, in order to tackle a different problem the PPI was converted to a binary outcome: 0 ("Not Under Poverty Line"), or 1 ("Under Poverty Line"). This was done by assinging 0 to any observations under 0.50, and assigning 1 to any observations over 0.50. There were no observations exactly at 0.50. 


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





