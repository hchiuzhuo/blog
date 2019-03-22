---
layout: post
title: Data science competition collection
category: data science
tag: [cousera]
---


### Abstract
This is a self-learning note for [How to Win a Data Science Competition: Learn from Top Kagglers](https://www.coursera.org/learn/competitive-data-science/home) on cousera.

<!-- more -->
# 1. Week 1

## 1.1 Basics

### 1.1.1 Platforms for training data science competition
- Kaggle
- DrivenData
- CrowdAnalityx
- CodaLab
- DataSciencChallenge.net
- Datascience.net
- Single-competition sites (KDD, VisDooM)

### 1.1.2 Families of ML algorithm

- Linear model.
  - Intuition: Separate data set into two subspaces by a line or a hyper plane.
  - Examples: Logistic regression, SVM.
  - Pros: Good for sparse and high dimensional data.
  - Cons: Too simple for complex data, e.g. ring shape data set.
  - Impl: scikit learn, vowpal wabbit
  [Explanation/Demonstration of Gradient Boosting](http://arogozhnikov.github.io/2016/06/24/gradient_boosting_explained.html)

- Tree-based model.
  - Intuition: Use divide-and-conquer approach to recur sub-split spaces into sub-spaces. Split space into boxes.
  - Examples: decision tree, random forest, GDBT.
  - Pros: Good for tabular data, many winner take this approach.
  - Cons: Hard to capture linear dependencies since it requires a lot of splits.
![Comparision of linear model and tree based model](https://hchiuzhuo.github.io/website/public/postsimg/2018-02-24-cousera-data-science-competition/lineartree.png)
  - Impl: scikit learn (general), dmlc xgboost and microsoft/light gbm (faster, higher accuracy)
  [Explanation of Random Forest](http://www.datasciencecentral.com/profiles/blogs/random-forests-explained-intuitively)

- kNN
  - Intuition: Points close to each other are likely to have similar labels.
  - Distance metric: square distance is the easiest one but sometimes this metric cannot capture semantic such as images.
    Hence, feature based on nearest neighbors are often very informative.
  - Impl: scikit learn.
  [Example of kNN](https://www.analyticsvidhya.com/blog/2014/10/introduction-k-neighbours-algorithm-clustering/)

- Neural Networks
  - Intuition: Feed-forward NNs produce smooth non-linear decision boundary.
  - Pros: Good for images, sounds, text, and sequences.
  - Impl: tensorflow, keras, pytorch (recommend)

> *No free lunch theorem: "Here is no method which outperforms all others for all tasks"* <br>
> Every method relies on some assumptions which relates to the data set.

Conclusion: The most powerful methods are Gradient Boosted Decision Trees and Neural Networks. But other methods should not be underestimate as well.

### 1.1.3 Quiz

## 1.2 Hardware and software
Hardware: 32G ram, 6 cores.
Software: pandas, numpy, matplot etc.

Todo
1. wait for week 2 eda.
2. some part of week 1 haven't finished.

## 1.3 Feature preprocessing and generation with respect to models

For **linear model**, the categorical data type usually may not linear depend on target value.
In this case, one-hot-encoding is a common technique to transform the data set.

| pclass|   1|   2|   3|
| ----- |:--:|:--:|:--:|
| target|   1|   0|   1|

| pclass|pclass==1|pclass==2|pclass==3|
| ----- |:-------:|:-------:|:-------:|
|   1   |    1    |         |         |
|   2   |         |    1    |         |
|   1   |    1    |         |         |
|   3   |         |         |    1    |

However, **random forest** does not need this transformation.

Understand of a model can help us to create useful features.

### 1.3.1 Numeric features
#### Preprocessing: Numerical feature Scaling

Reasons for do feature scaling
We use preprocessing to scale all features to one scale, so that their initial impact on the model will be roughly similar.
For example, when we used KNN for prediction, this could lead to the case where some features will have critical influence on predictions.

Some models may not depend on feature scale.

- Tree-based models
  - decision tree: find the most useful split.
- Non-tree-based models
  - linear, nearest neighbors, NN.

Regularization impacts linear model to be proportional to feature scale.<br>
Gradient descent methods goes out of control without a proper scaling.<br>
Different feature scaling results in different model quality.

Numerical feature Scaling:
1. To \[0,1\] <br>
      sklearn.preprocessing.MinMaxScalar <br>
      X = (X - X.min())/(X.max() - X.min())

```
for titanic dataset.

train[['Age', 'SibSp]].hist(figsize=(10, 4));
scalar = MinMaxScalar()
xtrain = scalar.fit_transform(train[('Age', 'SibSp']])
pd.DataFrame(xtrain).hist(figsize=(10, 4));
```

2. To mean=0, std=1<br>
      sklearn.preprocessing.StandardScalar<br>
      X = (X - X.mean())/ X.std()

MinMaxScaling or StandardScaling transformations force features impacts on non-tree-based models will be roughly similar.
Even more, if you want to use KNN, we can go one step ahead and recall that the bigger feature is, the more important it will be for KNN.

#### Preprocessing: outliers

Reasons for remove outliers.
Linear model is easily be affected by outliers.

Solution: winsorization
Clip features values between two chosen values of lower bound and upper bound and  choose them as some percentiles of that feature.
This procedure of clipping is often applied in financial data.

winsorization
```
pd.Series(x).hist(bins=30)
UPPERBOUND, LOWERBOUND = np.percentile(x, [1, 99])
y = np.clip(x, UPPERBOUND, LOWERBOUND)
pd.Series(y).hist(bins=30)
```

#### Preprocessing: rank transformation
To set spaces between proper variouse values to be equal.<br>
A better option than MinMaxScalar if we have outliers, because outlier ill be placed closer to other points.

 If we apply a rank to the source of array, it will just change values to their indices. Now, if we apply a rank to the not-sorted array, it will sort this array, define mapping between values and indices in this source of array, and apply this mapping to the initial array. Linear models, KNN, and neural networks can benefit from this kind of transformation if we have no time to handle outliers manually.
```
scipy.stats.rankdata
rank([-100, 0, 100])==[0, 1, 2]
```

 One more important note about the rank transformation is that to apply to the test data, you need to store the creative mapping from features values to their rank values. Or alternatively, you can concatenate, train, and test data before applying the rank transformation.

#### Preprocessing: log transformation, square root
Reasons for log, squart root transformation:
Drive too big values closer to the feature's average value.
The values near zero are becoming a bit more distinguishable.
Simple but good fore improve NN.

For non-tree-based models, especially neural network
1. Log transform: np.log(1 + x)
2. Raising to the power < 1: np.sqrt(x + 2/3)

#### Feature generation:
Ways to proceed:
- prior knowledge
  Ex 1: price, size => price/size.
  Ex 2: vertical distance, horizontal distance => directly distance


### 1.3.2 Categorical and ordinal features
#### Preprocessing: Label encoding
Feature value is ordered in some meaningful way. We cannot derive the meaning of difference b.t. ordinal feature value.
Ex. Ticket class: 1,2,3; Driver's license: A, B, C, D
Label encoding: map categorical feature's unique values to different numbers.
Good for tree methods since tree-methods can split feature, and extract most of the useful values in categories on its own.
Bad for non-tree-based-models. If you want to train linear model kNN on neural network, you need to treat a categorical feature differently.

- Encode in alphabetical (sorted) order. [S, C, Q] -> [2, 1, 3]. `sklearn.preprocessing.LabelEncoder`
- Encode in the order of appearance. [S, C, Q] -> [1, 2, 3]. `Pandas.factorize`
- Encode in frequency. [S, C, Q] -> [0.5, 0.3, 0.2]. Preserve some distribution, good for both linear and non linear model.
```
encoding = titanic.groupby('Embarked').size()
encoding = encoding/len(titanic)
titanic['enc'] = titanic.Embarked.map(encoding)
```
Assumption: Value freq is depend on target value.
Note: If several categorical feature has similar distribution, the features won't be distinguishable.
Linear model is not suitable for label encoding features.

#### Preprocessing: One hot encoding.

```
pandas.get_dummies
sklearn.preprcessing.OneHotEncoder
```

| pclass|   1|   2|   3|
| ----- |:--:|:--:|:--:|
| target|   1|   0|   1|

| pclass|pclass==1|pclass==2|pclass==3|
| ----- |:-------:|:-------:|:-------:|
|   1   |    1    |         |         |
|   2   |         |    1    |         |
|   1   |    1    |         |         |
|   3   |         |         |    1    |

Pros: Good for linear feature.
Cons: May generate sparse matrix. The tree-method will slow down.

#### Feature generation
Feature interaction b.w several categorical features.
May help linear model and KNN.

| pclass|   sex|   pclass_sex|
| ----- |:----:|:-----------:|
|      3|  male|        3male|
|      1|female|      1female|
|      3|  male|      3female|
|      1|female|      1female|

Pclass_sex ==

| 1male|1female|2male|2female|3male|3female|
| ---- |:-----:|:---:|:-----:|:---:|:-----:|
|      |       |     |       |  1  |       |
|      | 1     |     |       |     |       |
|      |       |     |       |     |       |
|      | 1     |     |       |     |   1   |



### 1.3.3 Datetime and coordinates features

#### Date and time
1. Periodicity: Capture repetitive patterns in the data
Day number in week, month, season, year, second, minute, hour

Ex. Patient is prescribed on Wed per week.

2. Time since
  a. Row-independent moment. ex: since 00:00:00 UTC, 1 Jan 1970.
  b. Row-dependent important moment. ex: Number of days left until next holidays./ time passed after last holiday.

3. Difference b.t. dates: datetime_feature_1 - datetime_feature_2

 ![DateTimeGen](../public/postsimg/2018-02-24-cousera-data-science-competition/Datetime-genfeature.png)
<!-- link for local website test ![PBFT](/website/public/postsimg/2018-02-24-cousera-data-science-competition/Datetime-genfeature.png) -->
<!-- link for github website ![PBFT](https://hchiuzhuo.github.io/website/public/postsimg/2018-02-24-cousera-data-science-competition/Datetime-genfeature.png) -->

Date and sales are original feature. Others are generated feature.

#### Coordinates
1. Interesting places from train/test data or additional data <br>
2. Centers of clusters <br>
3. Aggregated statistics <br>

### 1.3.4 Missing values.
Type: NA values, Empty Strings, -1, Very Large Numbers, -99999 (and less), 999, 99

![FindNAN](../public/postsimg/2018-02-24-cousera-data-science-competition/FindNAN.png)
<!-- link for local website test ![PBFT](/website/public/postsimg/2018-02-24-cousera-data-science-competition/Datetime-genfeature.png) -->
<!-- link for github website ![PBFT](https://hchiuzhuo.github.io/website/public/postsimg/2018-02-24-cousera-data-science-competition/Datetime-genfeature.png) -->

Fillna approaches
1. -999, -1, etc
Pros: it gives three possibility to take missing value into separate category. <br>
Cons: this is that performance of linear networks can suffer.

2. mean, median
Pros: Usually beneficial for simple linear models and neural networks.
Cons: For trees it can be harder to select object which had missing values in the first place.

3. Reconstruct value.
Add binary feature "is null" can be beneficial but sometimes double the number of columns.
Avoid filling nans before feature generation.
Xgboost can handle NaN.

### 1.3.5 Quiz

Suppose you have a dataset X, and a version of X where each feature has been standard scaled.
For which model types training or testing quality can be much different depending on the choice of the dataset?
1.Linear models, 2.GBDT, 3.Random Forest, 4.Neural network, 5.Nearest neighbours
ans. 1, 4, 5
There are two reasons for this: first, amount of regularization applied to a feature depends on the feature's scale. Second, optimization methods can perform differently depending on relative scale of features.


## 1.4 Feature extraction from text and images.

### 1.4.1 Text
Text -> vector
1. Bag of words :TFIDF, N-grams
2. Embeddings (~word2vec)

Text preprocessing
1. lowercase
2. lemmatization
3. stemming
4. stopwords


# 2. Week 2

## 2.1  EDA
- Get domain knowledge.
- Check if the data is intuitive.
- Understand how the data is generated.


## 2.2 Process anonymized features
- Try to decode the feature: guess the true meanings.
- Guess the feature types.

```
Helpful functions:
df.dtypes
df.info()
x.value_counts()
x.isnull()
```
## 2.2 Visualizations

One dimension
```
Histograms: plt.hist(x)
Plot(index versus value): plt.plot(x, '.')
Stats:
   df.describe()
   x.mean()
   x.var()
Others:
  x.value_counts() //count distinct values
  x.isnull()
```

2 dimension
```
plt.scatter(x1, x2)
pd.scatter_matrix(df)
df.corr(), plt.matshow(...)
df.mean().sort_values().plot(style='.')
```

[todo]
Get more info from [iris eda](https://www.youtube.com/playlist?list=PLS8ACsmFCpmSvhSv4TUkP6OJ1o4IEDEAc_
### Ref
[[1] competitive-data-science](https://www.coursera.org/learn/competitive-data-science/lecture/7I3do/competition-mechanics)
[[2] git repo](https://github.com/hchiuzhuo/competitive-data-science)
[[3] iris eda](https://www.youtube.com/playlist?list=PLS8ACsmFCpmSvhSv4TUkP6OJ1o4IEDEAc)
