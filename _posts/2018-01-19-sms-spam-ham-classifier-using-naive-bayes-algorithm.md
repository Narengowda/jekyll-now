---
title: SMS Spam/Ham classifier using Naive Bayes algorithm
undefined: In this article lets predict a given SMS is SPAM or HAM based on the probability
  of presence of certain words which were part of SPAM messages.
created_date: 2018-01-16 18:30:00 +0000
date: 2018-01-19 17:37:33 +0000
---
Conditional probability is the probability that something will happen, **_given that something else_ has already occurred**. Using the conditional probability, we can calculate the probability of an event using its prior knowledge.

Below is the formula for calculating the conditional probability.[![Bayes](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/Bayes_rule-300x172-300x172.png =300x172)](https://www.analyticsvidhya.com/wp-content/uploads/2015/09/Bayes_rule-300x172.png)

**where**

* P(H) is the probability of hypothesis H being true. This is known as the prior probability.
* P(E) is the probability of the evidence(regardless of the hypothesis).
* P(E|H) is the probability of the evidence given that hypothesis is true.
* P(H|E) is the probability of the hypothesis given that the evidence is there.

Naive Bayes classifier gives great results when we use it for textual data analysis. Such as Natural Language Processing.

In this article lets predict a given SMS is SPAM or HAM based on the probability of presence of certain words which were part of SPAM messages.

Lets code

```python
import pandas as pd    
data = pd.read_csv('./spam_sms.csv', encoding = "cp1252")
data = data[['v1', 'v2']]
data.head()
```

|  | v1 | v2 |
| 0 | ham | Go until jurong point, crazy.. Available only ... |
| 1 | ham | Ok lar... Joking wif u oni... |
| 2 | spam | Free entry in 2 a wkly comp to win FA Cup fina... |
| 3 | ham | U dun say so early hor... U c already then say... |
| 4 | ham | Nah I don't think he goes to usf, he lives aro... |

```python
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
count_vect = CountVectorizer()

x_train_counts = count_vect.fit_transform(data['v2'])

# Check frequesncy of a word .astype
print count_vect.vocabulary_.get('hello')
>>> 3814

classifier = MultinomialNB()
classifier.fit(x_train_counts, data['v1'])

MultinomialNB(alpha=1.0, class_prior=None, fit_prior=True)
print classifier.predict(count_vect.transform(["Hey how are you"]))[0]
>>>ham

print classifier.predict(count_vect.transform(["free credit card for you only"]))[0]
>>> spam

print classifier.predict(count_vect.transform(["We are super excited to deliver happines to you. To enjoy seamless shopping experience download our app. We have a welcome gift for you. "]))[0]
>>> spam
```

### How ever this one fails !!

The trick is to deceive, use cred!t - credit 0nly - only 0ffer - offer st0res = stores

```python
print classifier.predict(count_vect.transform(["cred!t card f0r you 0nly, great disc0unts. 0ffer on selected st0res"])
>>> ham
```

SMS data from: [https://www.kaggle.com/uciml/sms-spam-collection-dataset](https://www.kaggle.com/uciml/sms-spam-collection-dataset "https://www.kaggle.com/uciml/sms-spam-collection-dataset")

references:

[http://dataaspirant.com/2017/02/06/naive-bayes-classifier-machine-learning/](http://dataaspirant.com/2017/02/06/naive-bayes-classifier-machine-learning/ "http://dataaspirant.com/2017/02/06/naive-bayes-classifier-machine-learning/")

[https://www.analyticsvidhya.com/blog/2017/09/naive-bayes-explained/](https://www.analyticsvidhya.com/blog/2017/09/naive-bayes-explained/ "https://www.analyticsvidhya.com/blog/2017/09/naive-bayes-explained/")