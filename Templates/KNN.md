# KNN Model
### Create feature matrix
```
# all features and response need to be numbers for scikit-learn
# map each iris species to a number
iris['species_num'] = iris.species.map({'Iris-setosa':0, 'Iris-versicolor':1, 'Iris-virginica':2})

# store feature matrix in "X"
feature_cols = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
X = iris[feature_cols]
print (X)

# store response vector in "y"
y = iris.species_num
print(y)
```
### Import Model
```
from sklearn.neighbors import KNeighborsClassifier
# make an instance of a KNeighborsClassifier object
knn = KNeighborsClassifier(n_neighbors=1)
```
### Fit the model
```knn.fit(X, y)```
### Predict
```
# Since we used all our data to create the model, let's create a hypothetical new observation to see the prediction
new_observation = [[3, 5, 4, 2]]
knn.predict(new_observation)
# Since we used all our data to create the model, let's create a hypothetical new observation to see the prediction
new_observation = [[3, 5, 4, 2],[4,1, 5,7]]
knn.predict(new_observation)
X_new = [[3, 5, 4, 2], [5, 4, 3, 2]]
knn.predict(X_new)
```

# Tuning a KNN model
```
# instantiate the model (using the value K=5)
knn = KNeighborsClassifier(n_neighbors=5)

# fit the model with data
knn.fit(X, y)

# predict the response for new observations
knn.predict(X_new)

# calculate predicted probabilities of class membership
knn.predict_proba(X_new)
```
