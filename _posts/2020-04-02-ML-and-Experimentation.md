---
layout: post
title: Machine Learning & Experimentation
tags: statistics hypothesis_testing bootstrap permutation
---
While doing code review for a Machine Learning project, I saw that a colleague approached data cleaning differently than I do--they applied data transformations on the entire dataframe, while I have always done it on select columns. I thought it was interesting, and wondered which was more computationally efficient (if there was a clear difference at all). Upon scouring google and StackExchange, I found that the answer to my question could not be found on the internet.
![google_search](/assets/stats/google_search.png)
I decided this was a perfect example to use for a mental exercise in Experimentation and Statistical Analysis.


<!-- more -->


## Experimental Design
[Jupyter Notebook](https://bit.ly/stats_notebook)

To complete this analysis I need to know:
- Null hypothesis
- Alternative hypothesis

Because I am interested to know if there was a detectable difference between the two data transformation methods, I needed to conduct a 2-sample Hypothesis test and generate synthetic to analyze. I set my tolerance for false-positives (critical p-value) to be 5%, and therefore a corresponding p-value of 0.05 by convention.

I created data frames of varying lengths and measured the time it took to complete the transformation. I imagined that if I were a Product Manager faced with this optimization decision, I need to decide how much savings in time and resources would be enough evidence to put this on the roadmap, depending on the time it would take to implement the changes (cost) versus the benefits of these changes in this cost-benefit analysis. This is important to know beforehand for conducting a Power Analysis.

### Power Analysis

A Power Analysis is comprised of 4 components:
- Effect size
- Sample size
- Significance
- Power

In order to estimate the sample size, we would need to estimate the effect size (aka minimum detectable effect) that we’re interested in detecting to achieve statistical power of 80% (this is set by convention). This is important because we don’t want to conduct an underpowered test, which would waste time and resources. Conversely, an over-powered test also wastes time and resources collecting data that contribute marginal increase in accuracy. By conducting a power analysis, we have a reference point to decide the optimal length of time needed to run the experiment given our constraints.

### Exploratory Analysis
After collecting the data that I needed to conduct the experiment, I plotted the results in a boxplot to easily visualize the differences between these two samples. There doesn’t seem to be a difference between them at first glance, but I decided to do my due-diligence and make sure by performing statistical tests of significance.
![Boxplot Test](/assets/stats/Boxplot.svg)
## Statistical Analysis
### Student's T-Test
To perform a t-test, I needed to make sure the assumptions were met since it is a parametric test. Most notably, the t-test assumes that the population from which we are drawing samples from is of a normal distribution. These boxplots suggest that I do not meet this requirement. A way to quickly visually assess normality would be using the Quantile-Quantile (QQ) plots. Here is what a QQ plot looks like for a normally distributed population
![Normal QQ Plot](/assets/stats/Normal_QQ_Plot.svg)
Here is what the QQ plots for my samples look like. Just to be safe, I opted for non-parametric tests like the Mann-Whitney U test. After running this test, I get a p-value which suggests that the two distributions of my experiment are different. Unfortunately this doesn't tell me _what_ is different about my samples (e.g. mean, median, variance, etc.). To further explore the differences, I used nonparametric resampling techniques like Permutation Tests and Bootstrapping. Because these do not make assumptions about the underlying data, it is ok to use it for this experiment.
![Not Normal QQ Plot](/assets/stats/Not_Normal_QQ_Plot.svg)

## Results
I conducted Permutation tests on the mean, median, and variance to identify how my samples are different. The p-values for differences in mean, medians and variance suggest that we have evidence to reject the null hypothesis. Note, because technically we are applying multiple statistical tests to the same dataset, some correction needs to be applied to prevent the increased risk of false-positives. Even with a Bonferroni correction applied to these p-values, they are sufficiently low to reject the null hypothesis.
![Permutation for Mean Test Statistic](/assets/stats/Permutation_Mean.svg)
![Permutation for Median Test Statistic](/assets/stats/Permutation_Median.svg)

To quantify how different the median and variances are between the populations, I used bootstrapping to create confidence intervals for these test statistics. Because of the Central Limit Theorem, bootstrapping exhibits a normal distribution for the test-statistics.
![Bootstrap Mean](/assets/stats/Bootstrap_Mean.svg)
![Bootstrap Median](/assets/stats/Bootstrap_Median.svg)
Our 95% confidence interval finds a 1.56% to 1.62% mean reduction in computation time. This is interpreted as `if we conducted multiple repeated studies, we are 95% confident that the true population parameter is captured in this range.`

### Key Takeaways
### Statistical Significance v. Practical Significance
But just because something is statistically significant, doesn’t mean that it is _practically_ significant. By this, I mean we need to ask ourselves if this magnitude of difference makes a justifiable impact to our business and decisions. Conversely, keep in mind that the threshold for statistical significance is based on tolerance for false positives. This can be unique to each situation and does not need to be 5%. Depending on the experiment and company, you may choose to move forward with the changes even though you didn’t meet the statistical significance threshold--that is something that you as a stakeholder will decide.
