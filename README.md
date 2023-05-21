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

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                        | nutrition                                    |   n_steps | steps                                             | description                                       | ingredients                                                                                                                                                                    |   n_ingredients |          user_id |   recipe_id | date                |   rating | review                                            |   rating_mean | review_date         |   cals |   fat |   sugar |   sodium |   protein |   sat_fat |   carbs | health   |   review_count |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------|----------:|:--------------------------------------------------|:--------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|------------:|:--------------------|---------:|:--------------------------------------------------|--------------:|:--------------------|-------:|------:|--------:|---------:|----------:|----------:|--------:|:---------|---------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and arrange the rack in t | these are the most; chocolatey, moist, rich, dens | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to bake. |             4 | 2008-11-01 00:00:00 |  138.4 |    10 |      50 |        3 |         3 |        19 |       6 | False    |              1 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing  | this is the recipe that we use at my school cafet | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half (ju |             5 | 2012-01-01 00:00:00 |  595.1 |    46 |     211 |       22 |        13 |        51 |      26 | False    |              1 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart  | since there are already 411 recipes for broccoli  | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles that |             5 | 2008-12-01 00:00:00 |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart  | since there are already 411 recipes for broccoli  | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son's first birthday party thi |             5 | 2009-04-01 00:00:00 |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart  | since there are already 411 recipes for broccoli  | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the brocc |             5 | 2013-08-01 00:00:00 |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |

### Univariate Analysis
To get an idea of the distribution of individual variables...

Here are some of the key findings:

<iframe src="assets/rating_dist.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/review_count_dist.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis
I then looked at the relationship between different pairs of variables...

Here are some of the key findings:

<iframe src="assets/ratingsproportion_time.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/avgrating_time.html" width=800 height=600 frameBorder=0></iframe>

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
