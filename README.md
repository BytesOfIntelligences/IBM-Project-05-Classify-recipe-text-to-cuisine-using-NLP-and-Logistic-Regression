<div align="center">
      <h1> <img src="https://github.com/ahammadmejbah/IBM-Project-02-Transform-Photos-to-Sketches-and-Paintings-with-OpenCV/blob/main/Additional%20Files/SN_web_lightmode.svg" width="300px"><br/>IBM Project 05 Classify recipe text to cuisine using NLP and Logistic Regression</h1>
     </div>

## **Table of Contents**

<ol>
    <li><a href="https://#Objectives">Objectives</a></li>
    <li>
        <a href="https://#Setup">Setup</a>
    </li>
    <li>
        <a href="https://#Background">Background</a>
        <ol>
            <li><a href="https://#What-does-Logistic-Regression-do?">What does Logistic Regression do?</a></li>
            <li><a href="https://#How-does-Logistic-Regression-work?">How does Logistic Regression work?</a></li>
        </ol>
    </li>
    <li><a href="https://#Example-1--Classification-Problem:-Predicting-the-types-of-cuisine">Example 1 - Classification Problem:  Predicting the types of cuisine</a></li>
</ol>

<a href="https://#Exercises">Exercises</a>

<ol>
    <li><a href="https://#Exercise-1---Prepare-the-data">Exercise 1 - Prepare the data</a></li>
    <li><a href="https://#Exercise-2---Build-the-Logistic-Regression-model">Exercise 2 - Build the Logistic Regression model</a></li>
</ol>


## Objectives

After completing this lab you will be able to:

*   Use Logistic Regression to solve classify recipes into cuisine categories.
*   Explain how logistic regression works

### Setup

For this lab, we will be using the following libraries:

