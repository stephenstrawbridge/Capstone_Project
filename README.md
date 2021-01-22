# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Capstone Project: Film Linguistics - Predicting Film Rating
Created by:  Stephen Strawbridge, Cohort #1019

### Overview of Project
Film producers know that there is a vast variety of variables that are considered when film reviewers give their review score, such as character development, plot, director style, quality of actor/actress performance, and many more. But has the *linguistic features* of a movie script ever been considered? Could usage of certain words in the script hold predictive value on how well the film will be received by it's viewer? Film producers, more often than not, do not place heavy attention on word usage and linguistic features in a script. This project seeks to place attention on this concept and answer the above questions.   

### Problem Statement
I hypothesize that movie production companies are *not* as accurate as they could be in anticipating their reviews from viewers, due to the lack of consideration in linguistic features used in script.  This project aims to create the most ideal prediction model(s), with an emphasis on script linguistics, so that production companies can anticipate their review success during the production phase and adjust budgets accordingly.

### Contents of Project
##### Notebooks
* <u>Notebook 1</u> - Contains the data cleaning of the raw dataset pulled in, as well as feature engineering and modification for which features were ultimately used in the succeeding models.
* <u>Notebook 2</u> - Contains the exploratory data analysis of the cleaned dataset from Notebook 1.  This analysis was used to drive decision making in succeeding notebooks.
* <u>Notebook 3a</u> - Contains the first of three models used: the linear regression model with polynomial features and principal component analysis applied.
* <u>Notebook 3b</u> - Contains the second of three models used: the Random Forest regression model.
* <u>Notebook 3c</u> - Contains the last of three models used: the support vector regression model.
* <u>Notebook 3d</u> - Contains the best model (support vector regressor) with it's respective best parameters, as well as analysis of predictions and residuals from this model.

##### Other contents
* <u>Best_Params</u> - Contains the saved best parameters for the Random Forest and Support Vector regression models, which were crucial for tuning the models.
* <u>CSVs</u> - Contains the raw/original dataset pulled from online source. Also contains the cleaned CSV dataframe that was produced by Notebook 1.  This cleaned CSV was imported at the start of all modeling notebooks.
* <u>Excels</u> - Contains 'cleaned' and 'original' excel dataframes solely used for interpretability purposes, rather than as imports into any coding notebook. 'Predictions' and 'Tableau' excel dataframes were imported into Tableau for visualization and presentation purposes.
* <u>images</u> - Contains images used in either the coding notebooks for the following README.
* <u>Tableau Public Link</u> - GIVE LINK



