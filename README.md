**WAM! News Election Prognostication Proposal**

Wassie, Mekdes
Aziz, Zaid
Morgen, Nadia

**Problem Statement (Goal)**
Can we create a model that more accurately predicts the outcome of the 2021 California gubernatorial recall election?

**Executive Summary**
Californians recently voted decisively not to recall Governor Gavin Newsom.  Yet many pollsters and pundits predicted the election would be a squeaker.  An August SurveyUSA poll sponsored by the San Diego Tribune, KABC-TV Los Angeles, and KGTV-TV San Diego found that 51% of likely voters would vote to recall Gov. Newsom, while 40% would vote no.  This ignited a rigorous fundraising and PR campaign for Newsom.  Why did the results differ so much from the prognostications, especially earlier prognostications?  Is modern polling plagued by the lack of land lines that made random sampling easier?  Did pollsters use the wrong metrics?  Or did pollsters miss some key metrics?

At WAM! News, we are trying to develop a more accurate model than our competitors. Using the CCES Dataset from Harvard University, we will build a classification model that accurately classifies and predicts CA’s gubernatorial preferences. The model will be evaluated based on accuracy, specificity, and recall, with the goal of trying to get achieving higher accuracy than the null model and our competitors.

Although telephone polling has known methodological limitations, machine learning has improved our capacity to factor in more variables in different ways than ever before.  Machines can sift through data in ways that humans and calculators cannot.  Using a wide variety of methods that Gustavo Caffaro used in developing a model that predicted Trump's 2016 victory (https://towardsdatascience.com/using-a-neural-network-to-predict-voter-preferences-ccb9122a6df1), we hope to improve the predictions made in the 2021 California gubernatorial recall election.

Note that in 2021, voters voted on whether or not to recall Governor Newsom.  Therefore, our models are all classification models.  Since voters definitely chose not to recall the Governor, there was never an election to choose a new governor.

**1. Sample Information**
We used data from the Cooperative Election Study(CES), publicly available at https://cces.gov.harvard.edu/data.  The dataset includes national and some state polling data for 2006-2020.  Although our focus is the 2021 gubernatorial recall election in California, we examined demographic data for 2016-2020, as we suspected that shifting demographics might play a partial role.

Summary of CCES Sampling Methodology
The CCES employs sample matching, meaning they draw their sample randomly, with the goal of matching the target population.  In this case, the target population is California residents, aged 18 and older.  The investigators also used a pool of opt-in respondents to ensure that their sample matched the demographics of adult Californians at large.  Details on this process are available in the CCES Guide 2020 available at https://cces.gov.harvard.edu/data.

The data are also weighted to adjust for any remaining imbalances.  The dataframe used to match the data come from the American Community Study (ACS) conducted by the Census Bureau.  Demographers consider the ACS a highly reputable  and reliable data source.

Data Citation:
Schaffner, Brian, co-PI, Tufts University
Ansolabehere, Stephen, co-PI, Harvard University
Luks, Sam, co-PI, YouGov

Cooperative Election Study Common Content, 2020
2021-03-26
2021
V3
Harvard Dataverse
https://doi.org/10.7910/DVN/E9N6PH
doi/10.7910/DVN/E9N6PH

**2. CCES Data Summaries**
Since the CES is a national survey, we used a subset of it, specifically, California residents between 2016-2020.

Our annual n's are as follows:
2016 | 2017  |2018  |2019  |2020  |Total
-----| ------|------|------|------|------
6,021| 1,816 |5,284 |1,650 |5,028 |19,799

The following table interests us most (CCES Guide 2020.pdf, p 18):

Table 3: Survey Accuracy in 2020 CES Sample for Statewide Offices
  Office       |Average Error(Dem bias)| Root MSE| Av Freq |Exp Std Error
-------------- |----------------------|----------|--------|--------------
 President     |  .84                 | 1.96     | 675    | 2.63
 U.S. Senate   |  .85                 | 2.73     | 575    | 2.66
 Governor      |  -0.43               | 1.92     | 431    | 2.91
 State Atty Gen|  1.25                | 1.92     | 799    | 2.16
 Sec of State  |  1.65                | 1.65     | 538    | 2.73


