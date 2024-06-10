# Ingredients to Insights: A Statistical Analysis of Recipes

Welcome to **Ingredients to Insights:** 🍝 This is a detailed data science project undertaken at UCSD. It focuses on exploring the relationship between the complexity of a recipe to its average rating. We analyze a dataset containing information about various recipes from [food.com](https://www.food.com/)

By Dhanvi Patel

## Introduction

Cooking is a hobby for a lot of people. Some people, especially avid bakers like myself, enjoy detailed recipes and the delicious dishes they result in. For others, cooking is a mode of sustenance. They prefer simple recipes with less complexity. Through this project, I wanted to analyze the relationship between the number of steps in a recipe and its average rating. Do recipes with more steps receive higher ratings due to readers feeling biased by the effort they've invested in making them? Or do they have lower ratings resulting from disappointment from its complexity?

The primary relationship we are focusing on is:  

**What is the relationship between the number of steps and the average rating of recipes?**

Readers of this website should care about the dataset and this question specifically because it offers insights into cooking preferences and the potential impact of recipe complexity on satisfaction. Whether you're a food enthusiast, a casual cook, or someone interested in data analysis, understanding this relationship can guide you in choosing recipes that align with your cooking style and expectations.

### Dataset 

We are analyzing two datasets consisting of recipes and ratings posted on [food.com](https://www.food.com/). The first entry is from 2008 and the newest one is from 2018. 

The first dataset, `recipe`, contains 83782 rows, with 83782 unique recipes, and 10 columns with following information:

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe   

The second dataset, `interactions`, contains 731927 rows and each row contains a review from the user on a specific recipe. The columns it includes are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |


**Fun fact:** Cooking is a proven stress-relieving practice! 

## Data Cleaning and Exploratory Data Analysis

To optimize our dataset analysis, we performed the following data cleaning steps:

1. Left merge the recipes and interactions datasets together on `id` and `recipe_id` respectively 

	- This step matches unique recipes to ratings and reviews 


2. Fill all ratings of 0 with np.nan

	- Ratings range from 1 to 5; 1 being worst and 5 being the best. Thus, a value of 0 in the ratings column is a clear indication of missing values. To avoid misinterpretation of statistical facts, we place 0 with np.nan


3. Add `average_rating` to the dataframe 

	- Different people can rate a recipe differently. To get a better sense of a recipe's true popularity, we average all ratings and add it to a new column named `average_rating`


4. Add `'is_complex` to the dataframe

	- The column `is_complex` holds boolean value True if the recipe has more than 10 steps, False otherwise. 


5. Add `year` and `month` of submission of recipe

	- Use Timestamp to convert `submitted` column into `year` and `month` to observe any trends


#### Resulting dataframe 

| Column                  | Description    |
| :---------------------- | :------------- |
| `'name'`                | object         |
| `'id'`                  | int64          |
| `'minutes'`             | int64          |
| `'contributor_id'`      | int64          |
| `'submitted'`           | datetime64[ns] |
| `'tags'`                | object         |
| `'nutrition'`           | object         |
| `'n_steps'`             | int64          |
| `'steps'`               | object         |
| `'description'`         | object         |
| `'ingredients'`         | object         |
| `'n_ingredients'`       | int64          |
| `'user_id'`             | float64        |
| `'recipe_id'`           | float64        |
| `'date'`                | datetime64[ns] |
| `'average rating'`      | float64        |
| `'is_complex`           | bool.          |
| `'year`                 | bool.          |
| `'month`                | bool.          |

The resulting dataframe has 234429 rows and 18 columns. Let's look at the first couple rows of recipes of our **cleaned** dataframe for better understanding. 

Note: Scroll right for more columns 

| name                                 |     id |   minutes | submitted           |average rating |   is_complex   |   n_steps     | n_ingredients |
|:-------------------------------------|-------:|----------:|:--------------------|--------------:|---------------:|--------------:|:-------------:|
| 1 brownies in the world    best ever | 333281 |        40 | 2008-10-27 00:00:00 |             4 |          False |            10 |    9          |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 2011-04-11 00:00:00 |             5 |          True  |            12 |    11         |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |             5 |          False |             6 | 	9          |  
| millionaire pound cake               | 286009 |       120 | 2008-02-12 00:00:00 |             5 |          True  |             7 |    7          |   



### Univariate Analysis 

**Distribution of Number of Steps of Recipes**

This plot displays the distribution of the number of steps required in recipes. 

<iframe
  src="assets/univariate_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the plot, we can see that most recipes have a moderate number of steps, with a peak around 6-10 steps. This indicates that the majority of recipes are not overly complex in terms of the number of steps required.


**Distribution of Average Rating**

This plot shows the distribution of average ratings for recipes.

<iframe
  src="assets/univariate_2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution is skewed towards higher ratings, indicating that most recipes receive favorable reviews from users. The peak around the higher end suggests that users are generally satisfied with the recipes they try.


### Bivariate Analysis 

**Relationship between Number of Steps and Average Rating**

This line plot visualizes the relationship between the number of steps in a recipe and its average rating.

<iframe
  src="assets/bivariate_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the graph, we notice that towards the extreme spectrum of number of steps, recipes either have very high ratings or very low ratings. We want to explore this relationship further. 

### Useful Aggregates 

This pivot table provides a statistical summary of the relationship between the number of steps in a recipe and its ratings. The table is grouped by the number of steps (n_steps) and includes three key metrics for each group:

	1. Average Rating: The mean rating of recipes that have a specific number of steps. This value helps identify whether recipes with more steps tend to receive higher or lower ratings on average. For example, if recipes with more steps consistently have higher average ratings, it might indicate that users appreciate the detailed instructions. Conversely, if they have lower ratings, it might suggest that users find complex recipes frustrating or difficult to follow.

	2. Rating Standard Deviation: The standard deviation of the ratings for recipes with a specific number of steps. This metric indicates the variability or dispersion of the ratings within each group. A higher standard deviation means there is more variation in the ratings, while a lower standard deviation indicates that the ratings are more consistent.

	3. Recipe Count: The number of recipes that have a specific number of steps. This count shows how many recipes fall into each group, providing context for the average rating and standard deviation values.


| `n_steps`  | (mean, `average_rating`)   | (std, `average_rating`)  | ('count', 'prop_sugar') 
| ------:    | -------------------------: | -----------------------: | --------------------: |
|  1         |                4.65        |                0.49      |             2657      |            
|  2         |                4.67        |                 0.48     |                  7557 |              
|  3 				 |               4.66         |                  0.48    |                 11583 |              
|  ... 	 	   |               ...          |                  ...     |                   ... |               
|  93 			 |               5.00.        |                 0.0      |                    4  |              
|  98				 |               5.00         |                      0.0 |                     1 |               
|  100		   |               0.132994     |               0.0        |                     2 |     


Overall, this pivot table allows us to analyze and visualize how the complexity of recipes, in terms of the number of steps, correlates with user ratings, offering insights into user preferences and potential areas for improving recipe instructions.

<iframe
  src="assets/pivot_table_graph_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Assessment of Missingness 

Assessing the missingness in the dataset involves understanding whether the missing values are Missing Completely at Random (MCAR), Missing at Random (MAR), or Not Missing at Random (NMAR). In our dataset, we have most missing values in the `average_rating` and `description` columns, and it's likely that these values are NMAR.

The missingness in the `average_rating` column could be because users chose not to provide ratings for certain recipes. This is likely related to the unobserved characteristics of those recipes that users choose not to rate. For example, if a recipe is particularly complex or not appealing in description, users might decide not to engage with it, resulting in no rating being provided. This behavior suggests that the missingness is related to the inherent qualities of the recipes themselves. 

To better understand and potentially explain this missingness (thereby making it MAR), we might want to obtain additional data such as:

**User Engagement Metrics:** Information on how many users viewed but did not rate the recipe.


### Interpretation of Missingness Permutation Tests

To understand the relationship between the missingness in the `average_rating` column and other columns, we conducted permutation tests. These tests help determine if the missingness in `average_rating` is dependent on other variables in our dataset, specifically `description` and `n_steps`.

#### Test 1:** Dependency of Missing `average_rating` on `description`

**Null Hypothesis:** The missingness of `average_rating` does not depend on the missingness of `description`.

**Alternate Hypothesis:** The missingness of `average_rating` does depend on the missingness of `description`.

**Test Statistic:** Chi-squared statistic 

**Significance Level:** 0.05

**Permutation Process:** 

1. Shuffle the description missingness column 1000 times to simulate the distribution of the chi-squared statistic under the null hypothesis.
2. For each permutation, recalculate the chi-squared statistic with the shuffled description column.
3. Compute the p-value as the proportion of permuted chi-squared statistics that are greater than or equal to the observed chi-squared statistic.

**Results:**

**P-value:** 0.03596

**Interpretation:** Since the p-value is less than 0.05, we reject the null hypothesis. This indicates that the missingness of `average_rating` depends on the missingness of `description`.

**Visualization:**

<iframe
  src="assets/missing_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Test 2:**  Dependency of Missing `average_rating` on `n_steps`

**Null Hypothesis:** The missingness of `average_rating` does not depend on the number of steps (`n_steps`).

**Alternate Hypothesis:** The missingness of `average_rating` does depend on the number of steps (`n_steps`).

**Test Statistic:** Chi-squared statistic 

**Permutation Process:** 

1. Shuffle the `n_steps` column 1000 times to simulate the distribution of the chi-squared statistic under the null hypothesis.
2. For each permutation, recalculate the chi-squared statistic with the shuffled `n_steps` column.
3. Compute the p-value as the proportion of permuted chi-squared statistics that are greater than or equal to the observed chi-squared statistic.

**Results:** 

**P-value:** 1.0

**Interpretation:** Since the p-value is greater than 0.05, we fail to reject the null hypothesis. This indicates that the missingness of average_rating does not depend on the number of steps.

**Visualization:**

<iframe
  src="assets/missing_2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Hypothesis Testing 

This section presents the statistical analysis conducted to address the hypothesis that there is a difference in the average ratings between recipes with a high number of steps (more than 10) and those with a low number of steps (10 or fewer). Utilizing appropriate hypothesis testing methods and a significance level of 0.05, we investigate whether the observed data provide sufficient evidence to support this hypothesis.

#### Test 1: 

**Null Hypothesis (H0):** There is no difference in the average ratings between recipes with a high number of steps (more than 10) and those with a low number of steps (less than or equal to 10).

**Alternative Hypothesis (H1):** There is a difference in the average ratings between recipes with a high number of steps (more than 10) and those with a low number of steps (less than or equal to 10).

**Test Statistic:** The mean difference in average ratings between the two groups.

**Significance Level:** 0.05

**Justification**

1. **Choosing the hypothesis:**
These hypotheses are formulated based on the research question, which aims to determine whether there is a significant difference in average ratings between recipes with different numbers of steps. By formulating clear null and alternative hypotheses, we can directly test this research question.

2. **Significance Level:**
 A significance level of 0.05 is commonly used in hypothesis testing and provides a balance between Type I and Type II errors. It allows for a reasonable level of confidence in the conclusions drawn from the analysis.

3. **Permutation Test:**
 A permutation test is appropriate for this analysis as it does not rely on distributional assumptions and allows for testing hypotheses when the assumptions of parametric tests may not hold. It also provides an empirical way to compute the p-value, making it suitable for situations where the sample size is small or the data is non-normally distributed.

**Results of Test 1:**

Mean Difference in Average Ratings (Observed): 0.001641837343935748
P-value: 0.427

**Conclusion:** Fail to reject the null hypothesis (H0). There is no sufficient evidence to conclude that there is a difference in the average ratings between recipes with more than 10 steps and those with 10 or fewer steps.

**Visualization**

<iframe
  src="assets/hyp_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


#### Test 2: 

**Null Hypothesis (H0):** There is no difference in the average ratings between recipes with a high number of ingredients (greater than the average) and those with a low number of ingredients (lower than the average)

**Alternative Hypothesis (H1):** There is a difference in the average ratings between recipes with a high number of ingredients and those with a low number of ingredients.

**Test Statistic:** Mean difference in average ratings between the two groups.

**Significance Level:** 0.05

**Justification**

1. **Choosing the hypothesis:**
The null and alternative hypotheses are formulated based on the research question, which seeks to investigate whether there exists a difference in the average ratings between recipes with varying numbers of ingredients. These hypotheses provide clear statements about the expected outcome of the analysis, allowing for direct testing of the research question.

2. **Significance Level:**
 A significance level of 0.05 is commonly used in hypothesis testing and provides a balance between Type I and Type II errors. It allows for a reasonable level of confidence in the conclusions drawn from the analysis.

3. **Permutation Test:**
Same reasons as stated above. 

**Results of Test 2:**

Mean Difference in Average Ratings (Observed): -0.008623780234939815
P-value: 0.0

**Conclusion:** Reject the null hypothesis in favour of alternative. There is a significant difference in average ratings between recipes with a high number of ingredients and those with a low number of ingredients.

**Visualization**

<iframe
  src="assets/hyp_2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Framing a prediction problem 

**Prediction Problem:**
We are tackling a regression problem aiming to predict the average rating of a recipe based on two key features: the number of steps required to prepare the recipe (n_steps) and the number of ingredients used in the recipe (n_ingredients). The response variable in this problem is the average rating, which represents the overall quality or appeal of the recipe as rated by users.

**Type:** Regression

**Response Variable:** Average Rating

**Explanation:** The choice of the average rating as the response variable is justified because it directly reflects the quality or appeal of the recipe, which is the target of our predictive model. As our goal is to estimate the perceived quality of the recipe, the average rating is the most relevant variable to predict.

**Evaluation Metric:**
We will use the Mean Absolute Error (MAE) as our evaluation metric. MAE measures the average absolute difference between the predicted ratings and the actual ratings. We chose MAE over other suitable metrics such as Mean Squared Error (MSE) because MAE is more interpretable and less sensitive to outliers. Since our prediction problem is focused on estimating the average rating accurately, MAE provides a straightforward measure of prediction error that aligns with our goal.


## Baseline Model 

For our baseline model, we will use a simple linear regression model. The features included in our model are:

**Quantitative Features:**

Number of steps (`n_steps`)
Number of ingredients (`n_ingredients`)

**Encoding:**

Since both features are quantitative, no encoding is necessary.

**Performance Evaluation:**

We will evaluate our baseline model using the Mean Absolute Error (MAE) metric on both the training and testing datasets. This will allow us to assess how well the model generalizes to unseen data.

**Implementation:** 

**1.Data Preprocessing:** We imputed rows with missing values in both the features (n_steps, n_ingredients) and the target variable (average_rating).

**2.Data Splitting:** We split the dataset into features (X) and the target variable (y), and further divided them into training and testing sets using a 80-20 split ratio.

**Pipeline:** We defined a pipeline for our baseline model, which included:

Imputation: We used SimpleImputer from scikit-learn to handle missing values by replacing them with the mean of the respective feature.
Model Training: We trained a LinearRegression model on the imputed data.
Model Evaluation: We made predictions on both the training and testing data using the trained model and calculated the MAE for both sets.

**Baseline Model Performance:**

Training MAE: 0.3439544791331869
Testing MAE: 0.3400908942638112

The baseline model achieved a training MAE of approximately 0.344 and a testing MAE of around 0.340. Considering the baseline model's performance, it can be considered a decent starting point. These results serve as a reference point for evaluating the performance of future, more complex models.

**Visualization**

<iframe
  src="assets/baseline_model.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Final Model 

Features Added:

`steps_per_ingredient:`
 This feature captures the ratio of steps to ingredients, providing insight into the complexity of the recipe relative to the number of ingredients. Recipes with a higher ratio might be perceived as more complex, potentially impacting the average rating.

`avg_rating_by_contributor:`
 This feature captures the average rating given by the contributor, offering a glimpse into the rating behavior of the contributor. Recipes contributed by individuals who consistently give high ratings might have inflated average ratings.

**Modeling Algorithm:**

For the final model, we chose the Random Forest Regressor algorithm. Random forests are robust and versatile ensemble learning methods capable of capturing complex relationships in the data while mitigating overfitting.

**Hyperparameters and Selection Method:**

We used GridSearchCV to perform hyperparameter tuning for the Random Forest Regressor. The hyperparameters considered were:

1. Number of estimators (n_estimators)
2. Maximum depth of each tree (max_depth)
3. Minimum number of samples required to split an internal node (min_samples_split)
4. Minimum number of samples required to be at a leaf node (min_samples_leaf)

We searched over a predefined hyperparameter space and selected the combination that yielded the best performance based on the negative mean absolute error (MAE) score.

**Model Performance:**

The final model's performance was evaluated using MAE on both the training and testing datasets. The MAE values were as follows:

**Training MAE: 0.1686**
**Testing MAE: 0.2212**

The final model demonstrates strong performance on both the training and testing datasets, with mean absolute error (MAE) values of approximately 0.169 and 0.221, respectively. This indicates that, on average, the model's predictions deviate from the actual average ratings by around 0.169 units on the training set and 0.221 units on the testing set.

**Comparison with Baseline Model:**

The final model's performance represents an improvement over the baseline model's performance, as evidenced by the reduction in MAE on the testing dataset. The additional features and the more sophisticated modeling algorithm likely contributed to this improvement. 

The **visualization** of prediction errors highlights the distribution and spread of errors, providing insights into the model's predictive performance across different instances.

<iframe
  src="assets/error_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Fairness Analysis 

In this section, we conducted a fairness analysis to assess whether the performance of our final predictive model varies across different time periods, specifically focusing on recipes before and after the year 2010. The goal of the analysis was to investigate potential disparities in model predictions between these two groups and determine if any observed differences are statistically significant.

**Group X:** Recipes before the year 2010 
**Group Y:** Recipes from the year 2010 onward 

We will use the **Mean Absolute Error (MAE)** as our evaluation metric since we are dealing with a regression problem.

**Hypotheses Null Hypothesis (H0):** The model is fair. The MAE for recipes before 2010 and for recipes from 2010 onward are roughly the same, and any differences are due to random chance. 

**Alternative Hypothesis (H1):** The model is unfair. The MAE for recipes before 2010 is higher than the MAE for recipes from 2010 onward. 

**Test Statistic:** The difference in MAE between the two groups. 

**Significance Level Significance Level:** 0.05 

**Steps**

1. Calculate the observed test statistic: Compute the MAE for both groups and find the difference. 
2. Permutation test: Shuffle the labels (year) many times to generate the distribution of the test statistic under the null hypothesis. 
3. Calculate p-value: The proportion of permutations where the test statistic is as extreme or more extreme than the observed difference.

**Results**

**p-value:** The resulting p-value from our permutation test was **0.708**, indicating that the observed difference in MAE is not statistically significant at the 0.05 significance level.
**Observed Difference in MAE:** We observed a slight difference in MAE between recipes from the two time periods, with a value of **-0.00168**

**Conclusion:**

Based on our analysis, we found no compelling evidence to reject the null hypothesis. Thus, we conclude that our predictive model demonstrates fairness and impartiality, as any observed differences in MAE across time periods are likely due to random chance rather than systematic bias.

**Visualization**

<iframe
  src="assets/fairness_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



