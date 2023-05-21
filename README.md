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
The scatterplot visualizes the relationship between the mean rating and the review date. The color of the points represents the number of reviews, with darker hues indicating a higher number of reviews.

The chart reveals a downward trend, with the fitted line indicating a negative correlation between time and mean rating (r^2 = 0.459). This implies that over time, average ratings for recipes have been decreasing. It's important to consider this trend, as it could reflect shifts in user behavior or expectations, or changes in the quality of recipe submissions. Further analysis might be necessary to investigate the causes of this trend.

### Interesting Aggregates
Here I found the most popular ingredients and tags. This was done by grouping individual recipes, exploding the ingredients and tags and counting unique ones, then grouping again by individual ingredient or tag to find the mean rating of recipes they appear in. We can then filter by the minimum number of recipes they must appear in. This is necessary because if we did not do this, some obscure ingredient that only appears in a single recipe with a single rating of 5 could be at the top of the list, and this would not be a good representation of the overall data. The two tables showing most popular ingredients were filtered with a minimum recipe count of 200 and 750, respectively. The tags tables were for 750 and 2000 minimum recipes. The lower our filter number, we can expect to see less common but perhaps more creative or unique ingredients/tags, and higher filter numbers will give us the more common, widely loved ingredients like butter or salt.

| ingredients       |   mean_rating |   count |
|:------------------|--------------:|--------:|
| goat cheese       |       4.82336 |     385 |
| grape tomatoes    |       4.82325 |     318 |
| cranberry juice   |       4.81746 |     254 |
| vanilla ice cream |       4.81568 |     379 |
| kalamata olive    |       4.80605 |     249 |

| ingredients    |   mean_rating |   count |
|:---------------|--------------:|--------:|
| feta cheese    |       4.79484 |     998 |
| whipping cream |       4.75569 |     818 |
| bacon          |       4.75162 |    2667 |
| fresh cilantro |       4.75026 |    1833 |
| avocado        |       4.74817 |     907 |

| tags      |   mean_rating |   count |
|:----------|--------------:|--------:|
| smoothies |       4.78429 |     787 |
| grilling  |       4.78379 |    1300 |
| beverages |       4.78227 |    4573 |
| barbecue  |       4.77824 |     999 |
| cocktails |       4.77262 |    1840 |

| tags       |   mean_rating |   count |
|:-----------|--------------:|--------:|
| beverages  |       4.78227 |    4573 |
| summer     |       4.75965 |    2775 |
| sandwiches |       4.74887 |    2182 |
| salads     |       4.74545 |    5206 |
| tomatoes   |       4.74528 |    2190 |

|     |     |
| --- | --- |
| `<table border="1" class="dataframe"><thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>ingredients</th><th></th><th></th></tr></thead><tbody><tr><th>goat cheese</th><td>4.823362</td><td>385</td></tr><tr><th>grape tomatoes</th><td>4.823251</td><td>318</td></tr><tr><th>cranberry juice</th><td>4.817460</td><td>254</td></tr><tr><th>vanilla ice cream</th><td>4.815678</td><td>379</td></tr><tr><th>kalamata olive</th><td>4.806052</td><td>249</td></tr></tbody></table>` | `<table border="1" class="dataframe"><thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>ingredients</th><th></th><th></th></tr></thead><tbody><tr><th>feta cheese</th><td>4.794836</td><td>998</td></tr><tr><th>whipping cream</th><td>4.755685</td><td>818</td></tr><tr><th>bacon</th><td>4.751624</td><td>2667</td></tr><tr><th>fresh cilantro</th><td>4.750262</td><td>1833</td></tr><tr><th>avocado</th><td>4.748168</td><td>907</td></tr></tbody></table>` |
| `<table border="1" class="dataframe"><thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>tags</th><th></th><th></th></tr></thead><tbody><tr><th>smoothies</th><td>4.784287</td><td>787</td></tr><tr><th>grilling</th><td>4.783791</td><td>1300</td></tr><tr><th>beverages</th><td>4.782271</td><td>4573</td></tr><tr><th>barbecue</th><td>4.778239</td><td>999</td></tr><tr><th>cocktails</th><td>4.772616</td><td>1840</td></tr></tbody></table>` | `<table border="1" class="dataframe"><thead><tr style="text-align: right;"><th></th><th>mean_rating</th><th>count</th></tr><tr><th>tags</th><th></th><th></th></tr></thead><tbody><tr><th>beverages</th><td>4.782271</td><td>4573</td></tr><tr><th>summer</th><td>4.759649</td><td>2775</td></tr><tr><th>sandwiches</th><td>4.748871</td><td>2182</td></tr><tr><th>salads</th><td>4.745446</td><td>5206</td></tr><tr><th>tomatoes</th><td>4.745283</td><td>2190</td></tr></tbody></table>` |


These tables can help us understand which specific tags and ingredients might be associated with higher ratings, but more importantly, they can help me find new delicious ingredients and types of food to try cooking at home!

---

## Assessment of Missingness

### NMAR Analysis
I performed an analysis to determine whether the data is Not Missing At Random (NMAR)...

### Missingness Dependency
Then, I checked whether missingness in some variables depends on others...

---

## Hypothesis Testing
Finally, I conducted hypothesis tests to answer some of the main questions about this data...

---

Thank you for visiting my project! Feel free to contact me for any further questions or clarifications.
