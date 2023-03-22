## Framing the Problem:

This dataset was obtained and cleaned previously in my [Recipe-Analysis](https://github.com/415matt/Recipe-Analysis) repo. There I discovered and visualized some interesting relations between features in the dataset. 

In this project, I will create a regression model to predict the average rating of a recipe given its features. I thought that this would be an interesting project, as I have always wondered if there is some kind of "golden ratio" of ingredients and cook time that will produce some sort of predictable trend. 

 I will be using RMSE and R^2 to evaluate my error on both my training and test set. I specifically chose RMSE as it takes into account the affect of large errors, something I want to avoid in my model. Additionally I am also looking at the R^2 error as it is easily accessible through the `sklearn` library and adds an additional metric for my model's performance.

 My model will be trained only on features that can be known during the time of prediction (before the food is tasted). Each model I trained has different features and processes - keep reading to learn more. 
 
---
## Baseline Model

I decided to start simple with baseline model, using only two features: `n_steps` and `calories (#)`, both of which are quantitative descreet features. For this model I used linear regression with my data being preprocessed through the [StandardScaler()](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) which standardizes features by removing the mean and scaling to unit variance. Additionally, I also removed the 0.5% of data to avoid outliers that would affect the StandardScaler. 

**Performace:**
```
R^2 on the training set: 0.00041501462485626917
R^2 on the test set: 0.0002275448014414927

RMSE on the training set: 0.49504300848369526
RMSE on the test set: 0.5004967098236794
```


---
## Final Model
---
## Fairness Analysis
