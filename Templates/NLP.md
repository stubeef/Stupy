### Tokenization
What: Separate text into units such as sentences or words
Why: Gives structure to previously unstructured text
Notes: Relatively easy with English language text, not easy with some languages
``` # use CountVectorizer to create document-term matrices from X_train and X_test
      from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer
      vect = CountVectorizer()
      X_train_dtm = vect.fit_transform(X_train)
      X_test_dtm = vect.transform(X_test)
      
      # rows are documents, columns are terms (aka "tokens" or "features")
      X_train_dtm
      
      # show vectorizer options
      vect
      
      # We will not convert to lowercase this time
vect = CountVectorizer(lowercase=False)
X_train_dtm = vect.fit_transform(X_train)
X_train_dtm.shape

# include 1-grams and 2-grams
vect = CountVectorizer(ngram_range=(1, 2))
X_train_dtm = vect.fit_transform(X_train)
X_train_dtm.shape
```
# Predicting the star rating: 
```
  # use default options for CountVectorizer
vect = CountVectorizer()

# create document-term matrices
X_train_dtm = vect.fit_transform(X_train)
X_test_dtm = vect.transform(X_test)

# use Naive Bayes to predict the star rating
nb = MultinomialNB()
nb.fit(X_train_dtm, y_train)
y_pred_class = nb.predict(X_test_dtm)

# calculate accuracy
print metrics.accuracy_score(y_test, y_pred_class)


# calculate null accuracy
y_test_category = pd.DataFrame(np.where(y_test==5, 'best', 'worst'), columns = ['rating'])
y_test_category.rating.value_counts() / len(y_test_category.rating)


# alternate way to calculate null accuracy
y_test_binary = np.where(y_test==5, 1, 0)
max(y_test_binary.mean(), 1 - y_test_binary.mean())


# define a function that accepts a vectorizer object and calculates the accuracy
def tokenize_test(vect):
    X_train_dtm = vect.fit_transform(X_train)
    print 'Features: ', X_train_dtm.shape[1]
    X_test_dtm = vect.transform(X_test)
    nb = MultinomialNB()
    nb.fit(X_train_dtm, y_train)
    y_pred_class = nb.predict(X_test_dtm)
    print 'Accuracy: ', metrics.accuracy_score(y_test, y_pred_class)
    
    
    vect = CountVectorizer()
tokenize_test(vect)


# include 1-grams and 2-grams
vect = CountVectorizer(ngram_range=(1,3))
tokenize_test(vect)
```
### Stopword Removal
What: Remove common words that will likely appear in any text
Why: They don't tell you much about your text
stop_words: string {'english'}, list, or None (default)
If 'english', a built-in stop word list for English is used.
If a list, that list is assumed to contain stop words, all of which will be removed from the resulting tokens.
If None, no stop words will be used. max_df can be set to a value in the range [0.7, 1.0) to automatically detect and filter stop words based on intra corpus document frequency of terms.
```
# show vectorizer options
vect

# remove English stop words
vect = CountVectorizer(stop_words='english')
tokenize_test(vect)


# set of stop words
print vect.get_stop_words()
```
### Other CountVectorizer Options
max_features: int or None, default=None
If not None, build a vocabulary that only consider the top max_features ordered by term frequency across the corpus.

      