The gubernatorial race stands out as having a negative bias.  This means the model was biased against democrats because of the factors in the model.

All data shown have 95% confidence intervals.  All respondents were validated registered voters in California.

**3. Data Dictionary**
We used the following variables:

Variable Name | Data Type   | Description                             
--------------| ----------- | -----------------------------------------
 age          | float       | Respondent's age in years
 approval_gov | 4 pt Likert | Do you approve of the way Governor Newsom is doing his job?
 dist         | int         | Respondent's US Congressional District
 educ         | categorical | Highest level of education completed
 faminc       | categorical | Family's annual income
 gender       | categorical | Are you male or female?
 ideo5        | categorical | In general, how would you describe your own political viewpoint?
 martstat     | categorical | What is your marital status?
 newsint      | categorical | Would you say you follow what's going on in government and public affairs?
 ownhome      | binary      | Do you own your own home?
 race         | categorical | What racial or ethnic group describes you?
 voted_gov    | categorical | For whom did you vote for Governor?
 vv_regstatus | categorical | Validated registration status
 vv_party_gen | categorical | Political party registration

The full data dictionary and guide to the dataset is available at https://dataverse.harvard.edu/file.xhtml?fileId=4498854&version=6.0.

**4. Required Software**
We used the following software and python libraries for our analysis:
| Author     | Library        | Module
| ------------| ---------------| -------------------------
| matplotlib  | pyplot         |
| numpy       |                |
| pandas      |                |
| random      | random.seed    |
| seaborn     |                |
| sklearn     | ensemble       | RandomForestClassifier
| sklearn     | ensemble       | VotingClassifier
| sklearn     | linear_model   | LogisticRegression
| sklearn     | metrics        |
| sklearn     | metrics        | classification_report
| sklearn     | metrics        | confusion_matrix
| sklearn     | metrics        | plot_confusion_matrix
| tensorflow  | callbacks      | EarlyStopping
| tensorflow  | keras.layers   | Dense
| tensorflow  | keras.layers   | Dropout
| tensorflow  | keras.metrics  | Recall
| tensorflow  | keras.models   | Sequential
| tensorflow  | keras.utils    | to_categorical
| tensorflow  | keras.wrappers.scikit_learn | KerasClassifier

We dropped the 14 respondents who voted for third party candidates because they comprised a very small percentage of respondents.  Although it would behoove candidates to court third party voters in a tight race, we would need evidence that they would be open to voting for a democrat or republican, as well as some indication of which party they would choose.  We lack that information in the current dataset.  We also dropped those who responded "Not sure" (n = 25) or "I didn't vote in this race" (n = 37).

**5. Exploratory Data Analysis**
We found several interesting and possibly explanatory changes in demographics between 2016 and 2020:
* Median age decreased from 47 to 45
* The percentage of married respondents dropped from 49.7% to 42.0%.
* The percentage of single / never married respondents increased from 30.5% to 35.7%
* The percentage of widowed respondents increased by 1.5%, possibly due to COVID.
* White respondents comprised 55.7% in 2016, but only 52.9% in 2020.  Since the sample is carefully designed to mirror California's general adult population, this is noteworthy.
* No other ethnic groups showed clear gains or losses, though Hispanics increased 1.1%.  Since Hispanics are likely under-represented in the sample, per the study PI's, this is worth mentioning.

The final demographic that changed was political ideology.  Respondents were asked to classify their political ideology as very liberal, liberal, moderate, conservative, very conservative, or unsure.  Between 2016 & 2020:
* The proportion of moderates decreased slightly
* The proportion of liberals remained constant
* The proportion of very liberals increased approximately 5%
* The proportion of conservatives dropped 7%
* The proportion of very conservatives edged up 1%
* The proportion of unsures increased 1.5%

