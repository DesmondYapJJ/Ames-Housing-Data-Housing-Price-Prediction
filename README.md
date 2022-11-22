<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

# Project 2 - Ames Housing Data and Kaggle Challenge

# Executive Summary 

An in-house data science team has been set up within a property agent organisation to predict the prices of future house listings. This information would be used for internal reference only and assist the realtors to determine the valuation and potential of resale of a house from a seller. The original given dataset consists of 82 columns describing the conditions and qualities of the houses. The exploratory data analysis was split between the numeric and categoric features. Our initial findings showed that there were many numeric features which had multicollinearity with one another. By utilising the correlation between the features in comparison with correlation of features and sale price, we managed to reduce the number of numeric features with multicollinearity issue. 

Subsequently, feature engineering was done to the numeric features as well. A lasso model gave the best R-squared and RMSE score. The final features selected consist of only 14 - a significant reduction from the 82 initial columns. This would streamline the potential features that our fellow colleagues, the realtors, would have to look out for in a potential resale of a house. 

## Problem Statement

The organisation is interested to streamline and extract which features from the given Ames Housing Dataset that may be useful in providing an accurate and efficient service for the realtors to predict the future sale prices of houses in Ames, Iowa. 

This would assist the realtors in the process of identifying potential houses of particular resale value and the key features to look out for. 

## Data Sets

* [`train.csv`](./datasets/train.csv): Ames Housing Dataset
* [`train_clean.csv`](./datasets/train_clean.csv): Ames Housing Dataset after cleaning
* [`train_selected.csv`](./datasets/train_selected.csv): Ames Housing Dataset features selected
* [`test.csv`](./datasets/test.csv): Ames Housing Dataset
* [`test_clean.csv`](./datasets/test_clean.csv): Ames Housing Dataset after cleaning
* [`test_selected.csv`](./datasets/test_selected.csv): Ames Housing Dataset features selected
* [`submission_lasso.csv`](./datasets/submission_lasso.csv): Final export for kaggle submission

There are two sets of given data, train.csv and test.csv. The dataset consists of 82 variables that may be useful in predicting sale price. Our objective is to streamline and select the appropriate variables to be used as features to predict the prices of the houses in Ames. 

# Part 1
## Data Import and Cleaning

There are no duplicate rows in the datasets provided. With reference to the data dictionary, it became clear that there are many null values that are not actually missing values but instead represent that there is no such feature in the house. For example, if the house has no garage then null values appear multiple times across the variables related to garage for the particular house. 

As such, a value of 0 was impute to the numeric variables while a string 'none' was impute to the categoric variables. 

# Part 2
## Exploratory Data Analysis and Statistics

There are 39 numeric variables including sale price and 42 categorical variables, totalling 81 variables given. The process of streamlining the variables was split for numeric and categorical. 

#### Numeric Variables
A heatmap correlation between only the numeric variables was plotted to identify the multicollinearity between the variables. This was compared with a second heatmap correlation between the numeric variables and sale price. 
- Findings included that overall quality has the most correlations of more than 0.5 with 7 other features, garage area, garage cars, full bath, ground living area, total basement square feet, year remod/add and year built. 
- Garage cars has a very high correlation with garage_area at 0.89.
- Ground living area has a very high correlation with total rooms above ground at 0.81.
- Total basement square feet has a very high correlation with 1st floor square feet at 0.81.

It is likely that some of these highly correlated features can be dropped as they may predict the same response. 

The final numeric variables selected included 'overall_qual', 'gr_liv_area', 'garage_area', '1st_flr_sf', 'year_built', 'full_bath', 'mas_vnr_area' and 'fireplaces' as these have high correlation with sale price and if there were multicollinearity between the variables, the one with a higher correlation with sale price was selected. 

#### Categorical Variables
There are 23 categorical variables with common value occuring more than 80% in the data. These were dropped before boxplots were plotted for the remaining categorical variables vs sale price. 

The final categorical variables selected included 'ms_zoning', 'neighborhood', 'exter_qual', 'kitchen_qual', 'fireplace_qu' and 'bsmt_qual' as these variables had a wider range over sale price as seen in the respective box plots. 

#### Sale price
Average saleprice observed at 181469. The 5 highest saleprice 611657, 591587, 584500, 582933, 556581 while the 5 lowest saleprice 35311, 35000, 34900, 13100, 12789. The distribution was right skewed, suggesting that some houses were much more expensive that the rest. 

#### Final Selection
The final selection consisted of 14 features, of which 8 are numeric variables and 6 are categorical variables. 

# Part 3
## Modelling
#### Feature engineering, Model Evaluation and Selection

pd.get_dummies was applied to the selected categorical variables identified from the EDA. The categorical variables with more than 80% common value occurrence were dropped beforehand. 

The other feature engineering that was performed was done after the initial run on a linear regression model; a polynomial feature was done on the selected numeric variables. This significantly improved the R-square test score and root mean square error in the rerun for linear regression model. 

The same set of training data were then used to train a ridge and lasso model with hyperparameter to find the best alpha. 

A summary of the findings were as follows: 

|Model|Cross Val Score|R-square score|RMSE|Best alpha|
|-----|---------------|--------------|----|----------|
|Linear Regression|0.8544|0.8970|24739|nil|
|Ridge|0.8628|0.8975|24674|1.00|
|Lasso|0.8644|0.8985|24554|32.37|

The best model is lasso with R-square score of 0.8985 and RMSE of 24554. This model was then used to predict the provided test dataset for Kaggle submission. This scored a private score of 25195 on Kaggle website. 

#### Other considerations

A check on the coefficient of the lasso model with best alpha in a separate estimator showed that there were variables with coefficient reduced to zero. It could be possible that the features selected be further streamlined in the future. 

#### Conclusion and Recommendations

A suitable model was built by the data science team to streamline the process of housing sale price prediction and reduced the number of features that the realtors have to keep an eye out for (from 82 to 14). This would be effective in providing a first cut estimation in predicting the house sale price. It would also be advisable to obtain the latest data available to be used for house sale price prediction. 

There are also external factors which has not in considered in this given data set and should be considered, such as, 
- interest rates affecting housing prices
- government policies (eg, Singapore government increasing additional buyer stamp duty on second properties onwards ([*source*](https://www.propertyguru.com.sg/property-guides/additional-buyers-stamp-duty-guide-13034))