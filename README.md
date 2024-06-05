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


