# Ingredients to Insights: A Statistical Analysis of Recipes

Welcome to **Ingredients to Insights:** This is a detailed data science project undertaken at UCSD. It focuses on exploring the relationship between the complexity of a recipe to its average rating. We analyze a dataset containing information about various recipes from [food.com](https://www.food.com/)

Author: Dhanvi Patel

## Introduction

Cooking is a hobby for a lot of people. Some people, especially avid bakers like myself, enjoy detailed recipes and the delicious dishes they result in. For others, cooking is a mode of sustenance. They prefer sticking to simpler recipes with less complexity. Through this project, I wanted to analyze the relationship between the number of steps in a recipe and its average rating. Do recipes with more steps receive higher ratings due to readers feeling biased by the effort they've invested in making them? Or do lower ratings result from disappointment that stems from the complexity of a recipe?

The primary relationship we are focusing on is:  

**What is the relationship between the number of steps in a recipe and average rating of recipes?**

### Dataset 

We are analyzing two datasets consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/)

The first dataset, `recipe`, contains 83782 rows, indicating 83782 unique recipes, with 10 columns recording the following information:

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


Fun fact: The mushroom is the only non-animal natural source of vitamin D! 

## Data Cleaning and Exploratory Data Analysis

To optimize our dataset analysis, we performed the following data cleaning steps:

1. Left merge the recipes and interactions datasets together on `id` and `recipe_id` respectively 

	- This step matches unique recipes to ratings and reviews 


2. Fill all ratings of 0 with np.nan

	- Ratings range from 1 to 5; 1 being worst and 5 being the best. Thus, a value of 0 in the ratings column is a clear indication of missing values. To avoid misinterpretation of statistical facts, we place 0 with np.nan


3. Find the average rating per recipe, as a Series.

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

The resulting dataframe has 234429 rows and 18 columns. Let's look at the first couple rows of recipes of our **cleaned** dataframe for illustration purposes. 

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


### Useful Aggregates 

This pivot table provides a statistical summary of the relationship between the number of steps in a recipe and its ratings. The table is grouped by the number of steps (n_steps) and includes three key metrics for each group:

	1. Average Rating: The mean rating of recipes that have a specific number of steps. This value helps identify whether recipes with more steps tend to receive higher or lower ratings on average.For example, if recipes with more steps consistently have higher average ratings, it might indicate that users appreciate the detailed instructions. Conversely, if they have lower ratings, it might suggest that users find complex recipes frustrating or difficult to follow.

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



## Hypothesis Testing 







