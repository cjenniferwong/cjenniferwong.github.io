---
layout: post
title: Topic Modeling with Non-Negative Matrix Factorization
---

## Problem Statement: Unstructured text data that hasn't been analyzed. Can we used topic modeling to identify topics in the data and provide structure for future analyses and product development?

I have an unstructured text dataset that I wish to create structure in a way that makes it easier to analyze and draw insight from. With topic modeling, I can provide dimensions to analyze these data.

Principal Component Analysis is a well documented way to reduce dimensionality, but there are little resources for another form of matrix factorization: Non-Negative Matrix Factorization.

What is Non-Negative Matrix Factorization (NMF)?

![Graphical Depiction of NMF](/assets/matrix_factorization.png)

How does it compare to PCA?
Both are a forms of decomposition, but are implemented with different constraints on the factorization. PCA strives to identify components which are able to capture the greatest variance in the data points. NMF restricts the loading scores of matrices to be strictly non-negative. Philosophically, NMF mirrors the process of text generation more closely aligned with our mental model, as each component adds (non-negative) contributions to the final product.

How can I use it for topic modeling?

How do I evaluate the quality of my topics?


![Elbow Plot for Topic Modeling with NMF](/assets/Topic_Modeling_NMF.svg)
