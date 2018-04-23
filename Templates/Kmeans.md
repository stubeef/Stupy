## Import Classifier

```
from sklearn.cluster import KMeans
# make an instance of a KNeighborsClassifier object
kmeans = KMeans(n_clusters=2, random_state=0).fit(X_d)

# center and scale the data
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_d_scaled = scaler.fit_transform(X_d)
```
## Run Model
```
# K-means with 5 clusters on scaled data
kmeans_d = KMeans(n_clusters=2, random_state=0).fit(X_d_scaled)

# Print Labels
kmeans_d.labels_

#See Cluster Centers
kmeans_d.cluster_centers_
dfd['cluster']=kmeans_d.labels_
# save the cluster labels and sort by cluster
dfd['cluster'] = kmeans_d.labels_
dfd.sort_values('cluster')
```

## Review Centers
```
# review the cluster centers
dfd.groupby('cluster').mean()

dcenters = dfd.groupby('cluster').mean()
```
## Visualize 
```
%matplotlib inline
import matplotlib.pyplot as plt
plt.rcParams['font.size'] = 14
pd.plotting.scatter_matrix(X_d, c=dfd.cluster, figsize=(20,20), s=250)
```
## Scatter Plot Visualization
```
# scatter plot of calories versus alcohol, colored by cluster (0=red, 1=green, 2=blue)
plt.scatter(dfd.square_diff_log, dfd.square_diff_items_log, c=dfd.cluster, s=50)

# cluster centers, marked by "+"
plt.scatter(dcenters.square_diff_log, dcenters.square_diff_items_log, linewidths=3, marker='+', s=300, c='black')

# add labels
plt.xlabel('demand')
plt.ylabel('items')
```
## Cluster Evaluation and Silhousette Coefficient
```
# calculate Silhouette Coefficient for K=5
from sklearn import metrics
metrics.silhouette_score(X_d_scaled, kmeans_d.labels_)

# calculate SC for K=2 through K=19
k_range = range(2, 20)
scores = []
for k in k_range:
    kmeans = KMeans(n_clusters=k, random_state=1)
    kmeans.fit(X_d_scaled)
    scores.append(metrics.silhouette_score(X_d_scaled, kmeans.labels_))

# plot the results
plt.plot(k_range, scores)
plt.xlabel('Number of clusters')
plt.ylabel('Silhouette Coefficient')
plt.grid(True)
```
