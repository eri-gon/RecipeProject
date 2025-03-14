# Time is of the e-scent: Time Relationships in Food.com
by Eric Gan

---

## Introduction
Food.com is a popular recipe-sharing website allowing users to share and rate recipes. In many rating sites, I have observed grade inflation, where, over time, ratings become more lenient. I wondered: How did time affect recipes and reviews on the site? To answer this question, I analyzed two datasets of recipes and reviews of Food.com posted since 2008.

Recipes is a dataset with 83,782 rows and 13 columns.

Of the columns, we are interested in: 
- `‘id’`: Recipe ID
- `‘minutes’`: Minutes to prepare recipe
- `‘submitted’`: Date recipe was submitted
- `‘tags’`: Food.com tags for recipe
- `‘nutrition’`: Nutritional information
- `‘n_steps’`: Number of steps in recipe
- `‘n_ingredients’`: Number of ingredients in the recipe
- `‘description’`: Description of the recipe


Interactions is a dataset with 731,927 rows and 5 columns, which contains the reviews that were left on the recipes.

We are interested in: 
- `‘recipe_id’`: The ID of the recipe that was reviewed
- `‘date’`: the date of the review
- `‘rating’`: Rating given
- `‘review’`: The review text

---

## Data Cleaning and Exploratory Data Analysis
1. Left merge the recipes and interactions on `'id'` and `'recipe_id'`
2. Filled all 0 ratings with `np.nan`. This is appropriate, because the 0 ratings do not represent a bad review, but instead a review where the author compromised the recipe.
3. Add column `‘average_rating’` containing average rating per recipe
4. The nutrition column was originally in the format of `'[calories, total fat, ...]'`Using `eval` and `tolist`, split the nutrition column into individual columns of floats for each nutrient.
5. Converted the `'date'` and `'submitted'` columns into pandas datetime.
6. Created a col `'submitted_past_2013'` as booleans on whether a recipe was submitted before or after 2013 based on the `'submitted'` column. 
7. I looked at the first 100 most used tags and extracted 5 tags I thought to be relevant to time, creating 5 columns with booleans of whether each row had the tag. The columns are: `'easy'`, `'equipment'`, `'occasion'`, `'weeknight'`, `'from-scratch'`.
8. The `'tags'` column was in the format of strings ['tag1', 'tag2', ...]. Using `eval` and `' '.join()`, I convert the `tags` into string corpuses. This will be used later in my prediction model. 
9. Created a `min_category` column that categorizes the data depending on the minutes in the following bins: 0-30, 30-60, `60-90, 90-120, and 120+
10. Drop all the unnecessary rows

After cleaning the entire dataframe, I end up with a dataframe with 234,428 rows and 23 columnns `recipes_df.head()`:

|     id |   minutes | submitted           | tags                                                                                                                                                                             |   n_steps |   n_ingredients | date                |   rating |   avg_rating | easy   | equipment   | occasion   | weeknight   | from-scratch   |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates | submitted_past_2013   | min_category   |
|-------:|----------:|:--------------------|:---------------------|----------:|----------------:|:--------------------|---------:|-------------:|:-------|:------------|:-----------|:------------|:---------------|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|:----------------------|:---------------|
| 333281 |        40 | 2008-10-27 00:00:00 | 60-minutes-or-less time-to-make course main-ingredient preparation for-large-groups desserts lunch snacks cookies-and-brownies chocolate bar-cookies brownies number-of-servings |        10 |               9 | 2008-11-19 00:00:00 |        4 |            4 | False  | False       | False      | False       | False          |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | False                 | 30-60          |
| 453467 |        45 | 2011-04-11 00:00:00 | 60-minutes-or-less time-to-make cuisine preparation north-american for-large-groups canadian british-columbian number-of-servings                                                |        12 |              11 | 2012-01-26 00:00:00 |        5 |            5 | False  | False       | False      | False       | False          |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | False                 | 30-60          |
| 306168 |        40 | 2008-05-30 00:00:00 | 60-minutes-or-less time-to-make course main-ingredient preparation side-dishes vegetables easy beginner-cook broccoli                                                            |         6 |               9 | 2008-12-31 00:00:00 |        5 |            5 | True   | False       | False      | False       | False          |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | False                 | 30-60          |
| 306168 |        40 | 2008-05-30 00:00:00 | 60-minutes-or-less time-to-make course main-ingredient preparation side-dishes vegetables easy beginner-cook broccoli                                                            |         6 |               9 | 2009-04-13 00:00:00 |        5 |            5 | True   | False       | False      | False       | False          |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | False                 | 30-60          |
| 306168 |        40 | 2008-05-30 00:00:00 | 60-minutes-or-less time-to-make course main-ingredient preparation side-dishes vegetables easy beginner-cook broccoli                                                            |         6 |               9 | 2013-08-02 00:00:00 |        5 |            5 | True   | False       | False      | False       | False          |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | False                 | 30-60          |


---

## Assessment of Missingness

---

## Hypothesis Testing

---

## Framing a Prediction Problem

---

## Baseline Model

---

## Final Model

---

## Fairness Analysis

---
