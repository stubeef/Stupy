## Data Preparation and Advanced Model Evaluation

###  Handling missing values
Scikit learn can't handle missing values, as all values are numeric and hold meaning. 
```
# check for missing values
print titanic.isnull()
print titanic.isnull().sum()
```
Drop missing values
```
# drop rows with any missing values
df.dropna().shape
# Drop nulls
df.dropna()
# drop rows where Age is missing
df[df.Age.notnull()].shape
# Where age is not null
df.Age.notnull()
```
Other strategy is to impute missing values
```
# mean Age
titanic.Age.mean()
# median Age
titanic.Age.median()
# most frequent Age
titanic.Age.mode()

# fill missing values for Age with the median age
titanic.Age.fillna(titanic.Age.median(), inplace=True)
titanic.shape
```
### Handling categorical features
```
# Create and encode  feature
df['Female'] = df.Sex.map({'male':0, 'female':1})

# create a DataFrame of dummy variables for Embarked
embarked_dummies = pd.get_dummies(df.Embarked, prefix='Embarked')
#embarked_dummies.drop(embarked_dummies.columns[0], axis=1, inplace=True)

# concatenate the original DataFrame and the dummy DataFrame
# Results in new columns being added to the dataframe. Wider table versus deeper table, new column for each distinct value in 'Emabrked'
df = pd.concat([df, embarked_dummies], axis=1)
```
### Model
```
# define X and y
feature_cols = ['Pclass', 'Parch', 'Age', 'Female', 'Embarked_Q', 'Embarked_S']
X = titanic[feature_cols]
y = titanic.Survived

# train/test split
from sklearn.cross_validation import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=1)

# train a logistic regression model
from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression()
logreg.fit(X_train, y_train)

# make predictions for testing set
y_pred_class = logreg.predict(X_test)

# calculate testing accuracy
#from sklearn import metrics
from sklearn import metrics
print metrics.accuracy_score(y_test, y_pred_class)
```
### ROC curves and AUC
```
# predict probability of survival
y_pred_prob = logreg.predict_proba(X_test)[:, 1]
print(y_pred_prob)
```
### Plot ROC curve
```
%matplotlib inline
import matplotlib.pyplot as plt
plt.rcParams['figure.figsize'] = (8, 6)
plt.rcParams['font.size'] = 14

# plot ROC curve
fpr, tpr, thresholds = metrics.roc_curve(y_test, y_pred_prob)
plt.plot(fpr, tpr)
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.0])
plt.xlabel('False Positive Rate (1 - Specificity)')
plt.ylabel('True Positive Rate (Sensitivity)')
#print(metrics.roc_curve(y_test, y_pred_prob))
```
### Calculate AUC
```
# calculate AUC
print metrics.roc_auc_score(y_test, y_pred_prob)
```
### Plotting AUC and ROC
Don't use y_pred_class, use y_pred_prob. Why? If y_pred_class is used, it will interpret the zeroes and ones and predicted probalities of 0% and 100%
```
# histogram of predicted probabilities grouped by actual response value
df = pd.DataFrame({'probability':y_pred_prob, 'actual':y_test})
df.hist(column='probability', by='actual', sharex=True, sharey=True)
```
