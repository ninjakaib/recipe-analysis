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

'| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                        | nutrition                                    |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |          user_id |   recipe_id | date                |   rating | review                                                                                                                                                                                                                                                                                                                                           |   rating_mean | review_date         |   cals |   fat |   sugar |   sodium |   protein |   sat_fat |   carbs | health   |   review_count |\n|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|------------:|:--------------------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------:|:--------------------|-------:|------:|--------:|---------:|----------:|----------:|--------:|:---------|---------------:|\n| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'for-large-groups\', \'desserts\', \'lunch\', \'snacks\', \'cookies-and-brownies\', \'chocolate\', \'bar-cookies\', \'brownies\', \'number-of-servings\'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | [\'heat the oven to 350f and arrange the rack in the middle\', \'line an 8-by-8-inch glass baking dish with aluminum foil\', \'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted\', \'remove from heat and let cool to room temperature\', \'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated\', \'add cooled chocolate and mix until uniform in color\', \'add flour and stir until just incorporated\', \'transfer batter to the prepared baking dish\', \'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes\', \'remove from the oven and cool completely before cutting\']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you\'ll ever make.....sereiously! there\'s no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they\'re pure heaven!                                                                                                              | [\'bittersweet chocolate\', \'unsalted butter\', \'eggs\', \'granulated sugar\', \'unsweetened cocoa powder\', \'vanilla extract\', \'brewed espresso\', \'kosher salt\', \'all-purpose flour\'] |               9 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |             4 | 2008-11-01 00:00:00 |  138.4 |    10 |      50 |        3 |         3 |        19 |       6 | False    |              1 |\n| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | [\'60-minutes-or-less\', \'time-to-make\', \'cuisine\', \'preparation\', \'north-american\', \'for-large-groups\', \'canadian\', \'british-columbian\', \'number-of-servings\']                                                               | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | [\'pre-heat oven the 350 degrees f\', \'in a mixing bowl , sift together the flours and baking powder\', \'set aside\', \'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy\', \'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined\', \'add in the flour mixture to the wet ingredients and blend until combined\', \'scrape down the sides of the bowl and add the chocolate chips\', \'mix until combined\', \'scrape down the sides to the bowl again\', \'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking\', \'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center\', \'serve hot and enjoy !\'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don\'t have margarine or don\'t like it, then just use butter (softened) instead.                                                                                                                                            | [\'white sugar\', \'brown sugar\', \'salt\', \'margarine\', \'eggs\', \'vanilla\', \'water\', \'all-purpose flour\', \'whole wheat flour\', \'baking soda\', \'chocolate chips\']                    |              11 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |             5 | 2012-01-01 00:00:00 |  595.1 |    46 |     211 |       22 |        13 |        51 |      26 | False    |              1 |\n| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'side-dishes\', \'vegetables\', \'easy\', \'beginner-cook\', \'broccoli\']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | [\'preheat oven to 350 degrees\', \'spray a 2 quart baking dish with cooking spray , set aside\', \'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce\', \'pour into baking dish , sprinkle remaining cheese over top\', \'bake for 25 minutes or until cheese is lightly browned\', \'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes\']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | [\'frozen broccoli cuts\', \'cream of chicken soup\', \'sharp cheddar cheese\', \'garlic powder\', \'ground black pepper\', \'salt\', \'milk\', \'soy sauce\', \'french-fried onions\']          |               9 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |             5 | 2008-12-01 00:00:00 |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |\n|                                      |        |           |                  |             |                                                                                                                                                                                                                             |                                              |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |                  |             |                     |          | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |               |                     |        |       |         |          |           |           |         |          |                |\n|                                      |        |           |                  |             |                                                                                                                                                                                                                             |                                              |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |                  |             |                     |          | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family\'s favorite Zaar cookbook :)                                                                                                                                                                                                                          |               |                     |        |       |         |          |           |           |         |          |                |\n| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'side-dishes\', \'vegetables\', \'easy\', \'beginner-cook\', \'broccoli\']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | [\'preheat oven to 350 degrees\', \'spray a 2 quart baking dish with cooking spray , set aside\', \'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce\', \'pour into baking dish , sprinkle remaining cheese over top\', \'bake for 25 minutes or until cheese is lightly browned\', \'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes\']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | [\'frozen broccoli cuts\', \'cream of chicken soup\', \'sharp cheddar cheese\', \'garlic powder\', \'ground black pepper\', \'salt\', \'milk\', \'soy sauce\', \'french-fried onions\']          |               9 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son\'s first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |             5 | 2009-04-01 00:00:00 |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |\n| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'side-dishes\', \'vegetables\', \'easy\', \'beginner-cook\', \'broccoli\']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | [\'preheat oven to 350 degrees\', \'spray a 2 quart baking dish with cooking spray , set aside\', \'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce\', \'pour into baking dish , sprinkle remaining cheese over top\', \'bake for 25 minutes or until cheese is lightly browned\', \'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes\']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | [\'frozen broccoli cuts\', \'cream of chicken soup\', \'sharp cheddar cheese\', \'garlic powder\', \'ground black pepper\', \'salt\', \'milk\', \'soy sauce\', \'french-fried onions\']          |               9 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |             5 | 2013-08-01 00:00:00 |  194.8 |    20 |       6 |       32 |        22 |        36 |       3 | False    |              4 |'


### Univariate Analysis
To get an idea of the distribution of individual variables...

Here are some of the key findings:

### Bivariate Analysis
I then looked at the relationship between different pairs of variables...

Here are some of the key findings:

### Interesting Aggregates
I found some interesting aggregates...
<table>
  <tr>
    <td>
      '| ingredients       |   mean_rating |   count |\n|:------------------|--------------:|--------:|\n| goat cheese       |       4.82336 |     385 |\n| grape tomatoes    |       4.82325 |     318 |\n| cranberry juice   |       4.81746 |     254 |\n| vanilla ice cream |       4.81568 |     379 |\n| kalamata olive    |       4.80605 |     249 |'
    </td>
    <td>
      '| ingredients    |   mean_rating |   count |\n|:---------------|--------------:|--------:|\n| feta cheese    |       4.79484 |     998 |\n| whipping cream |       4.75569 |     818 |\n| bacon          |       4.75162 |    2667 |\n| fresh cilantro |       4.75026 |    1833 |\n| avocado        |       4.74817 |     907 |'
    </td>
  </tr>
</table>
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
