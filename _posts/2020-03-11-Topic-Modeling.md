---
layout: post
title: Topic Modeling with Non-Negative Matrix Factorization
tags: linear_algebra unsupervised_learning topic_modeling delta
---

## Problem Statement: Unstructured text data that hasn't been analyzed. Can we used topic modeling to identify topics in the data and provide structure for future analyses and product development?

[jupyter notebook](bit.ly/topic_modeling)

Unstructured text data provides a wealth of information, but require time and resources to parse through the corpus. This doesn't scale well when the corpus increases in volume. Luckily with topic modeling, we are able to utilize Statistics and Linear Algebra to quickly and efficiently extract insights.

There are a multitude of ways to implement topic modeling--you have to figure out which way is most appropriate given the data, and the problem statement. A probabilistic approach to this is **Latent Dirichlet Allocation (LDA)**.

Another well known way to reduce dimensions is **Principal Component Analysis (PCA)**. Note: when applied to text data, this method is usually referenced as **Latent Sentiment Analysis (LSA).**

I found that Non-Negative Matrix Factorization provided better results than LDA, and LSA.

##### What is Non-Negative Matrix Factorization (NMF)?

![Graphical Depiction of NMF](/assets/topic_modeling/matrix_factorization.png)

##### How does it compare to PCA?
Both are a forms of decomposition, but are implemented with different constraints on the factorization. PCA strives to identify components which are able to capture the greatest variance in the data points. NMF restricts the loading scores of matrices to be strictly non-negative. Philosophically, NMF mirrors the process of text generation more closely aligned with our mental model, as each component adds (non-negative) contributions to the final product.

##### How can I use it for topic modeling?

##### How do I evaluate the quality of my topics?
Because we are using NMF, we are able to evaluate how well this is able to represent the original matrix. Using the Frobenius norm (aka the Euclidean distance) we can quantify the reconstruction error at different numbers of components/topics. By looking at the gradient of the reconstruction error, we can identify when the gradient seems to be making marginal impact. This is effectively the elbow plot.

![Elbow Plot for Topic Modeling with NMF](/assets/topic_modeling/Topic_Modeling_NMF.svg)

##### What value does topic modeling provide?
Topic modeling is able to create structure from an unstructured dataset. In addition to uncovering topics in the data for product development and user/product research, we can utilize these dimensions to compare data and surface recommendations for similar text.
