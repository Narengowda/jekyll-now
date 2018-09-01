---
title: Perceptron in 20 lines of code
undefined: 'Description '
created_date: 2018-01-16 18:30:00 +0000
date: 2018-09-01 21:07:34 +0530

---
\`\`\`python

import pandas as pd

import numpy as np

import warnings

warnings.filterwarnings('ignore')

%matplotlib inline

import matplotlib

import numpy as np

import matplotlib.pyplot as plt

\`\`\`

    /usr/local/lib/python2.7/site-packages/pandas/_libs/__init__.py:4: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from .tslib import iNaT, NaT, Timestamp, Timedelta, OutOfBoundsDatetime

    /usr/local/lib/python2.7/site-packages/pandas/__init__.py:26: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import (hashtable as _hashtable,

    /usr/local/lib/python2.7/site-packages/pandas/core/dtypes/common.py:6: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import algos, lib

    /usr/local/lib/python2.7/site-packages/pandas/core/util/hashing.py:7: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import hashing, tslib

    /usr/local/lib/python2.7/site-packages/pandas/core/indexes/base.py:7: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import (lib, index as libindex, tslib as libts,

    /usr/local/lib/python2.7/site-packages/pandas/tseries/offsets.py:21: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      import pandas._libs.tslibs.offsets as liboffsets

    /usr/local/lib/python2.7/site-packages/pandas/core/ops.py:16: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import algos as libalgos, ops as libops

    /usr/local/lib/python2.7/site-packages/pandas/core/indexes/interval.py:32: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs.interval import (

    /usr/local/lib/python2.7/site-packages/pandas/core/internals.py:14: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import internals as libinternals

    /usr/local/lib/python2.7/site-packages/pandas/core/sparse/array.py:33: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      import pandas._libs.sparse as splib

    /usr/local/lib/python2.7/site-packages/pandas/core/window.py:36: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      import pandas._libs.window as _window

    /usr/local/lib/python2.7/site-packages/pandas/core/groupby/groupby.py:68: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import (lib, reduction,

    /usr/local/lib/python2.7/site-packages/pandas/core/reshape/reshape.py:30: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import algos as _algos, reshape as _reshape

    /usr/local/lib/python2.7/site-packages/pandas/io/parsers.py:45: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      import pandas._libs.parsers as parsers

    /usr/local/lib/python2.7/site-packages/pandas/io/pytables.py:50: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88

      from pandas._libs import algos, lib, writers as libwriters

Lets write a basic implementation of perseptron

[https://towardsdatascience.com/what-the-hell-is-perceptron-626217814f53](https://towardsdatascience.com/what-the-hell-is-perceptron-626217814f53 "https://towardsdatascience.com/what-the-hell-is-perceptron-626217814f53")

The perceptron consists of 4 parts .

1 Input values or One input layer

2 Weights and Bias

3 Net sum

4 Activation Function

FYI: The Neural Networks work the same way as the perceptron. So, if you want to know how neural network works, learn how perceptron works.

But how does it work?

The perceptron works on these simple steps

a. All the inputs x are multiplied with their weights w. Let’s call it k.

b. Add all the multiplied values and call them Weighted Sum.

c. Apply that weighted sum to the correct Activation Function.

    For Example : Unit Step Activation Function.

    

d. Why do we need Weights and Bias?

    Weights shows the strength of the particular node.

    A bias value allows you to shift the activation function curve up or down.

e. Why do we need Activation Function?

In short, the activation functions are used to map the input between the required values like (0, 1) or (-1, 1).

!\[\]([https://cdn-images-1.medium.com/max/1600/1](https://cdn-images-1.medium.com/max/1600/1 "https://cdn-images-1.medium.com/max/1600/1")*ztXU57QEETPHGXczHrSWSA.gif)

\`\`\`python

\# Lets do some sample code replicating whats show in the above diagram with different input size

\# this is with  10 different columns but only one input(training input)

data_len = 10

df = pd.DataFrame()

\# x  --->  x1, x2, x3 .......... x10

df\['x'\] = \[0,1,1,1,1,1,1,1,1,1\]

\#  w --->  w1, w2, w3, w4..........w10

df\['w'\] = \[1.0/data_len for i in range(data_len)\]

s = df\['x'\] * df\['w'\]

s

\`\`\`

    0    0.0

    1    0.1

    2    0.1

    3    0.1

    4    0.1

    5    0.1

    6    0.1

    7    0.1

    8    0.1

    9    0.1

    dtype: float64

\`\`\`python

\#A bias value allows you to shift the activation function curve up or down

bias = -1

s_sum = s.sum() + bias

print(s_sum)

if s_sum > -0.001:

    print("light")

else:

    print("dark")

\`\`\`

    -0.09999999999999998

    dark

 Lets try little advanced with multiple colums

\`\`\`python

\# now we have sample data set with two columns ie. x1 and x2

\#        

dataset = \[

    #  X1          X2

    \[2.7810836,2.550537003,0\],

    \[1.465489372,2.362125076,0\],

    \[3.396561688,4.400293529,0\],

    \[1.38807019,1.850220317,0\],

    \[3.06407232,3.005305973,0\],

    \[7.627531214,2.759262235,1\],

    \[5.332441248,2.088626775,1\],

    \[6.922596716,1.77106367,1\],

    \[8.675418651,-0.242068655,1\],

    \[7.673756466,3.508563011,1\]\]

\#                 BIAS,      W1,                    w2

weights_dataset = \[-0.1, 0.20653640140000007, -0.23418117710000003\]

dataset = sorted(dataset, key=lambda x:x\[0\])

plt.plot(\[i\[0\] for i in dataset\], \[i\[1\] for i in dataset\], 'ro')

\# filter positive output dataset

dataset_true_op = \[\[i\[0\], i\[1\]\] for i in dataset if i\[2\] == 1\]

plt.plot(\[i\[0\] for i in dataset_true_op\], \[i\[1\] for i in dataset_true_op\], 'go')

\# Plot weghits with some offset to see how it looks

plt.plot(\[weights_dataset\[1\]+5\], \[weights_dataset\[2\]+5\], 'bx')

\`\`\`

    \[<matplotlib.lines.Line2D at 0x112dec110>\]

\`\`\`python

def predict(row, weights):

    # takes a row \[x1, x2, x3.....\]

    # weight \[bias, w1, w2, w3 .....\]

    

    # lets initiate activation with bias

    # its equal to 

    # activation = sum(weight_i * x_i) + bias

    # Step activation function ->  prediction = 1.0 if activation >= 0.0 else 0.0

    

    activation = weights\[0\]

    just_weights = weights\[1:\]

    

    for x, w in zip(row, just_weights):

        activation += x * w

    

    return 1.0 if activation >= 0 else -1.0

    

    

for data in dataset:

    output = predict(data\[:-1\], weights_dataset)

    print "actual = {}   predicted = {}".format(data\[-1\], output)

    

\# So with the given weights actual value and predicted is correct    

\`\`\`

    actual = 0   predicted = -1.0

    actual = 0   predicted = -1.0

    actual = 0   predicted = -1.0

    actual = 0   predicted = -1.0

    actual = 0   predicted = -1.0

    actual = 1   predicted = 1.0

    actual = 1   predicted = 1.0

    actual = 1   predicted = 1.0

    actual = 1   predicted = 1.0

    actual = 1   predicted = 1.0

\`\`\`python

\# Lets write a perceptron which learns using feedback

DX = np.array(\[

    \[-2, 4\],

    \[4, 1\],

    \[1, 6\],

    \[2, 4\],

    \[6, 2\]

\])

DY = np.array(\[-1,-1,1,1,1\])

X_one = \[x for x, y in zip(DX, DY) if y == 1\]

X_minus_one = \[x for x, y in zip(DX, DY) if y == -1\]

plt.plot(\[i\[0\] for i in X_one\], \[i\[1\] for i in X_one\], 'ro')

plt.plot(\[i\[0\] for i in X_minus_one\], \[i\[1\] for i in X_minus_one\], 'bo')

\`\`\`

    \[<matplotlib.lines.Line2D at 0x11507a690>\]

\`\`\`python

\# Lets write a perceptron which learns using feedback

def learn(X, Y, epochs=1):

    # weights be the lenght of columns of dataset

    weights = np.zeros(len(X\[0\]) + 1)

    

    # set learning rate

    eta = 1

    

    # lets monitor errors

    errors_list = \[\]

    

    for learning_round in range(epochs):

        print("---- Learning {} -----".format(learning_round))

        # to calculate error at every round/epoch

        total_error = 0

        

        for x, y in zip(X, Y):

            # np.dot is matrix multiplication

            # eg: np.dot(\[1,2\], \[4,5\]) => 14

            # ∑ (x*w) * y  -> Error shoul=d 

            

            prediceted_out = predict(x, weights)

            error =  eta * (y - prediceted_out)

            weights\[1:\] = weights\[1:\] + (x * error)

            weights\[0\] = weights\[0\] + error

            

            total_error += abs(error)

            

            

            print("x= {}, weights= {}, y= {} error= {}  predicted = {}".format(x, weights, y, error, prediceted_out))

                

        errors_list.append(error)

                

                

    plt.plot(errors_list)

    plt.xlabel('Epoch')

    plt.ylabel('Total Loss')                

                

    return weights

\`\`\`

\`\`\`python

\# add bias to existing array

\# DX = np.array(\[

\#     \[-2, 4, -1\],

\#     \[4, 1, -1\],

\#     \[1, 6, -1\],

\#     \[2, 4, -1\],

\#     \[6, 2, -1\]

\# \])

print(learn(DX, DY, 2))

\`\`\`

    ---- Learning 0 -----

    x= \[-2  4\], weights= \[-2.  4. -8.\], y= -1 error= -2.0  predicted = 1.0

    x= \[4 1\], weights= \[ -4.  -4. -10.\], y= -1 error= -2.0  predicted = 1.0

    x= \[1 6\], weights= \[-2. -2.  2.\], y= 1 error= 2.0  predicted = -1.0

    x= \[2 4\], weights= \[-2. -2.  2.\], y= 1 error= 0.0  predicted = 1.0

    x= \[6 2\], weights= \[ 0. 10.  6.\], y= 1 error= 2.0  predicted = -1.0

    ---- Learning 1 -----

    x= \[-2  4\], weights= \[-2. 14. -2.\], y= -1 error= -2.0  predicted = 1.0

    x= \[4 1\], weights= \[-4.  6. -4.\], y= -1 error= -2.0  predicted = 1.0

    x= \[1 6\], weights= \[-2.  8.  8.\], y= 1 error= 2.0  predicted = -1.0

    x= \[2 4\], weights= \[-2.  8.  8.\], y= 1 error= 0.0  predicted = 1.0

    x= \[6 2\], weights= \[-2.  8.  8.\], y= 1 error= 0.0  predicted = 1.0

    \[-2.  8.  8.\]