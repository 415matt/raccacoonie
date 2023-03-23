## Framing the Problem:

This dataset was obtained and cleaned previously in my [Recipe-Analysis](https://github.com/415matt/Recipe-Analysis) repo. There I discovered and visualized some interesting relations between features in the dataset. 

In this project, I will create a regression model to predict the average rating of a recipe given its features. I thought that this would be an interesting project, as I have always wondered if there is some kind of "golden ratio" of ingredients and cook time that will produce some sort of predictable trend. 

 I will be using RMSE and R^2 to evaluate my error on both my training and test set. I specifically chose RMSE as it takes into account the affect of large errors, something I want to avoid in my model. Additionally I am also looking at the R^2 error as it is easily accessible through the `sklearn` library and adds an additional metric for my model's performance.

 My model will be trained only on features that can be known during the time of prediction (before the food is tasted). Each model I trained has different features and processes - keep reading to learn more. 
 
---
## Baseline Model

I decided to start simple with baseline model, using only two features: `n_steps` and `calories (#)`, both of which are quantitative descreet features. For this model I used linear regression with my data being preprocessed through the [StandardScaler()](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) which standardizes features by removing the mean and scaling to unit variance. Additionally, I also removed the 0.5% of data to avoid outliers that would affect the StandardScaler. 

**Performance:**
```
R^2 on the training set: 0.00041501462485626917
R^2 on the test set: 0.0002275448014414927

RMSE on the training set: 0.49504300848369526
RMSE on the test set: 0.5004967098236794
```

*My R^2 and RMSE weren't particularly impressive, but for a linear regression model with two features this is a good starting point.*

After doing some reasearch I found that while my R^2 score is pretty terrible, the RMSE isn't too bad. Keeping in mind the application of this model, we can tolerare slightly more error than mission-critical scenarios involving models such as self-driving cars. 

---
## Final Model

In my final model I wanted to learn from the baseline and make tweaks to improve its performance. First I switched the `StandardScaler()` for a `QuantileTransformer()`. This is because QuantileTransformer is better suited to handle outliers. In fact, "they are robust to outliers in the sense that adding or removing outliers in the training set will yield approximately the same transformation."

Next, I changed my regression model to a `RandomForestRegressor()`. This regression is non-linear and I hoped that a tree would better fit all of the features I was about to add. The advantage of a Random Forest over a Normal Decision tree is that it avoids overfitting, and also normalizes the data we pass it through the use of sampling random features and rows from the dataset. 

I decided to add all of the nutritional information in the hopes that the regressor would be able to identify some sort of trend where certain amounts of sugar & carbohydrates or something similar would be an indicator of how "good" the food tastes (and therfore a higher average review). All of the nutrition features that I added are quantitative continuous. 

<br>


**Tuning Hyperparameters**

For the final model I also used `GridSearchCV()` to try different combinations of hyperparameters in order to find out what combination led to the lowest loss. Because I was training on my laptop, I found that setting `n_jobs=-1` in gridsearch significantly speed up the procsess of performinf the gridsearch as it allowed me to dedicate all of the cores on my processor to this task. 

These are the best hyperparameters that my gridsearch returned after 15 mins:
```
{'max_depth': 7, 'max_features': 3, 'n_estimators': 400}
```

*Now to plug them back into my RandomForestRegressor and evaluate my model!*

<br>

**Results**

```
R^2 on the training set: 0.005681727709422213
R^2 on the test set: 0.004666632804694593

RMSE on the training set: 0.4937371200700354
RMSE on the test set: 0.49938434639791457
```

That's an RSME decrease of 0.00111236342 over the test set and 0.00130588841 over the training set, as well as a R^2 increase of 0.004439088 over the test set and 0.00526671308 over the training.

While these results weren't what I was hoping for, they show across the board improvements from our baseline model for both tests. Based on my knowledge of the dataset, I belive that this shows that there is simply not a very strong correlation between nutritional information and average rating in this dataset.


---
## Fairness Analysis
I decided to group my data into when the recipe was made. This dataset contains the years 2008-2018, so I split my data into two groups: 2008-2013 and 2014-2018.

<br>

**Null Hypothesis:** Our model is fair. Its precision for old recipes and new recipes are roughly the same,and any differences are due to random chance.

**Alternative Hypothesis:** Our model is unfair. Its precision for new recipes is lower than its precision for old recipes.


In order to test my hypothesis I will calculate my observed RMSE on the new recipes, permute the posting year, then calculate the porportion of values that are above the observed RMSE. My significance level to reject the null hypothesis is 5%.

`P Value: 1.0` - thus we fail to reject the null hypothesis!1