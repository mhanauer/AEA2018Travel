---
title: "Week 6"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Now we are going to be talking about t-tests.  Why use a t-test instead of a z-test?  First, because z-tests assume that you know the actual standard deviation, which we almost never have.  Second and more importantly, it is more robust to outliers and thus non-normality.  Also, after about 100 people, the t and z are approximately the same.  T-tests use degrees of freedom, which are the number of items that are free to vary.  For example, if you have an average of 10 with three numbers and two numbers are 0 and 20, then you know that the last number is 10.  So for every sample statistic that you want to estimate you lose a degree of freedom (might be slightly different for more complex cases, but this is a general principle).  

Let's generate some data to work with:
```{r}
set.seed(123)
fideility = rnorm(100, 50, 2)
fideilityNonNorm = rbind(rnorm(50, 90, 2), rnorm(50, 40, 20))
PHQ9Pre = round(rnorm(100, 50, 4),0)
PHQ9Post = round(rnorm(100, 40, 4), 0)
genderSamp = c(1,0)
gender = as.factor(sample(genderSamp, 100, prob = c(.5, .5), replace = TRUE))
```
What are the assumptions of a t-test
Unless paired, the two variables are independent
Variances of the two variables are equal (can use Welch's if not)
Variables are normally distributed
Data are collected from a simple random sample (no fancy sampling schemes)

One-sample t-test equation:https://www.google.com/search?newwindow=1&biw=1058&bih=778&tbm=isch&sa=1&ei=6iBTW-vfFOKOggeJraT4CQ&q=t-test+one-sample+equation&oq=t-test+one-sample+equation&gs_l=img.3..0i8i30k1.4770.9156.0.9383.20.19.0.0.0.0.350.2282.0j10j3j1.14.0....0...1c.1.64.img..6.14.2275...0j0i30k1.0.LjmbLBiCfwk#imgrc=5kLVLXBvT0gNSM:

One sample t-test.  Let us assume that a fidelity score of 50 or higher means the person is exhibiting "fidelity".  If we are looking at higher scores then should this be a one-tailed or two-tailed test and why?
```{r}
t.test(fideility, alternative = "greater", mu = 50)
```
So now let's try a two-sample test.  What's wrong with this test?
```{r}
t.test(PHQ9Post, PHQ9Pre)
```
Yes, these are pre and post scores from PHQ9Pre and PHQ9Post so we need to account for the relationship between the two.
```{r}
t.test(PHQ9Post, PHQ9Pre, paired = TRUE)
t.test(PHQ9Post, PHQ9Pre, paired = TRUE, alternative = "less")
```
Let us go back to fideility.  Maybe we are interested in understanding whether gender if affecting fideility.  
```{r}
t.test(fideility~gender)
```
The non-parametric version of a t-test Wilcox Rank Sum Test

Assumptions
Variables are independent (unless paired)
Data are at least ordinal (not binary)
Equal vars (unless Welchs)

Example: Let us assume that we want to understand differences in fidelity among gender.  Maybe we have a lot of people who are really good and then a lot who are ok.  So here is the ugly distribution below. 
```{r}
hist(fideilityNonNorm)
```
So the t-test will not work for this distribution.  So we need the Wilcox rank summed test.  In the fidelity example, it will mean looking at all the values in both groups, then ranking them.  For example, if one male has the score of 100 and that is the highest score, that person will receive a one.  Then the person regardless of gender who has the second highest score will receive a two.  If there is a tie, then the average of the rank is applied to both.  The sum of the ranks is added for the two groups and then we evaluate if these sums are statistically significant from each other.

Drawbacks: These tests are less powerful, because they are using less data.  When we use the actual values instead of the ranks we are getting more information so therefore we have more certainty.  

See here for more details: http://sphweb.bumc.bu.edu/otlt/mph-modules/bs/bs704_nonparametric/BS704_Nonparametric4.html

Fidelity by gender, paired, and alternative all the same
```{r}
wilcox.test(fideility~gender)
wilcox.test(PHQ9Post, PHQ9Pre, paired = TRUE)
wilcox.test(PHQ9Post, PHQ9Pre, paired = TRUE, alternative = "less")
```
Next week correlation, correlation with binary and ordinal variables, maybe bivariate regression

