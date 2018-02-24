---
layout: post
title: Feature mining
category: data science
tag: [feature engineering]
---
### Abstract
<!-- more -->

## 1. An overview of feature mining

### 1.1 The power of increasing dimensionality

When 1 dimension cannot separate 2 classes of data, we can try to extend 1 dimension to 2 dimensions to see if there is any possibility to classify datasets.
(todo: see example)

### 1.2 The curse of dimensionality

The hughes phenomenon.
Good: When the number of dimensions is increase, the separability will increase as well. However, the curve grows slow when the number of dimensions goes beyond a threshold.
Bad:  When the number of dimensions is increase, the accuracy of statistics estimation drops dramatically. Furthermore, the accuracy depends on the dataset size,
meaning when we fix the number of dimensions, the classifier trained from the larger dataset size will have better prediction accuracy.

### Ref
[1. Big Data Analysis - Feature Extraction / Feature Mining](https://www.youtube.com/playlist?list=PLt0SBi1p7xrSGg3HSbK0pU9bFf_Cd86Cd)
