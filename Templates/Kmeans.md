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
