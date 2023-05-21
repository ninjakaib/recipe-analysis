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

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>id</th>
      <th>minutes</th>
      <th>contributor_id</th>
      <th>submitted</th>
      <th>tags</th>
      <th>nutrition</th>
      <th>n_steps</th>
      <th>steps</th>
      <th>description</th>
      <th>ingredients</th>
      <th>n_ingredients</th>
      <th>user_id</th>
      <th>recipe_id</th>
      <th>date</th>
      <th>rating</th>
      <th>review</th>
      <th>rating_mean</th>
      <th>review_date</th>
      <th>cals</th>
      <th>fat</th>
      <th>sugar</th>
      <th>sodium</th>
      <th>protein</th>
      <th>sat_fat</th>
      <th>carbs</th>
      <th>health</th>
      <th>review_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 brownies in the world    best ever</td>
      <td>333281</td>
      <td>40</td>
      <td>985201</td>
      <td>2008-10-27</td>
      <td>[60-minutes-or-less, time-to-make, course, main-ingredient, preparation, for-large-groups, desserts, lunch, snacks, cookies-and-brownies, chocolate, bar-cookies, brownies, number-of-servings]</td>
      <td>[138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]</td>
      <td>10</td>
      <td>['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']</td>
      <td>these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!</td>
      <td>[bittersweet chocolate, unsalted butter, eggs, granulated sugar, unsweetened cocoa powder, vanilla extract, brewed espresso, kosher salt, all-purpose flour]</td>
      <td>9</td>
      <td>386585.0</td>
      <td>333281.0</td>
      <td>2008-11-19</td>
      <td>4.0</td>
      <td>These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.</td>
      <td>4.0</td>
      <td>2008-11-01</td>
      <td>138.4</td>
      <td>10.0</td>
      <td>50.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>19.0</td>
      <td>6.0</td>
      <td>False</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 in canada chocolate chip cookies</td>
      <td>453467</td>
      <td>45</td>
      <td>1848091</td>
      <td>2011-04-11</td>
      <td>[60-minutes-or-less, time-to-make, cuisine, preparation, north-american, for-large-groups, canadian, british-columbian, number-of-servings]</td>
      <td>[595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]</td>
      <td>12</td>
      <td>['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft &amp; chewy in the center', 'serve hot and enjoy !']</td>
      <td>this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.</td>
      <td>[white sugar, brown sugar, salt, margarine, eggs, vanilla, water, all-purpose flour, whole wheat flour, baking soda, chocolate chips]</td>
      <td>11</td>
      <td>424680.0</td>
      <td>453467.0</td>
      <td>2012-01-26</td>
      <td>5.0</td>
      <td>Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, &amp; I made the whole batch &amp; used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe!</td>
      <td>5.0</td>
      <td>2012-01-01</td>
      <td>595.1</td>
      <td>46.0</td>
      <td>211.0</td>
      <td>22.0</td>
      <td>13.0</td>
      <td>51.0</td>
      <td>26.0</td>
      <td>False</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>50969</td>
      <td>2008-05-30</td>
      <td>[60-minutes-or-less, time-to-make, course, main-ingredient, preparation, side-dishes, vegetables, easy, beginner-cook, broccoli]</td>
      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>
      <td>6</td>
      <td>['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>[frozen broccoli cuts, cream of chicken soup, sharp cheddar cheese, garlic powder, ground black pepper, salt, milk, soy sauce, french-fried onions]</td>
      <td>9</td>
      <td>29782.0</td>
      <td>306168.0</td>
      <td>2008-12-31</td>
      <td>5.0</td>
      <td>This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!  \nThe photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.  \nThanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)</td>
      <td>5.0</td>
      <td>2008-12-01</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
      <td>False</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>50969</td>
      <td>2008-05-30</td>
      <td>[60-minutes-or-less, time-to-make, course, main-ingredient, preparation, side-dishes, vegetables, easy, beginner-cook, broccoli]</td>
      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>
      <td>6</td>
      <td>['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>[frozen broccoli cuts, cream of chicken soup, sharp cheddar cheese, garlic powder, ground black pepper, salt, milk, soy sauce, french-fried onions]</td>
      <td>9</td>
      <td>1196280.0</td>
      <td>306168.0</td>
      <td>2009-04-13</td>
      <td>5.0</td>
      <td>I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.</td>
      <td>5.0</td>
      <td>2009-04-01</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
      <td>False</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>50969</td>
      <td>2008-05-30</td>
      <td>[60-minutes-or-less, time-to-make, course, main-ingredient, preparation, side-dishes, vegetables, easy, beginner-cook, broccoli]</td>
      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>
      <td>6</td>
      <td>['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>[frozen broccoli cuts, cream of chicken soup, sharp cheddar cheese, garlic powder, ground black pepper, salt, milk, soy sauce, french-fried onions]</td>
      <td>9</td>
      <td>768828.0</td>
      <td>306168.0</td>
      <td>2013-08-02</td>
      <td>5.0</td>
      <td>Loved this.  Be sure to completely thaw the broccoli.  I didn&amp;#039;t and it didn&amp;#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.</td>
      <td>5.0</td>
      <td>2013-08-01</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
      <td>False</td>
      <td>4</td>
    </tr>
  </tbody>
</table>

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
