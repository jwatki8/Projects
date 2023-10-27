# Capstone_Final_Report- Jessica Williams
# What are users eating? A study of the most popular food.com recipes
Food.com is looking to have more diverse cuisine types within the recipes that they offer on their site. They have started to get an overwhelming majority of recipes for a few select cuisine types because these recipes have had the highest user interactions thus far. But what are the more specific factors that are giving these recipes high interaction? Is it only the cuisine type or could it be something else? The goal for this research is to find a significant correlation between the recipe attributes and user interaction and ratings. Finding the factors that contribute to user satisfaction with recipes outside of cuisine type can help us find recipes that have these specific attributes within different types of cuisine.

## 1. Data
To examine this question, we will use a dataset that consists of food.com recipes. This dataset includes information about the amount of time, nutritional facts, steps, ingredients, as well as the recipe contributor. It also includes user ratings and interactions (comments) about the recipes. The stakeholders for this project are the content manager for the site as well as the recipe addition and retention manager and team.
- [Food Kaggle Data](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv)

## 2. Method
After examining the 5 different data sets (ingr_map, RAW_recipes, PP_recipes, Raw_interactions, PP_users), I determined that the raw recipes and raw interactions files contained the most useful data. My aim was to find recipe features that had a strong effect on the ratings of the recipes. With so much text and numeric data pertaining to the attributes of the recipes I first needed to pull more generalized features and components from the data through descriptive statistics and text feature analysis. From there I looked at how the features correlated with two different target variables for the recipes which were the mean rating of the recipe and the average polarity score of the reviews for the recipe. From here I tested a few models to see which had the best predictive power for the target variables.

## 3. Data Wrangling
[Full Data Cleaning Report](https://github.com/jwatki8/Projects/blob/main/Capstone%20project%20data%20wrangling%20(4).ipynb)

This was a fairly tidy dataset but there were a few structural issues I needed to fix.
### 1.	Merging effectively

The two main data sets I needed to combine were the raw recipes and raw interactions data frames. Because the raw interactions data frame contains multiple reviews and rating for all of the recipes I wanted to create a column that would summarize the ratings and reviews and mke it easier to combine the two data frames. I created a “rating_mean” column that averaged all of the ratings for each recipe and a “polarity_avg” column that averaged all of the polarity scores for each review of a given recipe.

### 2.	Creating text features

There was a lot of text data in the raw recipes dataframe in the ingredients, steps and tag columns. I needed to pull some of the most commonly seen text in these categories to get a base for creating attribute features. In this stage I stuck to pulling only the single ngrams from the columns at first.

### 3.	Other cleaning

Some other general cleaning I did with this data included dropping all null values, changing some of the numeric columns to floats, updating the ‘minutes’ column to a datetime column and making sure that the “recipe_id’ column is named the same across all dataframes.

From here all of the two dataframes as well as the new feature columns were merged to create our working dataset called “recipe_attributes”.

## 4. EDA

[Full EDA Report](https://github.com/jwatki8/Projects/blob/main/Capstone%20%20Exploratory%20Data%20Analysis(EDA)%20Draft.ipynb)

I started by searching for any significant correlations between the numeric attribute variables and the target variables (rating_mean and polarity_avg). Although there seemed to be a higher concentration of higher ratings when the steps, ingredients, and minutes variable for a recipe is low, there were no significant correlations at all.

![Screenshot of correlation map.](/Read%20me%20files/correlation%20map.png)

Next I attempted some dimensionality reduction on the text features with the ngram range of one. From this I noticed that the features with the highest importance belonged in the step category.  After running a correlation matrix I saw no correlations higher than 0.005.

Since I didn’t find any features that seemed to have any significant predictive power on my target variables, I tried extracting text features with longer ngram ranges from the data. The feature importance was still heavy with step features but once again there were no significant correlations between any of the text features and the target variables.

![Screenshot of a feature graph.](Read%20me%20files/Feature%20importance.png)


Finally I made some further dimensionality reduction attempts by looking at the correlation between the text features and dropping the features with correlations above 0.75.


## 5. Modeling

[Full Modeling Report](https://github.com/jwatki8/Projects/blob/main/Capstone%20-%20Modeling%20.ipynb)

Since this project aimed to find features that had predictive power for the target variable, I tested a few different regression models on the data. I tested 3 different regression models and the reults were as follows:

### First was a linear regression model:

Our $R^2$ for the train and test sets are 0.069 and 0.063.

Our mean absolute error for the train and test sets are 0.646 and 0.642.

### Next was the Random Forest Regression model:

Our $R^2$ for the train and test sets are 0.093 and 0.088.

Our mean absolute error for the train and test sets are 0.647 and 0.642.

### Lastly, the K-Neighbors Regression model:

Our $R^2$ for the test set is 0.028.

Our mean absolute error for the test set is 0.656.

Looking at our models we can see that our features don’t show much predictive power. I would also like to note that I also fit the model to the rating polarity target variable as well, but the results weren’t much different.  Out of the 3 models, I got the best results from our Random Forest Regression.


### Hyperparameters 

Because I am dealing with a very large data set, I ran a RandomizedSearchCV for the hyperparameters individually. This decreased the time for the CV search dramatically. The max_features parameter was the most important to me so I started there using values of 10, 20, 30, 40 and 50. The CV search determined the best max feature parameter was 50.

Nest I wanted to find the best number of estimators. I tested the values 100,200,300 and 400. The best n_estimators parameter was 300.

Next I tested a few values for max_depth. Of the values 2, 4,6 and 8, the best parameter value was 8. Overall the hyperparameter values for this dataset seem to give the best performance when they are larger.


## Results

After applying the model to both data with our tuned hyperparameters the results I got were as follows


Our $R^2$ for the train and test sets are 0.134 and 0.105. 

Our mean absolute error for the train and test sets are 0.632 and 0.634.

In comparison to the scores for the review polarity dataset ($R^2$ of test set= 0.089, MAE of test set=0.634), the metrics for the rating mean data set were minimally better so I decided to stick with the rating mean as the target variable. Although the results could be better, using these hyperparameters for the Random Forest Regression Model give us the best metrics we have seen thus far.

