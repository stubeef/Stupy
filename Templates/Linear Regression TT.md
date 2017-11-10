# Test and Train Split

```
# from sklearn.cross_validation import train_test_split # deprecated syntax
from sklearn.model_selection import train_test_split
```

# define a function that accepts a list of features and returns testing RMSE
def train_test_rmse(feature_cols):
    X = bikes[feature_cols]
    y = bikes.total
    X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=123)
    linreg = LinearRegression()
    linreg.fit(X_train, y_train)
    y_pred = linreg.predict(X_test)
    return np.sqrt(metrics.mean_squared_error(y_test, y_pred))
    
    train_test_split(X, y, random_state=123)
    
    # compare different sets of features
    print train_test_rmse(['temp', 'season', 'weather', 'humidity'])
    print train_test_rmse(['temp', 'season', 'weather'])
    print train_test_rmse(['temp', 'season', 'humidity'])
    print train_test_rmse(['temp', 'humidity'])
    
    # split X and y into training and testing sets
    X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=123)

# create a NumPy array with the same shape as y_test
y_null = np.zeros_like(y_test, dtype=float)

# fill the array with the mean value of y_test
y_null.fill(y_test.mean())
y_null

# compute null RMSE
np.sqrt(metrics.mean_squared_error(y_test, y_null))
    
    ```
