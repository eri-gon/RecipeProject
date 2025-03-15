# Time is of the E-scent: Time Relationships in Food.com
by Eric Gan

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

<iframe
  src="assets/recipes_df.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Univariate Analysis
The first distribution I was interested in was the distribution of `'date'`. The graph is below:
<iframe
  src="assets/reviews-time.html"
  width="800"
  height = "500"
  frameborder="0"
></iframe>
This graph shows the amount of reviews over time. Food.com’s reviews peaked in June 2010. Around 2013, there has been a significant decrease in reviews. Just through distribution of reviews alone, there is a clear difference between before and after 2013. 

### Bivariate Analysis
Then I wanted to see how time affected the ratings of the reviews. I did this by graphing `'rating'` over `'date'`:
<iframe
  src="assets/rating-time.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Because of how many ratings there were, I needed to average the ratings per month to declutter the graph. This graph shows the average rating of all the reviews per month. The reviews significantly fluctuate per month, but in general, it seems to be going down. 

### Interesting Aggregates
We group the recipes_df by whether a recipe was submitted before or after 2013. 
<iframe
  src="assets/agg-df.html"
  width="800"
  frameborder="0"
></iframe>
There are some cool trends seen already. It seems that after 2013, the complexity of recipes were increasing, with average steps and ingredients increased. Most of the tags decreased in usage. Furthermore, all average nutrition increased. 

---

## Assessment of Missingness
The three columns in the combined dataframe that have missing values are `description`, `rating`, and `review`.

### NMAR missingness
I believe that `‘review’` is *NMAR*. Recipes with `‘review’` missing are recipes that never received a review from Food.com users. This would mean that users found the recipe not memorable enough to leave a typed review behind. As a result, the value of the review is the cause of its missingness. 

### Missingness Dependency
`‘rating’` missingness is caused whenever a user leaves a 0 review which means that the recipe was compromised in some way. Using permutation testing, I checked if the rating's missingness is caused by `n_ingredients` or `minutes` with permutation testing:

*Null hypothesis:* The missingness of ratings does not depend on the number of ingredients in the recipe. 

*Alternate Hypothesis:* The missingness of ratings does depend on the number of ingredients. 

*Test statistic:* The absolute mean difference between the group with missing ratings and the group without missing ratings. 

*Significance level:* 0.05

<iframe
  src="assets/perm1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I got a *p-value* of **0**. I *reject* the null hypothesis. This means that the missingness of `'rating'` does depend on the number of ingredients. This makes sense, because the more ingredients, the more likely the user would compromise the recipe by not having the proper ingredient. 

*Null hypothesis:* The missingness of `'rating'` does not depend on the duration of the recipe. 

*Alternate hypothesis:* The missingness of `'rating'` does depend on the duration of the recipe. 

*Test statistic:* The absolute mean difference between the group with missing `'rating'` and the group without missing `'rating'`. 

*Significance level:* 0.05

<iframe
  src="assets/perm2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I got a *p-value* of **0.106**. I *fail to reject* the null hypothesis. This means that the missingness of `'rating'` is not dependent on `'minutes'`. 

---

## Hypothesis Testing
To figure out if time of recipe submission had an influence on the rating it recieved, I did a hypothesis test: 

*Null hypothesis:* The distribution of ratings posted after 2013 and the distribution of ratings posted before 2013 are the same.

*Alternate hypothesis:* The distribution of ratings posted  after 2013 and the distribution of ratings posted before 2013 are different. 

*Test statistic:* Absolute mean difference between rating posted after 2013 and rating posted before 2013

*Significance level:* 0.05

<iframe
  src="assets/hypo.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the hypothesis test, with a *p-value* of **0**, we reject the null hypothesis. This suggests that the ratings after 2013 was different from the distribution of ratings before 2013. 

---

## Framing a Prediction Problem
Time is of the essence and I want to be able to predict how long a recipe would take. To do this, I will be creating a multiclass classification problem. Each recipe has already been given a `'min_category'` that was derived from how many minutes the recipe took. I did a 0.8 to 0.2 training test split. I used accuracy as the metric to evaluate my model, because I am only interested in the overall correctness of my model, and not interested in the recall nor percision. 

---

## Baseline Model
I used a random forest classifier with max_depth of 3 and 100 trees. I used two columns to make features for the model. First, I one hot encoded submitted_past_2013. Second, I used Countvectorizer to transform tags into features. I used these features, because I noticed that there was a difference between recipes submitted past and before 2013. I used tags, because there are many tags that can be used to determine the time category of each recipe. Tags such as 'easy', 'oven' and '60-min-or-less'. 

After fitting the model, the accuracy score was **0.6641**. I think this is an acceptable accuracy, because my model performs better than random guessing. 

---

## Final Model
In my final model, I added two extra features, n_steps and n_ingredients. Furthermore, I used grid search to optimize the max_depth with depth 3,15,19,23,27. The main reason I chose these hyperparameters was because my choice of Countvectorizing each recipe tag was extremely time consuming as it created over 800 features. Using any tree depth greater than 20 made my computer struggle. 

My final model used a tree depth of 27. The accuracy of the model increased to **0.7837**. This makes sense, because increasing the tree depth would in turn increase the accuracy of the model when it is not overtraining. 

---

## Fairness Analysis
I wondered if there was a difference in accuracy between my models predictions on recipes before and after 2013. 

Group X represents recipes posted after 2013. Group Y represents recipes posted before 2013. 

*Null hypothesis:* The model is fair. The accuracy is the same for both groups. 

*Alternative hypothesis:* The model is unfair. The accuracy for predicting the groups are different. 

*Significance level:* 0.05

<iframe
  src="assets/fair.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After performing the permutation test, we get a *p-value* of **0.04**, meaning we reject the null hypothesis. This shows that My final model does have a biased accuracy based on whether a recipe was submitted before or after 2013. 

---
