### Loop Example using CV

```
Example of a loop
# list of values to try
max_depth_range = range(1, 8)

# list to store the average RMSE for each value of max_depth
RMSE_scores = []

# use CV with each value of max_depth
for depth in max_depth_range:
    treereg = DecisionTreeRegressor(max_depth=depth, random_state=1)
    MSE_scores = cross_val_score(treereg, X, y, cv=14, scoring='neg_mean_squared_error')
    RMSE_scores.append(np.mean(np.sqrt(-MSE_scores)))
```
1. Create range of values to try (max_depth_range)
2. Create empty list to store results in
3. For loop, `depth` is a dummy variable, it could be anything. 
4. logic runs sequentially inside the loop
