# Recipes-Research-Project

This is the Project 3 under course DSC80

By Jiangqi Wu & Yuxuan Zhang

## Introduction

The world of cooking and food has grown exponentially over the years, with a vast array of recipes and cuisines available at our fingertips. In this project, we aim to dive into an extensive dataset consisting of recipes and their respective ratings. Our goal is to uncover trends and patterns that can provide valuable insights into the factors contributing to the popularity and success of a recipe.

The dataset is sourced from food.com and contains recipes and reviews posted since 2008. The data is divided into two parts: recipes and ratings. The recipes dataset includes information such as recipe name, ID, preparation time, contributor ID, submission date, tags, nutrition information, number of steps, steps text, and description. The ratings dataset, on the other hand, contains user ID, recipe ID, date of interaction, rating, and review text.

The dataframe for recipes have 83782 rows, meaning 83782 unique recipes. The dataframe for ratings have 731927 rows, each stand for a review on the recipe. For furthur research purposes, we add column for average rating for each recipes in the dataframe, so we could analyze the features of recipes and corresponding ratings.

In the project, we will be first cleaning the data set and conduct exploratory data analysis, to obtain some basic information of the data set and relation between columns. Then, we will assess the missingness contained in the data set by NMAR analysis and analyzing the missingness dependency. 

In the missingness analysis, we will be focusing on ...

Moreover, we would focus on the research question that, are complex recipes and simple recipes rated in the same scale. We would define recipe with fewer than 10 steps as simple recipes, and with more than 10 steps as complex recipes. We would analyze the rating scale related to the complexity of the recipe.

This research question could be important for recipe-designers and food.com website holder. By answering this research question, we could possibly provide the viewpoints to the rating scale for people who use this website. With our result stating whether people would prefer complex recipes or not, recipe-designers could design more complex/simple recipes to meet the need to people using the website.

## Data Cleaning

Before conducting analysis related to the datasets, we would first conduct data cleaning to make the data more convenient for analysis.

> Checking Data Types
 
First, I check the data type for each column and think about the necessary data cleaning steps.

```
name              object
id                 int64
minutes            int64
contributor_id     int64
submitted         object
tags              object
nutrition         object
n_steps            int64
steps             object
description       object
ingredients       object
n_ingredients      int64
dtype: object
```
> Converting Object to List

The first step we are going to do to the dataframe is the tags, steps and ingredients columns. The three column all look like lists of string, but by checking the specific entry in the dataframe, we find that they are actually not lists. This could due to when web scraping, data collecter does not convert the text into list. As a result, we take action to convert the these three columns into list of string.

> Converting `nutrition` Column to List and Assign Individual Columns

We also find that the `nutrition` column in the dataframe look like a list containing float but actually not. We find that in the list, the float represent: `'calories', 'total fat (PDV)', 'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)'`. As a result, we first convert the column into list of float and create individual column for each nutrition. In each column, we also convert the data to float data type that is convenient for later calculation.

> Changing submitted column into datetime

The `submitted` column in the dataframe is presented as a object. We change it into datetime data type so we could apply basic operations later.

> Merging Two Dataframes
 
Then, since there are two dataframe but with common column, which are `id` and `recipe_id`. As a result, we merge the two dataframe together to show the recipes and corresponding rating and reviews.

merged dataframe head needed

> Adding Average Rating Column

After merging the two dataframes, we find one important data is the rating for the recipes. As a result, we add new column name `ave_rating`, which include the average rating for the column. Also, we beleve that the 0 in the rating might be empty rating that people do not fill in. As a result, we replace 0 with nan value

> Cleaning Result

The cleaned dataframe is shown below.

df head needed.

## Exploratory Data Analysis

### Univariate Analysis

In the univariate analysis, we would analyze the distribution of number of ingredients and the distribution of number of steps

<iframe src="assets/fig1.html" width=800 height=600 frameBorder=0></iframe>

This shows that the distribution could be approximate as a gaussian distribution but skewed right. We would say that the graph centered around 8, meaning that most recipes have 8 ingredients.

<iframe src="assets/fig2.html" width=800 height=600 frameBorder=0></iframe>

