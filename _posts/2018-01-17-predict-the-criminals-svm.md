---
title: Predict the Criminals - SVM
undefined: Description
created_date: 2018-01-17 00:00:00 +0530
date: 2018-01-17 23:10:46 +0000
---
There has been a surge in crimes committed in recent years, making crime a top cause of concern for law enforcement. If we are able to estimate whether someone is going to commit a crime in the future, we can take precautions and be prepared. 

You are given a dataset containing answers to various questions concerning the professional and private lives of several people. A few of them have been arrested for various small and large crimes in the past. Use the given data to predict if the people in the test data will commit a crime. The train data consists of 45718 rows, while the test data consists of 11430 rows.

The evaluation metric is precision score.

### [Download Dataset](https://he-s3.s3.amazonaws.com/media/hackathon/predict-the-criminal/predict-the-criminal/d17428d0-e-Criminal.rar)

Here is how this problem can be solved using SVM

    %matplotlib inline
    import pandas as pd
    import matplotlib.pyplot as plt
    df = pd.read_csv('./e_criminal/criminal_train.csv')
    df.head()

|  | PERID | IFATHER | NRCH17_2 | IRHHSIZ2 | IIHHSIZ2 | IRKI17_2 | IIKI17_2 | IRHH65_2 | IIHH65_2 | PRXRETRY | ... | TOOLONG | TROUBUND | PDEN10 | COUTYP2 | MAIIN102 | AIIND102 | ANALWT_C | VESTR | VEREP | Criminal |
| 0 | 25095143 | 4 | 2 | 4 | 1 | 3 | 1 | 1 | 1 | 99 | ... | 1 | 2 | 1 | 1 | 2 | 2 | 3884.805998 | 40026 | 1 | 0 |
| 1 | 13005143 | 4 | 1 | 3 | 1 | 2 | 1 | 1 | 1 | 99 | ... | 2 | 2 | 2 | 3 | 2 | 2 | 1627.108106 | 40015 | 2 | 1 |
| 2 | 67415143 | 4 | 1 | 2 | 1 | 2 | 1 | 1 | 1 | 99 | ... | 2 | 2 | 2 | 3 | 2 | 2 | 4344.957980 | 40024 | 1 | 0 |
| 3 | 70925143 | 4 | 0 | 2 | 1 | 1 | 1 | 1 | 1 | 99 | ... | 2 | 2 | 1 | 1 | 2 | 2 | 792.521931 | 40027 | 1 | 0 |
| 4 | 75235143 | 1 | 0 | 6 | 1 | 4 | 1 | 1 | 1 | 99 | ... | 2 | 2 | 2 | 2 | 2 | 2 | 1518.118526 | 40001 | 2 | 0 |

5 rows × 72 columns

    import numpy as np
    X = df.iloc[: ,:-1]
    Y = df.iloc[: , X.shape[1]:]
    Y = np.ravel(Y)
    Y.shape
    
    
    (45718,)
    
    # Feature Selection
    
    # Recursive Feature Elimination (RFE) is based on the idea to repeatedly
    # construct a model and choose either the best or worst performing feature,
    # setting the feature aside and then repeating the process with the rest of the features
    
    from sklearn import datasets
    from sklearn.feature_selection import RFE
    from sklearn.linear_model import LogisticRegression
    
    logreg = LogisticRegression()
    
    rfe = RFE(logreg, 18)
    rfe = rfe.fit(X, Y)
    print(rfe.support_)
    print(rfe.ranking_)
    
    
    [False  True False  True False  True False False False False False False
     False False False False  True False False False False False False False
     False False False False  True False  True False  True False False False
     False False False False False  True False False  True False False False
      True False  True False  True False False  True  True  True False False
      True  True False False False False False  True False False False]
    [28  1 35  1 38  1 37 51 36 21 13 53 18 54  2  3  1 11 20 15  5 24 23 25 16
     17 33 48  1 44  1 41  1 50  6  4  9 12 14 34 49  1 32 52  1 40 19 47  1 39
      1 46  1 45 22  1  1  1 43 42  1  1  8  7 31 30 10  1 27 26 29]
    
    
    # Normalize the data
    from sklearn import preprocessing
    x = df.values #returns a numpy array
    
    min_max_scaler = preprocessing.MinMaxScaler()
    x_scaled = min_max_scaler.fit_transform(x)
    X = pd.DataFrame(x_scaled)
    
    
    X = X[[column for consider, column in zip(rfe.support_, X.columns) if consider]]
    
    from sklearn.cross_validation import train_test_split
    
    X_train, X_test, y_train, y_test = train_test_split(X, Y, test_size=0.3, random_state=0)
    
    
    import pandas as pd
    import numpy as np
    from sklearn import svm, datasets
    import matplotlib.pyplot as plt
    
    C = 1.0 # SVM regularization parameter
    svc = svm.SVC(kernel='linear', C=C, decision_function_shape='ovr').fit(X_train, y_train)
    y_pred = svc.predict(X_test)
    
    
    from sklearn.metrics import confusion_matrix, f1_score
    confusion = confusion_matrix(y_test, y_pred)
    #print(confusion)
    tn, fp, fn, tp = confusion.ravel()
    # TrueNegative, FalsePositive, Falsenegative, TruePositive
    
    print "========== Criminal prediction output ============"
    print "Correct predictions = ", tn+tp
    
    
    print "INCorrect predictions = ", fp+fn
    
    score = f1_score(y_test, y_pred)
    print score
    
    
    ========== Criminal prediction output ============
    Correct predictions =  12724
    INCorrect predictions =  992
    0.0