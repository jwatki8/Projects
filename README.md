# Capstone_Final_Report
# What are users eating? A study of the most popular food.com recipes
Food.com is looking to have more diverse cuisine types within the recipes that they offer on their site. They have started to get an overwhelming majority of recipes for a few select cuisine types because these recipes have had the highest user interactions thus far. But what are the more specific factors that are giving these recipes high interaction? Is it only the cuisine type or could it be something else? The goal for this research is to find a significant correlation between the recipe attributes and user interaction and ratings. Finding the factors that contribute to user satisfaction with recipes outside of cuisine type can help us find recipes that have these specific attributes within different types of cuisine.

## 1. Data
To examine this question, we will use a dataset that consists of food.com recipes. This dataset includes information about the amount of time, nutritional facts, steps, ingredients, as well as the recipe contributor. It also includes user ratings and interactions (comments) about the recipes. The stakeholders for this project are the content manager for the site as well as the recipe addition and retention manager and team.
- [Food Kaggle Data](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv)

## 2. Method
After examining the 5 different data sets (ingr_map, RAW_recipes, PP_recipes, Raw_interactions, PP_users), I determined that the raw recipes and raw interactions files contained the most useful data. My aim was to find recipe features that had a strong effect on the ratings of the recipes. With so much text and numeric data pertaining to the attributes of the recipes I first needed to pull more generalized features and components from the data through descriptive statistics and text feature analysis. From there I looked at how the features correlated with two different target variables for the recipes which were the mean rating of the recipe and the average polarity score of the reviews for the recipe. From here I tested a few models to see which had the best predictive power for the target variables.

## 3. Data Wrangling

This was a fairly tidy dataset but there were a few structural issue I needed to fix.
1.	Merging effectively
The two main data sets I needed to combine were the raw recipes and raw interactions data frames. Because the raw interactions data frame contains multiple reviews and rating for all of the recipes I wanted to create a column that would summarize the ratings and reviews and mke it easier to combine the two data frames. I created a “rating_mean” column that averaged all of the ratings for each recipe and a “polarity_avg” column that averaged all of the polarity scores for each review of a given recipe.

2.	Creating text features
There was a lot of text data in the raw recipes dataframe in the ingredients, steps and tag columns. I needed to pull some of the most commonly seen text in these categories to get a base for creating attribute features. In this stage I stuck to pulling only the single ngrams from the columns at first.

3.	Other cleaning
Some other general cleaning I did with this data included dropping all null values, changing some of the numeric columns to floats, updating the ‘minutes’ column to a datetime column and making sure that the “recipe_id’ column is named the same across all dataframes.

From here all of the two dataframes as well as the new feature columns were merged to create our working dataset called “recipe_attributes”.


