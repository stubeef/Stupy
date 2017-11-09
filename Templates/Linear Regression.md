### Linear Regression

# create X and y
```
# create a list of features
feature_cols = ['temp', 'season', 'weather', 'humidity']

# create X and y
# We have created our X (feature matrix) and Y (response vector so now lets:
# import our chosen estimator, instantiate it into a variable, fit the model with the X and y
X = bikes[feature_cols]
y = bikes.total

# instantiate and fit
linreg = LinearRegression()
linreg.fit(X, y)

# print the coefficients
print linreg.intercept_
print linreg.coef_

# pair the feature names with the coefficients
zip(feature_cols, linreg.coef_)
```

# Using the model to form a prediction

```
# manually calculate the prediction
linreg.intercept_ + linreg.coef_*25

# use the predict method
linreg.predict(25)
```

# Error Terms
```
# example true and predicted response values
true = [10, 7, 5, 5]
pred = [8, 6, 5, 10]

# calculate these metrics by hand!
from sklearn import metrics
import numpy as np
print 'MAE:', metrics.mean_absolute_error(true, pred)
print 'MSE:', metrics.mean_squared_error(true, pred)
print 'RMSE:', np.sqrt(metrics.mean_squared_error(true, pred))
```
