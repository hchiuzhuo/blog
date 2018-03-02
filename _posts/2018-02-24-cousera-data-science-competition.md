---
layout: post
title: Data science competition collection
category: data science
tag: [cousera]
---


### Abstract
<!-- more -->
## 1. Week 1

#### Platforms for training data science competition
- Kaggle
- DrivenData
- CrowdAnalityx
- CodaLab
- DataSciencChallenge.net
- Datascience.net
- Single-competition sites (KDD, VisDooM)

#### Families of ML algorithm
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
![Comparision of linear model and tree based model](../public/_postsimg/2018-02-24-cousera-data-science-competition/lineartree.png)
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

### Quiz

### Hardware and software
Hardware: 32G ram, 6 cores.
Software: pandas, numpy, matplot etc.

#### Todo
1. wait for week 2 eda.
2. some part of week 1 haven't finished.

### Feature preprocessing and generation with respect to models

#### Feature preprocessing
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

#### Feature generation
[todo] not so clear.

### Numeric features

### Ref
[[1] competitive-data-science](https://www.coursera.org/learn/competitive-data-science/lecture/7I3do/competition-mechanics)
[[2] git repo](https://github.com/hchiuzhuo/competitive-data-science)
