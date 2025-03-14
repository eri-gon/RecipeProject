# Time is of the e-scent: Time Relationships in Food.com
by Eric Gan

---

## Introduction
Food.com is a popular recipe-sharing website allowing users to share and rate recipes. In many rating sites, I have observed grade inflation, where, over time, ratings become more lenient. I wondered: How did time affect recipes and reviews on the site? To answer this question, I analyzed two datasets of recipes and reviews of Food.com posted since 2008.

Recipes is a dataset with 83,782 rows and 13 columns.

Of the columns, we are interested in: 
- `‘id’`: Recipe ID
- ‘minutes’: Minutes to prepare recipe
- ‘submitted’: Date recipe was submitted
- ‘tags’: Food.com tags for recipe
- ‘nutrition’: Nutritional information
- ‘n_steps’: Number of steps in recipe
- ‘n_ingredients’: Number of ingredients in the recipe
- ‘description’: Description of the recipe


Interactions is a dataset with 731,927 rows and 5 columns, which contains the reviews that were left on the recipes.

We are interested in: 
- ‘recipe_id’: The ID of the recipe that was reviewed
- ‘date’: the date of the review
- ‘rating’: Rating given
- ‘review’: The review text

---

## Data Cleaning and Exploratory Data Analysis
1. Left merge the recipes and interactions on 'id' and 'recipe_id'
2. Filled all 0 ratings with np.nan. This is appropriate, because the 0 ratings do not represent a bad review, but instead a review where the author compromised the recipe.
3. Add column ‘average_rating’ containing average rating per recipe
4. The nutrition column was originally in the format of [calories, total fat, ...]Split the nutrition column into individual columns of floats for each nutrient.
5. Converted the date and submitted columns into pandas datetime.
6. Created a col submitted_past_2013 as booleans on whether a recipe was submitted before or after 2013 based on the 'submitted' column. 
7. I looked at the first 100 most used tags and extracted 5 tags I thought to be relevant to time, creating 5 columns with booleans of whether each row had the tag. The columns are: 'easy', 'equipment', 'occasion', 'weeknight', 'from-scratch'.
8. The 'tags' column was in the format of lists. Using 'eval' and '' '.join()'

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
