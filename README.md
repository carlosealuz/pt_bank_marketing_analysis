# Portuguese bank institution marketing analysis
This is a personal project I did with a dataset available at [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing).

This dataset contains data regarding a marketing campaign a bank from Portugal did from May 2008 to November 2010 and the objective of this campaign was to lead the client to subscribe a term deposit. So, it's a classification problem to predict if a client will subscribe a term deposit or not.


# Table of Contents
1. [Data overview](#Data_overview)
2. [EDA](#EDA)
3. [ML Models](#ML_Models)
4. [Referências](#Referencias)

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
I did an EDA to undestand the dataset before train some models and to give some feedback to this bank marketing team about this campaign. Main informations I found was:
Taking a look at the dataset with the describe function we can see that
Taking a look at the describe of this dataframe we can understand some quick informations:

* Minimum age is 18 (maybe is the minumum age to open a bank account at Portugal?). Maximum age is 95. Mean age is ~41.
* Balance has a lot of dispersion. At least one client have a negative balance. Maximum balance is higher than 100.000
* Mean marketing call duration was about 4 minutes. In my opinion, 4 minuts is not a lot considering a marketing (or a sales) call. Which is good because we don't want to upset the client with long calls.
* Mean number of calls done to the same user was 2. Again, in my opinion, this is ok.
* Previous looks like there is some outliers. Maximum value here is 275 which means the client was contacted 275 times before the campaign? This value seems off to me.
* pdays I will convert in a category. -1 will be never_contacted and other values will be already_contacted

Regardless the mean call duration and mean number of calls, this data has a lot of unbalance. 88% os clients does not subscribed a term deposit. It would be a good point to review the message of this call. 

* The majority of clients in the bank work on a blue-collar and management positions, married with secondary degree of education and without personal loan but with housing loan.
* Also, more people at management, technician and admin did deposits. And its important to notice that, as I mention above the majority of people at the dataset does have a housing loan but most people without this loan did a deposit.

Since 88% of contacted people do not did a deposit at the bank, I think the marketing team should try to increase call duration, maybe give some coupon codes with a third partner or also give discount to loan (house or personal) interest. As we saw on the EDA, a lot of clients have a housing loan, but they do not have a deposit done during this campaign.

![Alt text](images/deposits_by_job.jpg?raw=true)
![Alt text](images/deposits_by_education.jpg?raw=true)
![Alt text](images/deposits_by_loan.jpg?raw=true)
![Alt text](images/deposits_by_marital.jpg?raw=true)
![Alt text](images/contacts_by_day.jpg?raw=true)


## ML Models <a name="ML_Models"></a>
With a good classification model, the marketing team can improve how they spend money with customer acquisition. Instead of call to everyone, they could target their audience and reduce CAC.

As a baseline model, I created a Random Forest with default hyperparameters.

This model had an accuracy of 88% and AUC of 0.523 also there is a lot of False positives which is bad here.

For this first model I just did some feature engineering and drop some columns and encoded categorical variables with Sklearn's OneHotEncoder. I kept class imbalance.

Next thing I did was tuning hyperparameters with skopt and gp_minimize. The idea with gp_minimize is to use a gaussian process to minimize the loss function. I used roc_auc_score as a metric to the model, then I created a function receiving a set of hyperparameters, training the model and calculate roc_auc_score. Since gp_minimize return the minimum metric value as a good hyperparameters, the function returns the negative of roc_auc_score.

The model with hyperparameters tuned does not improved. False positives increased instead.

Then I used SMOTE to balance the data. SMOTE works by generating new samples for the minority class. The way it works is by first randomly select a sample of the minority class, then it search for the K neighbours (default is 5) and a random neighbour is selected and a new example is generated by selecting a random space between this two samples. More detailed information can be found [here](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/)

The same Random Forest with hyperparameters tuned was trained with this 'new' dataset with balanced classes and it does improved. False positives decreased.

The last model I trained was a LightGBM with hyperparameters tuned (same strategy as before). This was the best model so far with lowest false positives so far, precision and recall improved compared with other models and AUC also improved.

So, this could be a model to put at production maybe in an app in heroku or any cloud this bank want to subscribe. This app could receive a csv file prepared by the marketing team with clients features and it would return a new csv with clients tagged as potential subscribers. With this at hand, they can target their audience and decrease CAC, calling only users with a subscription potential.

## Referências <a name="Referencias"></a>

* https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/
* https://www.youtube.com/watch?v=WhnkeasZNHI&ab_channel=MarioFilho
* https://archive.ics.uci.edu/ml/datasets/Bank+Marketing#



