
```# simulate splitting a dataset of 25 observations into 5 folds
from sklearn.cross_validation import KFold
kf = KFold(25, n_folds=5, shuffle=False)

# print the contents of each training and testing set
print '{} {:^61} {}'.format('Iteration', 'Training set observations', 'Testing set observations')
for iteration, data in enumerate(kf, start=1):
    print '{:^9} {} {:^25}'.format(iteration, data[0], data[1])
## Syntax error    
```
### Cross-validation example: parameter tuning
Goal: Select the best tuning parameters (aka "hyperparameters") for KNN on the iris dataset
```
from sklearn.cross_validation import cross_val_score  
# 10-fold cross-validation with K=5 for KNN (the n_neighbors parameter)
knn = KNeighborsClassifier(n_neighbors=5)
scores = cross_val_score(knn, X, y, cv=10, scoring='accuracy')
print (scores)
# use average accuracy as an estimate of out-of-sample accuracy
print (scores.mean())
# search for an optimal value of K for KNN
k_range = range(1, 31)
k_scores = []
for k in k_range:
    knn = KNeighborsClassifier(n_neighbors=k)
    scores = cross_val_score(knn, X, y, cv=10, scoring='accuracy')
    k_scores.append(scores.mean())
print (k_scores)
```
Plot the value of K for KNN (x-axis) versus the cross-validated accuracy (y-axis)
```
%matplotlib inline
import matplotlib.pyplot as plt

# plot the value of K for KNN (x-axis) versus the cross-validated accuracy (y-axis)
plt.plot(k_range, k_scores)
plt.xlabel('Value of K for KNN')
plt.ylabel('Cross-Validated Accuracy')
```
### Cross-validation example: model selection
```
# 10-fold cross-validation with the best KNN model
knn = KNeighborsClassifier(n_neighbors=20)
print cross_val_score(knn, X, y, cv=10, scoring='accuracy').mean()

# 10-fold cross-validation with logistic regression
from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression()
print cross_val_score(logreg, X, y, cv=10, scoring='accuracy').mean()
logreg
```
### Cross-validation example: feature selection
```
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression

# read in the advertising dataset
data = pd.read_csv('http://www-bcf.usc.edu/~gareth/ISL/Advertising.csv', index_col=0)
print (data.head())

# create a Python list of three feature names
feature_cols = ['TV', 'radio', 'newspaper']

# use the list to select a subset of the DataFrame (X)
X = data[feature_cols]

# select the Sales column as the response (y)
y = data.sales

# 10-fold cross-validation with all three features
lm = LinearRegression()
mse_scores = cross_val_score(lm, X, y, cv=10, scoring='neg_mean_squared_error')
print (mse_scores)
# Fix the negative signs
mse_scores = -mse_scores
print (mse_scores)
# convert from MSE to RMSE
rmse_scores = np.sqrt(mse_scores)
print (rmse_scores)

# calculate the average RMSE
print (rmse_scores.mean())

# 10-fold cross-validation with two features (excluding Newspaper)
feature_cols = ['TV', 'Radio']
X = data[feature_cols]
print np.sqrt(-cross_val_score(lm, X, y, cv=10, scoring='mean_squared_error')).mean()
```

Resources
[scikit-learn documentation:](Cross-validation),(Model evaluation)
scikit-learn issue on GitHub: MSE is negative when returned by cross_val_score
Section 5.1 of An Introduction to Statistical Learning (11 pages) and related videos: K-fold and leave-one-out cross-validation (14 minutes), Cross-validation the right and wrong ways (10 minutes)
Scott Fortmann-Roe: Accurately Measuring Model Prediction Error
Machine Learning Mastery: An Introduction to Feature Selection
Harvard CS109: Cross-Validation: The Right and Wrong Way
Journal of Cheminformatics: Cross-validation pitfalls when selecting and assessing regression and classification models


