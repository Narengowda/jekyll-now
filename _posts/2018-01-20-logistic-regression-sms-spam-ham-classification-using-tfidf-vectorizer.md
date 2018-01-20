---
title: Logistic regression - SMS SPAM/HAM classification using TFIDF vectorizer
undefined: Our aim is to classify SMSes in to SPAM or HAM messages using logistic
  regression and TFIDF vectorize
created_date: 2018-01-20 00:00:00 +0530
date: 2018-01-20 11:59:41 +0000
---
Our aim is to classify SMSes in to SPAM or HAM messages using logistic regression and TFIDF vectorizer.

steps:

1. Read data from spam_sms.csv
2. SMS text processing by using TERM FREQUENCY and INVERSE DOCUMENT FREQUENCY
3. Train the model using Logistic regression
4. Predict on test data and calculate accuracy using estimator fro logistic regression

#### Word Frequencies with TfidfVectorizer

Word counts are a good starting point, but are very basic.

One issue with simple counts is that some words like “the” will  appear many times and their large counts will not be very meaningful in  the encoded vectors.

An alternative is to calculate word frequencies, and by far the most  popular method is called TF-IDF. This is an acronym than stands for  “Term Frequency – Inverse Document” Frequency which are the components  of the resulting scores assigned to each word.

    Term Frequency: This summarizes how often a given word appears within a document.
    Inverse Document Frequency: This downscales words that appear a lot across documents.

Without going into the math, TF-IDF are word frequency scores that  try to highlight words that are more interesting, e.g. frequent in a  document but not across documents.

The TfidfVectorizer will tokenize documents, learn the vocabulary and  inverse document frequency weightings, and allow you to encode new  documents. Alternately, if you already have a learned CountVectorizer,  you can use it with a TfidfTransformer to just calculate the inverse  document frequencies and start encoding documents.

The same create, fit, and transform process is used as with the CountVectorizer.

Below is an example of using the TfidfVectorizer to learn vocabulary  and inverse document frequencies across 3 small documents and then  encode one of those documents.

#### Watch this [https://youtu.be/FX41g2OVXgE?t=1m59s](https://youtu.be/FX41g2OVXgE?t=1m59s "https://youtu.be/FX41g2OVXgE?t=1m59s") to understand TFIDF

```python
import pandas as pd
import numpy as np
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model.logistic import LogisticRegression
from sklearn.model_selection import train_test_split, cross_val_score

data = pd.read_csv('./spam_sms.csv', encoding = "cp1252", header=None)
```

Random split into training and test sets can be quickly computed with the train_test_split helper function

```python
train_x, test_x, train_y, test_y = train_test_split(data[1], data[0])
```

Convert list of string to TFIDF format as explained above

```python
vectorizer = TfidfVectorizer(stop_words='english')
tfidf_train_x = vectorizer.fit_transform(train_x)
```

Now perform logistic regression on vectorized data

```python
classifier = LogisticRegression()
classifier.fit(tfidf_train_x, train_y)
```

Learning the parameters of a prediction function and testing it on  the same data is a methodological mistake: a model that would just  repeat the labels of the samples that it has just seen would have a  perfect score but would fail to predict anything useful on yet-unseen  data. This situation is called overfitting. To avoid it, it is common  practice when performing a (supervised) machine learning experiment to  hold out part of the available data as a test set

```python
tfidf_test_x = vectorizer.transform(test_x)
print tfidf_test_x.shape
scores = cross_val_score(classifier, tfidf_test_x, test_y, cv=5)
acc = scores.mean()
print "Accuracy: %0.2f percent" % (acc *100)
```

    (1394, 7170)
    Accuracy: 90.24 percent

Lets test our classifier with some sample messages.

```python
mess = ['you are a WINNER!! To claim 1000$ call 99999999', "I am ready to invest in dating service"]
output = classifier.predict(vectorizer.transform(mess))

for i ,m in enumerate(mess):
	print m, ' == ', output[i]
```

Output:

you are a WINNER!! To claim 1000$ call 99999999  ==  spam


I am ready to invest in dating service  ==  ham

SMS data from: [https://www.kaggle.com/uciml/sms-spam-collection-dataset](https://www.kaggle.com/uciml/sms-spam-collection-dataset "https://www.kaggle.com/uciml/sms-spam-collection-dataset")

Reference:  [https://youtu.be/FX41g2OVXgE?t=1m59s](https://youtu.be/FX41g2OVXgE?t=1m59s "https://youtu.be/FX41g2OVXgE?t=1m59s")