---
marp: true
theme: dsi-certificates-theme
_class: invert
paginate: true
math: mathjax
---

# 5: Logistic Regression

```code
$ echo "Data Science Institute"
```

---

# Logistic Regression

**Logistic regression** models the probability that the response $Y$ belongs to a particular category. Suppose we have a qualitative response $Y$ that has two levels, coded as 0 and 1, and one predictor variable. We want to model


> $
p(X)=\operatorname{Pr}(Y=1 \mid X)
$

The logistic function keeps the probabilities between 0 and 1. For one predictor, the function is 
> $
p(X)=\frac{e^{\beta_{0}+\beta_{1} X}}{1+e^{\beta_{0}+\beta_{1} X}}
$ As with linear regression, we are trying to fit $\beta_0, \beta_1$.

---

# Estimating the regression coefficients

$\beta_0$ and $\beta_1$ are estimated using the training data using a method called **maximum likelihood**. This involves maximizing the likelihood function, but we will not cover the details of this function.

---

# Odds

The **odds** compares the probability of a particular outcome to the probability of all the other outcomes.


> $
    \frac{p(X)}{1-p(X)}=e^{\beta_{0}+\beta_{1} X}
$

-   takes values between (0, $\infty$)

-   odds close to 0 $\Rightarrow$ very low probability of the outcome in question

-   odds much greater than 0 $\Rightarrow$ very high probability of the outcome in question.

---

# Log Odds

The **log odds** (or logit) is obtained by taking the logarithm of the odds 
> $
\log \left(\frac{p(X)}{1-p(X)}\right)=\beta_{0}+\beta_{1} X
$

-   Increasing $X$ by one unit changes the log odds by $\beta_1$.

-   If $\beta_1$ is positive, increasing $X$ is associated with increasing $p(X)$

-   If $\beta_1$ is negative, increasing $X$ is associated with decreasing $p(X)$

---

# Making Predictions

Once the coefficients have been estimated predictions can be made for any value of the predictor. Logistic regression will give the probability of the outcome and the classification will be according to some threshold which depends on the problem or how conservative the predictions should be.

---

# Multiple Predictors

Simple logistic regression can be extended to include multiple predictors 
> $
p(X)=\frac{e^{\beta_{0}+\beta_{1} X_{1}+\cdots+\beta_{p} X_{p}}}{1+e^{\beta_{0}+\beta_{1} X_{1}+\cdots+\beta_{p} X_{p}}}$

The log odds in this case becomes 
> $
\log \left(\frac{p(X)}{1-p(X)}\right)=\beta_{0}+\beta_{1} X_{1}+\cdots+\beta_{p} X_{p}$

As before, the maximum likelihood is used to estimate the coefficients.

---

# Exercise: Logistic Regression

Open the Classification Exercises R Markdown or Jupyter Notebook file.

-   Go over "Getting Started" together as a class.

-   Go through the "Logistic Regression" as a class.

-   5 minutes for students to complete the questions from "Logistic Regression".

-   Questions should be completed at home if time does not allow.

---

# Multinomial logistic regression

We can extend to two-class logistic regression to accommodate $K$ classes. We need to select one class to serve as the **baseline**, so we will choose the $K$th class. Then the model becomes 

> $\operatorname{Pr}(Y=K \mid X=x)=\frac{1}{1+\sum_{l=1}^{K-1} e^{\beta_{l 0}+\beta_{l 1} x_{1}+\cdots+\beta_{l p} x_{p}}}$

and,

> $\operatorname{Pr}(Y=k \mid X=x)=\frac{e^{\beta_{k 0}+\beta_{k 1} x_{1}+\cdots+\beta_{k p} x_{p}}}{1+\sum_{l=1}^{K-1} e^{\beta_{l 0}+\beta_{l 1} x_{1}+\cdots+\beta_{l p} x_{p}}} \text{for } k = 1, \dots, K-1$

The interpretation of the coefficients is tied to the choice of the baseline.

---

# Bayes Classifier

Suppose that we have a qualitative response variable $Y$ with $K$ distinct and ordered classes. The Bayes classifier use a less direct approach using Bayes' theorem to estimating the probabilities