*   [`pandas`](https://pandas.pydata.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01) for managing the data.
*   [`numpy`](https://numpy.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01) for mathematical operations.
*   [`sklearn`](https://scikit-learn.org/stable/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01) for machine learning and machine-learning-pipeline related functions.
*   [`seaborn`](https://seaborn.pydata.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01) for visualizing the data.
*   [`matplotlib`](https://matplotlib.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0187ENSkillsNetwork31430127-2021-01-01) for additional plotting tools.


### Importing Required Libraries

*We recommend you import all required libraries in one place (here):*


``` python
from tqdm import tqdm
import skillsnetwork
import json
import scipy
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix
from sklearn.metrics import classification_report
from sklearn.linear_model import LogisticRegressionCV
from sklearn.decomposition import PCA, TruncatedSVD
from sklearn.metrics import accuracy_score

# You can also use this section to suppress warnings generated by your code:
def warn(*args, **kwargs):
    pass
import warnings
warnings.warn = warn
warnings.filterwarnings('ignore')

```

### How does Logistic Regression work?

We'll go through the explanation for this algorithm with our recipe example.

#### The data

We just have two dishes, $x\_1$ and $x\_2$. They are each japanese and italian dishes respectively, and we have indicated their ingredient features with `0`'s and `1`'s.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-PY0181EN-SkillsNetwork/labs/2_linear_classifiers/images/Data.png" alt="drawing" width="500">

#### Step 1

Imagine we have already trained our binary logistic regressor, and the *$Parameters$* have already been determined. Each $\beta$ represents a parameter we have trained.

We will now plug in the features from our two dishes into the regression equation $f(x)$ which is the **sum** of the **product** of our inputs and parameters.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-PY0181EN-SkillsNetwork/labs/2_linear_classifiers/images/step1.png" alt="drawing" width="500">

#### Step 2

This is the important part! We now plug in the previous result into the logit function which will spit out a value between 0 and 1. This value can be interpreted as a probability of either class.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-PY0181EN-SkillsNetwork/labs/2_linear_classifiers/images/step2.png" alt="drawing" width="500">

#### Visualization

Here we visualize our results. Our two dishes, after being run through logistic regression, have been mapped onto a logit function with their $y$ values signifying how much they belong to either class. The further right you go, the more Italian the dish and vice versa.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-PY0181EN-SkillsNetwork/labs/2_linear_classifiers/images/visualization.png" alt="drawing" width="500">


### All together

Here is the big picture:
![image](https://github.com/ahammadmejbah/IBM-Project-05-Classify-recipe-text-to-cuisine-using-NLP-and-Logistic-Regression/assets/56669333/25f06ae0-2c72-43ae-b3cc-4adc9ba06dc0)


### Exercise 1 - Prepare the data

Now above we have the DataFrame all prepared, we need to split it into training and testing data. We'll be using the `train_test_split` function from sklearn.

```python
train_test_split(*arrays, test_size=None, train_size=None, random_state=None, shuffle=True, stratify=None)

#Example
X_train, X_test, y_train, y_test = train_test_split(
...     X, y, test_size=0.33, random_state=42)
...
>>> X_train
array([[4, 5],
       [0, 1],
       [6, 7]])
>>> y_train
[2, 0, 3]
>>> X_test
array([[2, 3],
       [8, 9]])
>>> y_test
[1, 4]
```

We'll be using the `stratify` parameter to stratify our data, because we have an inbalanced dataset with a large number of Mexican dishes/datapoints.

### Exercise 2 - Build the Logistic Regression model


We'll be using the sci-kit learn package to implement [logistic regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMPY0181ENSkillsNetwork36484875-2022-01-01). Make sure to use the `multi_class="multinomial"` argument.

```python
LogisticRegression(penalty='l2', *, dual=False, tol=0.0001, C=1.0, fit_intercept=True, intercept_scaling=1, class_weight=None, random_state=None, solver='lbfgs', max_iter=100, multi_class='auto', verbose=0, warm_start=False, n_jobs=None, l1_ratio=None)

#Example
>>> lr = LogisticRegression(random_state=0).fit(X, y)

>>> y_true = [0, 1, 2, 2, 2]
>>> y_pred = lr.predict(x_test)
>>> print(classification_report(y_true, y_pred, target_names=target_names))
```

### Option 1: Implementing Cross Validation

Cross validation is the practice of taking a random set from within the training data and using it to optimize the algorithm. This improves it's ability to generalize (perform well when given new data)


![image](https://github.com/ahammadmejbah/IBM-Project-05-Classify-recipe-text-to-cuisine-using-NLP-and-Logistic-Regression/assets/56669333/3f10fb86-ff8c-4145-b91a-3a16211ad50b)

By Gufosowa - Own work, CC BY-SA 4.0, [https://commons.wikimedia.org/w/index.php?curid=82298768](https://commons.wikimedia.org/w/index.php?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMPY0181ENSkillsNetwork36484875-2022-01-01&curid=82298768)

Here, we'll use 5 folds but you can use as many or as little as you like.


### Option 5: Playing with testing/training sizes

One more thing we can do is increasing/decreasing the testing and training sizes we have used. Giving our algorithm more data to train on could result in better testing accuracy.

We'll do this for a range of training set sizes, from 60% all the way to 90% in increments of 5%.


```python

# TAKES A WHILE

test_sizes = np.arange(.60, .95, .05)

train_scores = list()
test_scores = list()

for size in tqdm(test_sizes):
    x_train, x_test, y_train, y_test = train_test_split(df_pca, df['cuisine'], train_size=size, random_state=1, stratify=df['cuisine'])
    lr_cv = LogisticRegression().fit(x_train, y_train)
    
    train_scores.append(accuracy_score(y_train, lr_cv.predict(x_train)))
    test_scores.append(accuracy_score(y_test, lr_cv.predict(x_test)))

```


``` python

# plot lines
plt.plot(test_sizes, train_scores, label = "Training Accuracy")
plt.plot(test_sizes, test_scores, label = "Testing Accuracy")
plt.xlabel("Training size as percentage")
plt.ylabel("Accuracy")
plt.title("Accuracy Scores by Test Size")
plt.legend()
plt.show()

```

## Authors

[Richard Ye](https://linkedin.com/in/richard-ye?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMPY0181ENSkillsNetwork36484875-2022-01-01)


| Date (YYYY-MM-DD) | Version | Changed By | Change Description |
| ----------------- | ------- | ---------- | ------------------ |
| 2022-07-10        | 0.1     | Richard    | Create Lab         |

#### Copyright © 2022 IBM Corporation. All rights reserved.

<!-- </> with 💛 by readMD (https://readmd.itsvg.in) -->
    