### Background on data source
This dataset was found on the dataworld website ([*link*](https://data.world/robertjoellewis/film-subtitles)) and was published in 2016 by Robert Joel Lewis and his fellow colleagues.  First, subtitles were scrapped from the open subtitles website ([*link*](https://www.opensubtitles.org/en/search/subs)).  Next, to capture the word count of all of the features,  two different dictionaries were used: the 2015 linguistic inquiry word count dictionary (abbreviated as LIWC) ([*link*](https://dashboard.receptiviti.com/docs/liwc)) and the moral foundations dictionary ([*link*](https://moralfoundations.org/wp-content/uploads/files/downloads/moral%20foundations%20dictionary.dic)).  All other columns and features were pulled from the IMdb website.  The final consolidated dataset was published (as listed in the first link of this paragraph).

### Key Features and Data Dictionaries
The data dictionaries are provided in the links presented above.

### <u> Overall Process </u>
#### Notebook 1
*Data Cleaning* - One of the most crucial components of data cleaning in this project was determining which columns to drop, as the raw dataset has over 220 columns.  Fortunately, the columns with the vast majority of null values were the irrelevant columns that were not related to script linguistics (for example, 'plot summary' and 'made for').  Relevant columns only had a very small percentage of null values, which allowed dropping these rows without losing integrity of the dataset. Other noteworthy steps in data cleaning to preserve integrity involved dropping movies with less than 1,000 rating votes, dropping movies for which word count is below 1,000 words, and dropping rows for movies older than 30 years (rationale: vernacular could be significantly different for movies over 30 years old).  Despite the dropping of many columns and rows, the final cleaned dataset used for modeling still had nearly 10,000 rows and over 100 columns.



#### Notebook 2
*Exploratory Data Analysis (EDA)* - The EDA proved to be most valuable in determining which, and how many, linguistic ratios to drop in the data cleaning in Notebook 1.  Linguistic ratios were also dropped after multiple trial and error in the modeling notebooks, so therefore the EDA was an ongoing process throughout the project.  Key findings in the EDA were as follows:
* Some ratios had very little correlation or seemed to intuitvely have no predictive value on moving ranking (ex: 'Pronoun-ratio').  These ratios were dropped.
* A few ratios (such as 'Function-ratio') dominated in having the highest average mean.  In order to prevent losing predictive value from all other ratios, these few dominating ratios were dropped, which proved to improve the models.
* The top highest ratios (on average) was surprisingly very similar across all types of genres.  
* Overall, correlations of ratio between target variable were all *very* low which affected the approach going into the models.  With many low influencing features, principal component analysis, penalty terms, and other concepts would need to be considered.
* Regarding EDA on target variable (rating rank): distribution of rating rank is very steep and left skewed.  The mean is about 6.3.  The vast majority of the movies had movie average ratings between 5.5 and 7.5, despite the 1 to 10 scale.  This indicated that beating the baseline model (e.g. predicting mean rating of 6.3 every time) would be very challenging!

#### Notebook 3a
*Linear Regression*: This notebook contains the first of the 3 models:  Linear Regression with principal component analysis (PCA) and polynomial features.  After creating our features and target variable, polynomial features with a degree of 2 was applied to features dataframe.  This transformer caused improvement in the model score, as the many ratios with small and almost indiscernible amounts were combined in a vast variety of ways.  To compliment the polynomial transformer, PCA was applied.  It should be noted that there were no single principal components that held most of the explained variance, but nonetheless, using the top 200 principal components proved to achieve the best model scores. Training and testing R<sup>2</sup> were 33.72% and 29.83% respectively.  Testing RMSE was 0.948.  

#### Notebook 3b
*Random Forest Model*: This notebook contains the second of the 3 models:  Random Forest regression model. Note that PCA was applied to Random Forest but only appeared to slightly worsen the model.  Through a gridsearch with multiple trials and errors, the ultimate best parameters were 10,000 n_estimators, 20 max features, max depth of 100, and ccp_alpha of 0.  Increasing these parameters, with the exception of ccp_alpha, showed to improve the model score.  Training and testing R<sup>2</sup> were 90.80% and 31.09% respectively.  Testing RMSE was 0.9398. As evidenced in the disparity between the training and testing scores, this model was vastly overfit.  

#### Notebook 3c and 3d
*Support Vector Regression (SVR)*: This notebook contains the third of the 3 models:  the support vector regression model. Note that PCA was applied to Random Forest but only appeared to slightly worsen the model.  Through a gridsearch with multiple trials and errors, the ultimate best parameters were C of 0.92105, kernel of 'rbf', and degree of 0.5.  One of the reasons this model was chosen was because of it's ability to add a penalty parameter (C) which is crucial with many low influencing features.  Training and testing R-squared were 53.68% and 30.91% respectively.  Testing RMSE was 0.94103. It should be noted that although the R<sup>2</sup> and RMSE scores were the best, the model was still significantly overfit as evidenced in the disparity between the training and testing R<sup>2</sup>.


### <u> Summary of Analysis </u>
Overall, the support vector regression was deemed to be the best model in this project, as it was close to the Random Forest scores without being vastly overfit.  The relevant scoring metrics of the three models are presented below:

Note the Baseline RMSE is **1.1321**

| Model | Testing R<sup>2</sup> | RMSE | % below Baseline RMSE |
| --- | --- | --- | --- |
| Linear Regression | 29.83% | 0.9478 | 16.23% below baseline |
| Random Forest Regression | 31.09% | 0.9398 | 16.99% below baseline |
| SVR | 30.91% | 0.9410 | 16.88% below baseline |



### <u> Conclusions, Recommendations, and Limitations </u>
All three regressions outperformed the baseline model by a relatively small percentage (~16%). R<sup>2</sup> scores were unfortunately lower than expected, at around 30%.   One might initially think that the models are not very compelling and/or useful.  However, it should be noted that the baseline model was *very difficult* to beat, as the vast majority of the movie ranks were distributed closely around the mean.  If the models can beat this difficult baseline, it should be fair to say that there exists a predictive relationship between the linguistic features of a script and how well the viewers rank the movie.  During the production phase of a movie in which perhaps only the preliminary script has been assembled, producers can obtain *extra assurance* on how well their movie might be received using these data science models.

*Key Takeaways, Recommendations, and Limitations*:  As alluded to in the previous paragraph, model metric scores were still worse than anticipated.   I believe the main reasons for the lower than anticipated scores is the following:
* The context of the words is not taken into account.  Viewers may interpret and feel words differently depending on the delivery and context of the words.  
* Finding which, and how many, ratios to use as features is extremely challenging, as nearly every ratio had low correlation and low variability between each other.  Even with hours of trial and error, the most ideal combinations of the 224 original ratios was not found.   Perhaps with more time or a greater workforce on this project could more ideal combinations be found.
* The genre feature could skew results.  Genre was included as a feature in the modeling process, but better models may have been achieved if models were ran separately for each genre. This intuitively makes sense, as viewers may want to hear certain words more often in a romance film as opposed to a horror film.