Since moderates and unsures are the most difficult voters to predict, any changes in their numbers affect prognostications.

**6. Models**
Caffaro employed a neural network to calculate his predictions.  We employed the same neural net that he did, as well as several other models.  Specifically:
1. Logistic Regression with GridSearch
2. KNN with GridSearch
3. Multinomial Naive Bayes with GridSearch
4. All of the above with VotingClassifier
5. Random Forest Classifier
6. Neural Network

**Why We Chose These Models**
We began with logistic regression, KNN, and multinomial naive Bayes, with GridSearch.  Then we employed  VotingClassifier to utilize ensemble learning.  Logistic regression is simple and easily interpretable, so it makes a good starting point.  Next we tried a random forest classifier, and finally a neural network.


**Best Models**
1. Voting Classifier with Logistic Regression, multinomial naive Bayes, and KNN Classfier
2. Random Forest with Bootstrap
3. Neural Network with Early Stopping

| Metrics    | V. Classfier| Random Forest| Neural Network
| :-----------| -----------|--------------|---------------
| Accuracy    |      93%   |    93%       |    93.6%
| Sensitivity |      96%   |    96%       |      94%
| Specificity |      88%   |    89%       |      90

Based on the above metrics, we decided that the best model was the bootstrapped Random Forest model.
This model had a max depth of 6, max features of 0.5, and 100 estimators.

We chose this model as the best model because it had similar metrics as the neural network, but it was the least overfit and the easiest to interpret.

**Top 10 Features (Random Forest Model):**
1. Strongly disapprove of the governor
2. Republican (pid3)
3. Liberal (ideo5)
4. Strongly approve of the governor
5. Age
6. US Congressional District
7. Very Conservative (ideo5)
8. Independent (pid3)
9. Disapprove or Somewhat disapproves of the Governor


**Model Validation on 2021 Recall Election**
Finally, we tested our model on unseen data from 2020 as a way of predicting the 2021 gubernatorial recall election.  2021 data were unavailable.  Our model predicted that 68.7% of voters would vote no on the recall, and 31.3% of voters would vote to recall the Governor.  As of now, 62% of voters voted against the recall and 38% voted for it.  Since our model was trained on 2018 data and tested on 2020 data, we are pleased with our results.

**Limitations**
California is a liberal state, so predicting the outcome of a statewide election might seem easy.  We chose California because it had sufficient data.  We can't forecast election results, like most pollsters do, because we lack the right data.  We would need very current data to do this.  We were also limited to data available at no cost.

**Conclusions & Recommendations**

**1. Moderates are very difficult to predict**
One likely reason why pollsters are struggling to predict election outcomes is the increasing number of voters who describe themselves as moderate, independent, or otherwise non-partisan.  The only way to gauge how these people will vote is to ask them issue-related questions, such as, "Where do you stand on...?"

**2. Key demographics are shifting**
California is getting younger, less white, more single (never married), and more liberal.  While we have seen only small changes between 2016 and 2020, small changes make a large difference when generalizing a sample to a population.

**3. Weighting smaller samples is not the solution to under-representation.**
If accuracy is the goal, there are few solutions to obtaining more data.  The survey authors' method of using volunteers was likely the best option.  That said, using volunteers might have increased the newsint (how much respondents follow the news and current events) artificially.  This variable was skewed toward following current events more than I would expect.  The other possibility is that respondents followed a social desirability bias.

**4. Using more sophisticated machine learning models, such as random forest, can improve accuracy**
We have far more tools and computer power at our fingertips than we did even 10 years ago.  Tools are always involving.  We cannot assume that what worked 10 years ago remains the best method today.

**5. Mail-In Voting**
The recall election was an entirely mail-in election.  Higher voter turnout favors democrats.  It's possible that pollsters did not know how to quantify this, particularly in a recall election in a non-presidential election year.

**6. Clustering, PCA, and Time Series**
We believe we can improve accuracy using less interpretable models, such as clustering techniques and PCA.  We also want to add time series analysis, due to the nature of the data.
