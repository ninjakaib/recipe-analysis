# Savory Statistics
An exploration of a dataset of recipes and reviews


## Introduction
Welcome to my data analysis project! In this analysis, I will be exploring a large dataset of recipes and their corresponding reviews. This dataset is comprehensive and contains 234,429 rows, each representing a unique recipe-review pair. 

The primary question driving this analysis is: **What variables have the greatest effect on the rating of a recipe?** This question is of interest not just for amateur and professional chefs looking to create popular recipes, but also for anyone who enjoys cooking or eating - which is to say, practically everyone. By understanding what factors contribute to a higher rating, we can all learn how to better select and improve recipes based on our own tastes and the shared wisdom of the review community.

The relevant columns in the dataset for this analysis are:

- `id`: The unique identifier for each recipe.
- `minutes`: The total time it takes to prepare the recipe.
- `tags`: A list of tags associated with the recipe.
- `n_steps`: The number of steps involved in making the recipe.
- `n_ingredients`: The number of ingredients required by the recipe.
- `ingredients`: A list of the recipe's ingredients.
- `date`: The date the recipe was submitted.
- `rating`: The rating given by the reviewer, on a scale from 1 to 5.
- `rating_mean`: The average rating of all reviews for the recipe.
- `nutrition`: Nutritional information about the recipe, including calories, fat, sugar, sodium, protein, saturated fat, and carbohydrates.

Join me as I delve into this dataset to uncover what makes a recipe truly great!


---

## Cleaning and Exploratory Data Analysis

### Data Cleaning
We begin by loading in the two datasets, one for recipes and one for reviews and merging them together such that there is a row for every recipe review combination. Then ratings with value 0 are filled with np.nan. This is because a rating of 0 in the dataset means the user left a review without a rating. The review could have been just a comment, or they have not yet tried the recipe. Including 0 instead of nan would have skewed the review data in the negative direction. A column `rating_mean` was created from the average rating for each recipe.

The columns `nutrition`, `ingredients`, and `tags` contained lists represented as strings, so I converted the strings to lists and created new columns with values for each individual nutrient. I then added a boolean column `health` which was `True` for recipes containing health related tags. Another column, `review_count` was added which contains the number of reviews for each recipe. Finally, to deal with extreme values in the columns `minutes`, `n_steps`, `n_ingredients`, and `cals`, they were capped at the 99.7th percentile. Not only do these extreme values make visualizations harder to interpret, some or most of these values could be irrelevant. For example, the highest value in the `minutes` column was 2 years for a recipe called 'How to Preserve a Husband'. Column `review_date` was created by converting review dates into `pd.Timestamp` objects containing only year and month data. The resulting dataframe is attached below.


| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                              | nutrition                                    |   n_steps | steps                                             | description                                       | ingredients                                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                                            |   rating_mean | review_date   |   cals |   fat |   sugar |   sodium |   protein |   sat_fat |   carbs | health   |   review_count |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:--------------------------------------------------|:---------------------------------------------|----------:|:--------------------------------------------------|:--------------------------------------------------|:--------------------------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:--------------------------------------------------|--------------:|:--------------|-------:|------:|--------:|---------:|----------:|----------:|--------:|:---------|---------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course',  | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and arrange the rack in t | these are the most; chocolatey, moist, rich, dens | ['bittersweet chocolate', 'unsalted butter', 'egg |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but took forever to bake. |             4 | 2008-11-01    |  138.4 |    10 |      50 |        3 |         3 |        19 |       6 | False    |              1 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing  | this is the recipe that we use at my school cafet | ['white sugar', 'brown sugar', 'salt', 'margarine |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the recipe in half (ju |             5 | 2012-01-01    |  595.1 |    46 |     211 |       22 |        13 |        51 |      26 | False    |              1 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course',  | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart  | since there are already 411 recipes for broccoli  | ['frozen broccoli cuts', 'cream of chicken soup', |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best broccoli casseroles that |             5 | 2008-12-01    |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course',  | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart  | since there are already 411 recipes for broccoli  | ['frozen broccoli cuts', 'cream of chicken soup', |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first birthday party thi |             5 | 2009-04-01    |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course',  | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart  | since there are already 411 recipes for broccoli  | ['frozen broccoli cuts', 'cream of chicken soup', |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to completely thaw the brocc |             5 | 2013-08-01    |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |


### Univariate Analysis

<iframe src="assets/rating_dist.html" width=800 height=600 frameBorder=0></iframe>
This histogram represents the distribution of ratings given to the recipes. From the plot, it's evident that the majority of the ratings are a perfect score of 5. This could suggest that users are more inclined to rate a recipe only if they had a positive experience or found the recipe particularly enjoyable. Lower ratings, from 1 to 4, are significantly less common. This trend could indicate a potential bias in the rating system where users might only choose to rate when they are exceptionally satisfied. It's important to consider this potential bias when analyzing the impact of different variables on the rating.

<iframe src="assets/review_count_dist.html" width=800 height=600 frameBorder=0></iframe>
The second histogram demonstrates the distribution of the number of ratings each recipe received. The majority of the recipes have received only one rating, revealing that a large proportion of recipes in the dataset have limited feedback from users. As the number of ratings increase, the number of recipes receiving that many ratings drops dramatically, creating a highly right-skewed distribution. This pattern suggests that only a small subset of recipes garner a lot of user attention and feedback, potentially due to factors like popularity, visibility, or quality of the recipe. It's crucial to consider this wide variance in the number of ratings per recipe when trying to understand the overall rating system. While we can't make any conclusions about the relationship between the two variables quite yet, one might speculate that a majority of recipes have a single review with a rating of 5, and by looking at this code:

```python
len(df[(df['rating']==5) & (df['review_count']==1)])/len(recipes)
```
```
0.32048650068033707
```
We see that quite a high proportion of those recipes meet that criteria. We should keep this in mind when reviewing the results of any further analyses related to these columns.


### Bivariate Analysis
<iframe src="assets/ratingsproportion_time.html" width=800 height=600 frameBorder=0></iframe>
The line plot provides a visualization of the distribution of ratings over time, with each color representing a specific rating category. The y-axis (log-scaled for clarity) represents the proportion of ratings, while the x-axis represents the date of the review.

From the plot, we observe that the proportions of ratings 3, 4, and 5 are relatively stable over the years, suggesting that overall, the quality of recipes or user satisfaction hasn't drastically changed. However, a notable trend starting around 2012 shows an increase in the proportion of ratings that are 1 or 2. This might suggest an increased willingness of users to rate recipes negatively or perhaps a decline in recipe quality. This growing trend of low ratings may have potential implications for user engagement and recipe submissions.

<iframe src="assets/avgrating_time.html" width=800 height=600 frameBorder=0></iframe>
The scatterplot visualizes the relationship between the mean rating and the review date. The color of the points represents the number of reviews, with lighter hues indicating a higher number of reviews.

The chart reveals a downward trend, with the fitted line indicating a negative correlation between time and mean rating. This implies that over time, average ratings for recipes have been decreasing. It's important to consider this trend, as it could reflect shifts in user behavior or expectations, or changes in the quality of recipe submissions. Further analysis might be necessary to investigate the causes of this trend.

### Interesting Aggregates
Here I found the most popular ingredients and tags. This was done by grouping individual recipes, exploding the ingredients and tags and counting unique ones, then grouping again by individual ingredient or tag to find the mean rating of recipes they appear in. We can then filter by the minimum number of recipes they must appear in. This is necessary because if we did not do this, some obscure ingredient that only appears in a single recipe with a single rating of 5 could be at the top of the list, and this would not be a good representation of the overall data. The two tables showing most popular ingredients were filtered with a minimum recipe count of 200 and 750, respectively. The tags tables were for 750 and 2000 minimum recipes. The lower our filter number, we can expect to see less common but perhaps more creative or unique ingredients/tags, and higher filter numbers will give us the more common, widely loved ingredients like butter or salt.

<div style="display: flex; justify-content: space-around;">
    <table border="1" class="dataframe">
        <thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>ingredients</th><th></th><th></th></tr></thead><tbody><tr><th>goat cheese</th><td>4.823362</td><td>385</td></tr><tr><th>grape tomatoes</th><td>4.823251</td><td>318</td></tr><tr><th>cranberry juice</th><td>4.817460</td><td>254</td></tr><tr><th>vanilla ice cream</th><td>4.815678</td><td>379</td></tr><tr><th>kalamata olive</th><td>4.806052</td><td>249</td></tr></tbody>
    </table>
    <table border="1" class="dataframe">
        <thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>ingredients</th><th></th><th></th></tr></thead><tbody><tr><th>feta cheese</th><td>4.794836</td><td>998</td></tr><tr><th>whipping cream</th><td>4.755685</td><td>818</td></tr><tr><th>bacon</th><td>4.751624</td><td>2667</td></tr><tr><th>fresh cilantro</th><td>4.750262</td><td>1833</td></tr><tr><th>avocado</th><td>4.748168</td><td>907</td></tr></tbody>
    </table>
</div>
<div style="display: flex; justify-content: space-around;">
    <table border="1" class="dataframe">
        <thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>tags</th><th></th><th></th></tr></thead><tbody><tr><th>smoothies</th><td>4.784287</td><td>787</td></tr><tr><th>grilling</th><td>4.783791</td><td>1300</td></tr><tr><th>beverages</th><td>4.782271</td><td>4573</td></tr><tr><th>barbecue</th><td>4.778239</td><td>999</td></tr><tr><th>cocktails</th><td>4.772616</td><td>1840</td></tr></tbody>
    </table>
    <table border="1" class="dataframe">
        <thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>tags</th><th></th><th></th></tr></thead><tbody><tr><th>beverages</th><td>4.782271</td><td>4573</td></tr><tr><th>summer</th><td>4.759649</td><td>2775</td></tr><tr><th>sandwiches</th><td>4.748871</td><td>2182</td></tr><tr><th>salads</th><td>4.745446</td><td>5206</td></tr><tr><th>tomatoes</th><td>4.745283</td><td>2190</td></tr></tbody>
    </table>
</div>

These tables can help us understand which specific tags and ingredients might be associated with higher ratings, but more importantly, they can help me find new delicious ingredients and types of food to try cooking at home!

---

## Assessment of Missingness

### NMAR Analysis
First, lets see the percentage of missing data in each column by running this code:
```python
missing = {}
for col in (list(recipes.columns) + list(reviews.columns)):
    missing[col] = df[col].isna().sum() / df.shape[0] * 100
missing = dict(sorted(missing.items(), key=lambda x: x[1], reverse=True))
missing
```
```
{'rating': 6.413882241531551,
 'description': 0.04862879592541878,
 'review': 0.02474096634802008,
 'name': 0.0004265683853106911,
 'user_id': 0.0004265683853106911,
 'recipe_id': 0.0004265683853106911,
 'date': 0.0004265683853106911,
 'id': 0.0,
 'minutes': 0.0,
 'contributor_id': 0.0,
 'submitted': 0.0,
 'tags': 0.0,
 'nutrition': 0.0,
 'n_steps': 0.0,
 'steps': 0.0,
 'ingredients': 0.0,
 'n_ingredients': 0.0}
 ```
 
 
Although the `rating` column has the highest proportion of missing data, it appears that the missingness in the `rating` column is more likely to be Missing At Random (MAR) rather than NMAR. This is because users might choose to write a review without rating the recipe, irrespective of what their potential rating could have been. They could either not have tried the recipe yet or simply chosen not to leave a rating. In both cases, the missingness does not seem to depend on the unobserved ratings, but rather on factors that could potentially be observed (like whether they tried the recipe or not).

However, a potential NMAR column might be `description`. While it has a low proportion of missing values, it is conceivable that missing descriptions could be related to the actual, unobserved descriptions themselves. For example, if a recipe is simple and straightforward, a user might choose not to include a description because they feel it's unnecessary or redundant. In this case, the missingness in `description` could be dependent on the unobserved data (the unwritten descriptions), which would make this NMAR.

To fully confirm this, we would need additional data, such as user surveys or a mechanism of logging why users chose not to leave a description, which would allow us to better understand the factors influencing the presence or absence of a description. Another test we could perform is a permuatation test of the missingness in `description` against the `n_steps` column, as it is somewhat related to the complexity of a recipe.

### Missingness Dependency
#### Minutes
I performed permutation tests to determine whether the missingness in `description` was dependent on another column or not. Lets start by looking at the results of the test for the `minutes` column.

<iframe src="assets/missingdesc_perm_minutes.html" width=800 height=600 frameBorder=0></iframe>

The test statistic of choice for this permutation test was the difference in means for minutes when the description is missing and when it is not missing. `minutes` is a numerical distribution, so the difference in group means is an acceptable test stat. We can see the observed difference was about -7.2 minutes, and gives a p-value of 0.651, not even close to being statistically significant at any level.

<iframe src="assets/missingdesc_kde_mins.html" width=800 height=600 frameBorder=0></iframe>

After looking at the kernel desnity estimate for the distribution of minutes among the two groups, they look different enough that it would not be unreasonable to perform another permutation test using the KS statistic just to be sure.

```python
ks_2samp(dfm.loc[dfm['missing']==True, 'minutes'], dfm.loc[dfm['missing']==False, 'minutes'])
```
```
KstestResult(statistic=0.09399256736040223, pvalue=0.25011763931689807)
```
From these results, we can conclude it is not likely the missing values in `description` depend on `minutes`.

#### Number of Ingredients
Next, I chose to do the test again but with the `n_ingredients` column.

<iframe src="assets/missingdesc_perm_ingredients.html" width=800 height=600 frameBorder=0></iframe>

Once again we choose the difference in means as a test statistic since the number of ingredients is also a numerical distribution. The observed difference between the groups was roughly -1.1, and when permutating the missing labels and plotting the empircal distribution of difference in means, we can see this results in a p-value of just 0.00033. This is low enough to be statistically significant at any commonly used significance level (0.05 or 0.01), and we reject the null hypothesis that the mean number of ingredients is the same for recipes with or without a description.

<iframe src="assets/missingdesc_kde_ingredients.html" width=800 height=600 frameBorder=0></iframe>

The KDE for the two groups looks very different this time, and after performing another KS test

```python
ks_2samp(dfm.loc[dfm['missing']==True, 'n_ingredients'], dfm.loc[dfm['missing']==False, 'n_ingredients'])
```
```
KstestResult(statistic=0.14958473579762732, pvalue=0.010841034077387445)
```
Again we see a low p-value and can reject the null at a significance level of 5%. From these results, we can conclude that the missingness in `description` is probably MAR. It can't be (or is extremely unlikely to be) MCAR, since this permutation test shows a statistically siginicant result.


---

## Hypothesis Testing

Finally, I conducted a permutation test to see if ratings were different based on if recipes are healthy or not.  

**Null Hypothesis**: The distribution of ratings are the same for healthy and not healthy recipes.  
**Alternative Hypothesis**: The distribution of ratings is different for healthy and not healthy recipes.  

To classify recipes as healthy or not, we created a column `health` in the beginning that is `True` if a recipe contains tags with any of these keywords: `['diet', 'health', 'low', 'vegan']`, and `False` otherwise. Although the rating is given as a number, we will consider it to be a categorical variable since there are only 5 distinct ratings a user can give. We can compare the distribution of ratings between the two groups (healthy or not) by using TVD (total variation distance). The TVD is an appropriate test statistic for comparing two categorical distributions. I chose a significance level of 1% (0.01) to be most confident about the results.

<iframe src="assets/health_dist.html" width=800 height=600 frameBorder=0></iframe>

This shows us the distribution in ratings, conditional on `health` and gives us an observed TVD of 0.0122. Now we will run the permutation test and interpret the results.

<iframe src="assets/health_perm.html" width=800 height=600 frameBorder=0></iframe>

From the test, we can see that after shuffling the health labels and plotting the TVDs, all of them were less than the observed test statistic. This means we have a p-value of 0, and this is confirmed by running
```
np.mean(observed_tvd <= tvds)
```
```
0.00
```
At a significance level of 0.01, we reject the null hypothesis that the distribution of ratings is the same for healthy and non-healthy recipes. This suggests that there may be a difference in the ratings of healthy and non-healthy recipes, but further investigation is required to confirm this.

---

## Additional Interesting Figures

<iframe src="assets/minutes_dist.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/ingredients_time.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/health_nutrients.html" width=800 height=600 frameBorder=0></iframe>


Thank you for visiting my project! Feel free to contact me for any further questions or clarifications.