> $\operatorname{Pr}(Y=k \mid X=x)=\frac{\pi_{k} f_{k}(x)}{\sum_{l=1}^{K} \pi_{l} f_{l}(x)}$

-   $\pi_k$ is the **prior** probability that a random observation belongs to the $k$th class.

    -   estimated as the fraction of the training observation that belong to the $k$th class.

-   $f_{k}(X) \equiv \operatorname{Pr}(X \mid Y=k)$ is the **density function** of $X$ for an observation from the $k$th class.

There are several methods we will discuss that attempt to approximate the Bayes classifier using different approaches for estimating $f_k(x)$.

---

# Why Use Bayes Classifier?

-   When there is _**♦️a lot of separation between two classes♦️**_ logistic regression does not does not provide stable coefficient estimates.

-   If the _**♦️distribution of each of the predictors is approximately normal and the sample size is small♦️**_, these approaches are more accurate.

The methods that attempt to estimate the Bayes classifier that we will cover are:

-   Linear discriminant analysis,

-   Quadratic discriminant analysis, and

-   Naive Bayes.

---

# Linear Discriminant Analysis

Suppose we only have one predictor, so $p = 1$. In order to estimate $f_k(x)$ we make the following assumptions:

-   $f_k(x)$ is normal

-   the variance is the same across all $K$ classes.

Linear discriminant analysis (LDA) then approximates the Bayes classifier using the estimates: 
> $\hat{\mu}_{k} =\frac{1}{n_{k}} \sum_{i: y_{i}=k} x_{i} \quad (k \text{th mean)}$

> $\hat{\sigma}^{2} =\frac{1}{n-K} \sum_{k=1}^{K} \sum_{i: y_{i}=k}\left(x_{i}-\hat{\mu}_{k}\right)^{2} \quad \text{(variance)}$

> $\hat{\pi}_{k}=n_{k} / n \quad (k \text{th prior probability)}$

where $n$ = number of training observations, and $n_k$ = number of observations in the $k$th class.

---

# Linear Discriminant Analysis

The LDA classifier uses the estimates for $\pi_{k}, \mu_{k}$, and $\sigma^{2}$ to assign an observation $X = x$ to the class that has the largest 
> $
\hat{\delta}_{k}(x)=x \cdot \frac{\hat{\mu}_{k}}{\hat{\sigma}^{2}}-\frac{\hat{\mu}_{k}^{2}}{2 \hat{\sigma}^{2}}+\log \left(\hat{\pi}_{k}\right)
> $

---

# Linear Discriminant Analysis for $p>1$

Now suppose we have $X = (X_1, \dots, X_p)$ predictors. Assumptions: $X$ is multivariate Gaussian (i.e. each predictor is normally distributed with some correlation between them)

-   class-specific mean vectors

-   common covariance matrix across classes.

We classify observations to the class for which $\hat{\delta}_{k}(x)$ is the largest.

---

# Binary Classifiers

Binary classifiers, similarly to such as tests for diseases (positive versus negative), can make two types of errors:

-   Incorrectly assign an individual as positive when they are negative (False positive).

-   Incorrectly assign an individual as negative when they are positive (False negative).

---

# Confusion Matrix

A confusion matrix helps to summarize the two types of errors of binary classifiers. They compare the LDA predictions to the true outcomes of the training observations. In the case of medical tests this looks like:

![](images/05_confusionmatrix.png)

The red text is where the numbers are filled in.

---

# Confusion Matrix

![](images/05_confusionmatrix.png)

-   N and P are the number of actual negatives and positives respectively in the training data.

-   N\* and P\* are the number of predicted negative and positives in the training data.

---

# Threshold

Recall that the Bayes classifier assigns observations to the class for with the posterior probability is the greatest. Since probabilities sum to 1, for a binary classifier this means that a test will come back positive if:


> $\operatorname{Pr}( \text{positive} \mid X=x) > 0.5$

That is, the binary classifier uses a threshold of 50%. Depending on the classification problem, one may want to specify a different threshold level. For example:


> $\text{positive if }\operatorname{Pr}( \text{positive} \mid X=x) > 0.2$

---

# ROC

