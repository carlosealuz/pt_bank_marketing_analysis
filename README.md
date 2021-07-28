# Portuguese bank institution marketing analysis
This is a personal project I did with a dataset available at [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing).

This dataset contains data regarding a marketing campaign a bank from Portugal did from May 2008 to November 2010 and the objective of this campaign was to lead the client to subscribe a term deposit. So, it's a classification problem to predict if a client will subscribe a term deposit or not.


# Table of Contents
1. [Data overview](#Data_overview)
2. [EDA](#EDA)
3. [ML Models](#ML_Models)

## Data overview <a name="Data_overview"></a>

This data consist of 16 inputs plus the target. The inputs are the following:
1. age (numeric)
2. job : type of job (categorical: "admin.","unknown","unemployed","management","housemaid","entrepreneur","student",
                                   "blue-collar","self-employed","retired","technician","services") 
3. marital : marital status (categorical: "married","divorced","single"; note: "divorced" means divorced or widowed)
4. education (categorical: "unknown","secondary","primary","tertiary")
5. default: has credit in default? (binary: "yes","no")
6. balance: average yearly balance, in euros (numeric) 
7. housing: has housing loan? (binary: "yes","no")
8. loan: has personal loan? (binary: "yes","no")
#### related with the last contact of the current campaign:
9. contact: contact communication type (categorical: "unknown","telephone","cellular") 
10. day: last contact day of the month (numeric)
11. month: last contact month of year (categorical: "jan", "feb", "mar", ..., "nov", "dec")
12. duration: last contact duration, in seconds (numeric)
#### other attributes:
13. campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)
14. pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric, -1 means client was not previously contacted)
15. previous: number of contacts performed before this campaign and for this client (numeric)
16. poutcome: outcome of the previous marketing campaign (categorical: "unknown","other","failure","success")
#### Output variable (desired target):
17 - y - has the client subscribed a term deposit? (binary: "yes","no")

So, there are some informations interesting in this features, specially the columns with "unknown" values (education, job, contact and poutcome). This could be treated as a new category or as a missing data. In my analysis some columns I use this as a category like job, and other columns I decide to replace unkown values with one category already at the dataset, like contact. Also, poutcome column I decide to treat "unknown" as missing data and end up droping the entire columns because the majority of values were missing.

## EDA <a name="EDA"></a>


## ML Models <a name="ML_Models"></a>

