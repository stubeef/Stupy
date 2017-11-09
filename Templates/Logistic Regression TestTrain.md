 # Create X and Y
 ```
feature_cols = ['Pclass','Parch']
X = titanic[feature_cols]
y = titanic.Survived
```
# Split into test and Train sets
```
from sklearn.cross_validation import train_test_split
X = titanic[feature_cols]
y = titanic.Survived
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=123)
```
# Import model
```
from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression()
```
# Fit the model and examine the coefficients
```
logreg.fit(X_train,y_train)
logreg.coef_
```
# Make predictions on the testing set
```
# class predictions
y_pred_class = logreg.predict_proba(X_test)
```
# Calculate the accuracy using the testing set predictions
```
# calculate classification accuracy
from sklearn import metrics
print (metrics.accuracy_score(y_test, y_pred_class))
```
# Compare testing accuracy to null accuracy
```
# this works regardless of the number of classes
y_test.value_counts().head(1) / len(y_test)
```
