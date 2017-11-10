## Import Train_Test_Split
```
#from sklearn.cross_validation import train_test_split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y)
```

### STEP 1: split X and y into training and testing sets (using random_state for reproducibility) (K= 1)
`X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=99)`

### STEP 2: train the model on the training set (using K=1)
```
   knn = KNeighborsClassifier(n_neighbors=1)
   knn.fit(X_train, y_train)
   ```

### STEP 3: test the model on the testing set, and check the accuracy
   ```
   y_pred_class = knn.predict(X_test)
   print metrics.accuracy_score(y_test, y_pred_class)
   ```

### Repeating for K=50
```knn = KNeighborsClassifier(n_neighbors=50)
knn.fit(X_train, y_train)
y_pred_class = knn.predict(X_test)
print metrics.accuracy_score(y_test, y_pred_class)
# Comparing testing accuracy to null accuracy
```
### examine the class distribution
`y_test.value_counts()`

### compute null accuracy
```y_test.value_counts().head(1) / len(y_test)```

### Searching the 'best' value of K
```
# calculate TRAINING Accuracy and TESTING accuracy for K=1 through 100

k_range = range(1, 101)
training_error_rate = []
testing_error_rate = []

for k in k_range:

    # instantiate the model with the current K value
    knn = KNeighborsClassifier(n_neighbors=k)

    # calculate training error
    knn.fit(X, y)
    y_pred_class = knn.predict(X)
    training_accuracy = metrics.accuracy_score(y, y_pred_class)
    training_error_rate.append(1 - training_accuracy)
    
    # calculate testing error
    knn.fit(X_train, y_train)
    y_pred_class = knn.predict(X_test)
    testing_accuracy = metrics.accuracy_score(y_test, y_pred_class)
    testing_error_rate.append(1 - testing_accuracy)
    
    
    
   # allow plots to appear in the notebook
   %matplotlib inline
   import matplotlib.pyplot as plt
   plt.style.use('fivethirtyeight')

   # create a DataFrame of K, training accuracy, and testing acc
   column_dict = {'K': k_range, 'training error rate':training_error_rate, 'testing error rate':testing_error_rate}
   df = pd.DataFrame(column_dict).set_index('K').sort_index(ascending=True)
   df.head()

   # plot the relationship between K (HIGH TO LOW) and TESTING Accuracy
   df.plot(y='testing error rate')
   plt.xlabel('Value of K for KNN')
   plt.ylabel('Error rate (lower is better)')

   # find the minimum testing error and the associated K value
   df.sort_values(by='testing error rate').head()

   # alternative method
   min(zip(testing_error_rate, k_range)) 
```
### Training error versus testing error
```
# plot the relationship between K (HIGH TO LOW) and both TRAINING ERROR and TESTING ERROR
df.plot()
plt.xlabel('Value of K for KNN')
plt.ylabel('Error rate (lower is better)')
```
### Making predicitons on out-of-sample data
```
# instantiate the model with the best known parameters
knn = KNeighborsClassifier(n_neighbors=14)

# re-train the model with X and y (not X_train and y_train) - why?
knn.fit(X, y)

# make a prediction for an out-of-sample observation
knn.predict([[1, 1, 0, 1, 2]])
```
