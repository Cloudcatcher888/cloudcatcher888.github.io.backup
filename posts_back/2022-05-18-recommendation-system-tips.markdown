---
layout: post
title:  "Recommendation System Tips"
date:   2022-05-18
categories: jekyll update
---

# Some details of the recommendation system

## When are auc and ndcg used?

When recommending system evaluation, two groups of indicators are often encountered, one is AUC and Logloss (with negative samples), and the other is Recall, HitRate and NDCG (without negative samples, random sampling is required). These two groups of indicators often appear separately. Which group to use is related to whether there are negative samples in the data set (it has nothing to do with whether the model considers sequence, sorting model or recall model). Data sets with negative samples are often called click-through rate estimation problems. The scene of the sample is that the user passively accepts exposure, and the user selects certain items to click. At this time, the distribution of negative samples can be considered to have nothing to do with user preferences (although the model tends to push items that users like, this involves causality and bias. problem, not listed for the time being), the scene without negative samples is obtained based on the conversion of explicit data sets such as scoring or review data sets, and negative samples need to be randomly sampled.




