### Logistic Regression

```
# glass identification dataset
import pandas as pd
url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/glass/glass.data'
col_names = ['id','ri','na','mg','al','si','k','ca','ba','fe','glass_type']
glass = pd.read_csv(url, names=col_names, index_col='id')
glass.sort_values(by='al', inplace=True)
glass.head() 

glass.glass_type.value_counts(normalize=1)

# examine the number of observations by glass_type
glass.glass_type.value_counts().sort_index()

# types 1, 2, 3 are window glass
# types 5, 6, 7 are household glass
#numpy can only take numbers
glass['household'] = glass.glass_type.map({1:0, 2:0, 3:0, 5:1, 6:1, 7:1})
glass.head()
```
# understanding np.where and transorm variables to 1 or 0
```
import numpy as np
nums = np.array([5, 15, 8])

# np.where returns the first value if the condition is True, and the second value if the condition is False
np.where(nums > 10, 'big', 'small')

# transform household_pred to 1 or 0
glass['household_pred_class'] = np.where(glass.household_pred >= 0.5, 1, 0)
glass.tail()
```
# Logistic Regressio with a continuous variable
```
# fit a logistic regression model and store the class predictions
feature_cols = ['al']
X = glass[feature_cols]
y = glass.household

from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression()

logreg.fit(X, y)
glass['household_pred_class'] = logreg.predict(X)
# plot the class predictions
plt.scatter(glass.al, glass.household)
plt.plot(glass.al, glass.household_pred_class, color='red')
plt.xlabel('al')
plt.ylabel('household')

logreg.predict_proba(X)

# store the predicted probabilites of class 1
glass['household_pred_prob'] = logreg.predict_proba(X)[:, 1]
glass.head()

# plot the predicted probabilities
plt.scatter(glass.al, glass.household)
plt.plot(glass.al, glass.household_pred_prob, color='red')
plt.xlabel('al')
plt.ylabel('household_pred_prob')

# examine some example predictions
print logreg.predict(1)
print logreg.predict(2)
print logreg.predict(3)

# compute predicted probability for al=2 using the predict_proba method
logreg.predict_proba(2.0)
```
## Use a logistic regression with a categorical variable

```
# create a categorical feature
glass['high_al'] = np.where(glass.al > 2, 1, 0)

# fit a logistic regression model
feature_cols = ['high_al']
X = glass[feature_cols]
y = glass.household
logreg.fit(X, y)

logreg.predict(1)


logreg.intercept_
# examine the coefficient for al
zip(feature_cols, logreg.coef_[0])
```