The ROC (receiver operator characteristics) curve is a method for visualising the errors previously discusses for all possible thresholds.

![](images/05_roc.png)

---

# ROC
-   Performance of a classifier is given by the area under the ROC curve, called the AUC.

-   The larger the AUC the better.

-   Ideal ROC curve is as close to the top left corner as possible.

![](images/05_roc.png)

---

# Exercise: Linear Discriminant Analysis

Open the Classification Exercises R Markdown or Jupyter Notebook file.

-   Go over the "Linear Discriminant Analysis" section together as a class.

-   5 minutes for students to complete the questions from "Linear Discriminant Analysis".

-   Questions should be completed at home if time does not allow.

---

# Quadratic Discriminant Analysis

The Quadratic discriminant analysis (QDA) classifier assumes that:

-   observations are drawn from a class-specific Gaussian distribution

-   each class has its own covariance matrix (unlike LDA)

The QDA uses estimates for the class-specific means ($\mu_k$), covariance matrices ($\boldsymbol{\Sigma}_{k}$), and prior probability ($\mu_k$) to assign an observation $x$ to the class for which


> $
\delta_{k}(x)=-\frac{1}{2}\left(x-\mu_{k}\right)^{T} \boldsymbol{\Sigma}_{k}^{-1}\left(x-\mu_{k}\right)-\frac{1}{2} \log \left|\boldsymbol{\Sigma}_{k}\right|+\log \pi_{k}$

is the largest. Unlike the LDA, the function for $\delta_{k}(x)$ is quadratic which gives the QDA it's name.

---

# LDA vs QDA

When to use the LDA versus the QDA:

-   LDA is better than QDA when there are few training observations since it requires fewer parameters to be estimated.

-   QDA is best when there are many training observations or when the assumption of a common covariance matrix in the LDA is clearly wrong.

---

# Exercise: Quadratic Discriminant Analysis

Open the Classification Exercises R Markdown or Jupyter Notebook file.

-   Go over the "Quadratic Discriminant Analysis" section together as a class.

-   5 minutes for students to complete the questions from "Quadratic Discriminant Analysis".

-   Questions should be completed at home if time does not allow.

---

# Naive Bayes (quantitative)

The naive Bayes classifier assumes: \textit{within each class, the $p$ predictors are independent}. This allows us to disregard any association between the $p$ predictors and gives the form of $f_k(x)$ as 
> $f_{k}(x)=f_{k 1}\left(x_{1}\right) \times f_{k 2}\left(x_{2}\right) \times \cdots \times f_{k p}\left(x_{p}\right)$

where $f_{k j}$ is the density function of the $j$th predictor for observations in the $k$th class.

To estimate $f_{k j}$ using the training data there are several options:

-   For _**♦️quantitative♦️**_ $X_j$:

    -   assume that for each class, the $j$th predictor is drawn from a normal distribution

    -   or estimate it as the fraction of the training observations in the $k$th class that belong to the same histogram bin as $x_j$.

---

# Naive Bayes (qualitative)

The naive Bayes classifier assumes: \textit{within each class, the $p$ predictors are independent}. This allows us to disregard any association between the $p$ predictors and gives the form of $f_k(x)$ as 
> $
f_{k}(x)=f_{k 1}\left(x_{1}\right) \times f_{k 2}\left(x_{2}\right) \times \cdots \times f_{k p}\left(x_{p}\right)
$ where $f_{k j}$ is the density function of the $j$th predictor for observations in the $k$th class.

To estimate $f_{k j}$ using the training data there are several options:

-   For _**♦️qualitative♦️**_ $X_j$:

    -   count the proportion of training observations for the $j$th predictor that belong to each class.

---

# Exercise: Naive Bayes

Open the Logistic Regression Exercises Jupyter Notebook file.

-   Questions should be completed at home if time does not allow.

---

# How to choose the classification method

The choice of classification method depends on two things:

-   the true distribution of the predictors in each of the $K$ classes, and

-   the number of training observations ($n$) compared to to the number of predictors ($p$).

---

# References

Chapter 4 and section 2.2.3 of the ISLP book:

James, Gareth, et al. "Classification." An Introduction to Statistical Learning: with Applications in Python, Springer, 2023.