The distrubution also show similar trend in the number of steps, which is a right skewed gaussian distribution. By comparing at the two graph, the graph for the number of distribution is more centered. The center for the graph is around 7, meaning most recipes have 7 steps. Also, we could see the graph have a lot outliers that have very big step numbers. After observing the dataset and also consider together with the `minutes` column and real life situation, we decided to choose steps greater than 40 and minutes greater than 200 as outlier and not faithful data.

### Bivariate Analysis

Then, we do bivariate analysis between the number of steps and the number of ingredients


<iframe src="assets/fig3.html" width=800 height=600 frameBorder=0></iframe>


When the individual distritbution for number of steps and number of ingredients seems very similar, the scatter plot does not show very strong correlation between the number of steps and number of ingredient. We could say that there is weak positive relationship between the number of steps and the number of ingredients.


<iframe src="assets/fig4.html" width=800 height=600 frameBorder=0></iframe>


We could see that the average rating and the number of ingredients in the recipes do not have much relationship with each other. Especially with number of ingredients smaller than 15, it is almost a horizontal line, showing no relationship between the two variables. The large fluctuate with number of ingredients larger than 15 could be due to relatively small data size collected within that range.

### Interesting Aggregates

In the aggregates analysis, we will study the total fat with the cooking minutes

|   minutes |   mean total fat (PDV) |   median total fat (PDV) |   min total fat (PDV) |   max total fat (PDV) |
|------------------:|------------------------------:|--------------------------------:|-----------------------------:|-----------------------------:|
|                 0 |                      46       |                              46 |                           46 |                           46 |
|                 1 |                       7.78603 |                               0 |                            0 |                          159 |
|                 2 |                       9.69053 |                               0 |                            0 |                          419 |
|                 3 |                      12.5794  |                               2 |                            0 |                          411 |
|                 4 |                      20.4719  |                               7 |                            0 |                          258 |

This is the pivot table for the `total fat` and `minutes`


<iframe src="assets/fig6.html" width=800 height=600 frameBorder=0></iframe>


<iframe src="assets/fig7.html" width=800 height=600 frameBorder=0></iframe>

One interesting result that we find in the aggregates data is that there is a peek for total fat in the recipe around 60 minutes of cooking time. Otherwise the recipes' total fat is fluctuate around 50 PDV, which is around 1000 calories. This shows that most recipes collected are recipes for health food.

## Assessment of Missingness

In this part, we will be conducting assessment of missingness on the merged dataframe.

### NMAR Analysis

### Missingness Dependency

## Hypothesis Testing

The question we are going to research on is that: are regular recipes and complex recipes are rated in the same scale?

In this part, we will define a complex recipes as recipes have greater than 10 steps. We will conduct a permutation test.

### Setting Up the Testing

Null Hypothesis H0: People are rating all the recipes in the same scale.

Alternative Hypothesis H1: People are giving complex recipe lower rating

The reason for choosing one-sided test is that we might assume people could feel frustrated when cooking complex recipes, and also recipes with more steps are harder to cook

| complex   |   n_steps |   ave_rating |
|:----------|----------:|-------------:|
| False     |   6.35718 |      4.50184 |
| True      |  16.1414  |      4.48441 |

Since ave_rating is numerical data, so it is proper to use the difference in mean as test statistics. In the part of research, the significant level we choose is `0.05`

The observed difference in mean is `0.017428379224658563`

### Permutation Test

<iframe src="assets/fig10.html" width=800 height=600 frameBorder=0></iframe>

We ran permutation test for 10000 times and the graph shows the distribution of permutation test result. The red line marks the observed value.

### Hypothesis Testing Conclusion

The P-value for the testing is 0.0013, which means that at significant level of 0.05, we are able to reject the null hypothesis.

This result could be reasonable since first, hight level complexity of a recipes could mean difficulties in cooking the dish and increasing probability in failing. If people fail to cook the dish, they might give low rating to the recipe. Also, people might have higher expectation on the dish if it is complex and hard to make. Then, people might have a more strict rating scale for complex recipes. 

