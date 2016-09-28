---
title       : OLS Basics 
description : This section contains exercises dealing with the simple linear regression model and estimation using ordinary least squares. 
attachments :
  slides_link : https://github.com/Emwikts1970/URFITE_DC/raw/master/Econometrics

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:35167f113b
## Violation of OLS Assumptions

Have a look at the plot that showed up in the viewer to the right. Which of the OLS assumptions could be possibly violated?

*** =instructions
- No perfect multicollinearity
- Homoskedasticity
- Outliers are seldom
- Y is $\chi^2_{11}$ distributed

*** =hint
Have a look at the plot. What can you say about the dispersion of observations?

*** =pre_exercise_code
```{r}
library(sandwich)

set.seed(1)
x <- runif(500, 0, 1)
y <- 5 * rnorm(500, x, x)
plot(y ~ x, col = "steelblue", pch = 19)
```

*** =sct
```{r}
msg_bad <- "That is not correct!"
msg_success <- "Exactly! There seems to be heteroskedasticity: Dispersion increases with x."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:d71e82b5ef
## Regression and Robust Standard Errors

In the previous exercise, you saw example data exhibiting heteroskedasticity. In this exercise, we'll have a look at how 
we can get a regression summary reporting robust standard errors.

A data set consisting of observations you have seen before is now available in the workspace.

*** =instructions
- Regress `y` on `x` and a constant. Store the result in `mod`
- Report coefficients and robust standard errors.
- Plot the regression line.

*** =hint
- Use `lm()` for the first instruction.
- Use the `coeftest()` function. See how you can specify a robust variance-covariance estimator to be used. coeftest has an argument were this can be specified. A look at the help file might be useful: `?coeftest`.
- For the plot, use `abline()` on your model object.

*** =pre_exercise_code
```{r}
library(sandwich)
library(AER)
set.seed(1)
x <- runif(500, 0, 1)
y <- 5 * rnorm(500, x, x)
```

*** =sample_code
```{r}
# Data vectors `x` and `y` are now available in your workspace.


# Regress y on x. Store the model in `mod`.


# Report coefficients and robust standard errors.


# Add the regression line to the plot created by the code below.
plot(y ~ x, col = "steelblue", pch = 19)


```

*** =solution
```{r}
# Data vectors `x` and `y` are now available in your workspace.


# Regress y on x. Store the model in `mod`.
mod <- lm(y~x)

# Report coefficients and robust standard errors.
coeftest(mod, vcov.=vcovHC(mod, type="HC0"))

# Add the regression line to the plot created by the code below.
plot(y ~ x, col = "steelblue", pch = 19)
abline(mod)
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_function("lm", args = "formula",
              not_called_msg = "You didn't call `lm()`!",
              incorrect_msg = "You didn't call `lm()` with the correct argument, `formula`.")

test_object("mod")

test_function("coeftest", args = c("x","vcov."))
test_function("abline")


test_error()

success_msg("Good work!")
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:13d4cf0fb6
## Interpreting OLS Regressions I

Suppose that a researcher, using data on class size $CS$ and average test scores from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

<i>Say a class room has 25 students. What is the regression's prediction for that classroom's average test score? Don't forget: You can use the R console as a calculator!</i>

*** =instructions

- 517.05
- 1337
- 0
- 1

*** =hint

Have a look at the regression equation. How can you interpret the relation on the right hand side?

*** =sct
```{r}
msg_bad <- "That is not correct!"
msg_success <- "Exactly!"
msg_joke <- "LoL, your definitely not 1337!"
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_joke, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:50 skills:1 key:b0b7650bb3
## Interpreting OLS Regressions II

Suppose that a researcher, using data on class size $CS$ and average test scores from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

*** =instructions
Last year a classroom had 21 students, and this year it has 24 students. What is the regression's prediction for the change in the classrom average test score?

*** =hint

Have a look at the regression equation. How can you interpret the relation on the right hand side? What does this imply if you got test scores for two different class sizes?

*** =sample_code
```{}
# What is the difference in average test score?


```

*** =solution
```{r}
# What is the difference in average test score?
(640.3 - 4.93 * 24) - (640.3 - 4.93 * 21)

```

*** =sct
```{r}
test_output_contains("(640.3-4.93*24) - (640.3-4.93*21)", incorrect_msg = "No, that is not correct...")
```

--- type:NormalExercise lang:r xp:50 skills:1 key:250d5774a4
## Interpreting OLS Regressions III

Suppose that a researcher, using data on class size $CS$ and average test scores from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

*** =instructions
The sample average class size across 50 classrooms is 22.8. What is the sample average of the test scores across the 50 classrooms?

*** =hint

Review the formulas for the OLS estimators!

*** =sample_code
```{}
# What is sample average of the test score across the 50 classrooms?


```

*** =solution
```{r}
# First, define:
beta_hat_0 <- 640.3
beta_hat_1 <- -4.93
avg_cs <- 22.8

# Using the OLS formula for beta_hat_0:
avg_ts <- beta_hat_0 + beta_hat_1 * avg_cs
avg_ts
```

*** =sct
```{r}
test_output_contains("527.896", incorrect_msg = "Not correct...")
```

--- type:NormalExercise lang:r xp:50 skills:1 key:39aef8ac4c
## Interpreting OLS Regressions IV

Suppose that a researcher, using data on class size $CS$ and average test scores from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

*** =instructions
What is the sample standard deviation of test scores across the 50 classrooms?

*** =hint

Review the formulas for the $R^2$ and $SER$ 

$ R^2 = \frac{SSR}{TSS} $

$ SER = \sqrt{\frac{SSR}{n-2}} $

and

$ SSR = \sum_{i=1}^n u^2 $

*** =sample_code
```{r}
# What is sample standard deviation of test scores across the 50 classrooms?


```

*** =solution
```{r}
# Define:
SER <- 8.7
R2 <- 0.11
n <- 50
K <- 2

# Using the formulas:

SSR <- SER^2 * (n-K)
TSS <- SSR/R2 

# Hence, using the forumula for sample standard deviation:

sigma_hat <- sqrt(1/(n-1) * TSS)

# Print it

sigma_hat
```

*** =sct
```{r}
test_output_contains("sigma_hat", incorrect_msg = "Something's wrong... Did you use the forumula for sample standard deviation?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2d231a7828
## Inference in the Simple Regression Model

--- type:NormalExercise lang:r xp:100 skills:1 key:c4b2c27865
## Dummy Variables



--- type:NormalExercise lang:r xp:100 skills:1 key:79d4a98b65
## Heteroskedasticity I

In this block of exercises we will use the `cars` data set to explore ways to detect and to deal with heteroskedasticity when estimating simple linear models.

We will discuss two ways of detecting heteroskedasticity:

- Graphical inspection
- Statistical tests

We have already loaded the `AER` package for You.

*** =pre_exercise_code
```{r}
library(AER)
```

*** =instructions
- Get an overview over the data set using `summary()`
- Plot speed (`speed`) against distance (`dist`)
- Estimate the model $dist = \beta_0 + \beta_1 \times speed$. Store the restut in `mod`

*** =sample_code
```{r}
# Get an overview over the data


# Plot speed against distance


# Estimate the model


```

*** =solution
```{r}
# Get an overview over the data
summary(cars)

# Plot speed against distance
plot(cars$speed, cars$dist)

# Estimate the model
mod <- lm(cars$dist ~ cars$speed)

```

*** =sct
```{r}
test_function_result("summary")
test_function("plot", args=c("x","y"))
test_or(
  test_function("lm", eq_condition = "equal"),
  test_function("lm", eq_condition = "equivalent")
)
test_object("mod", eval=F)
success_msg("You are doing great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:324d782dca
## Heteroskedasticity II

The object `mod` from the previous exercise is available in Your workspace. 


*** =pre_exercise_code
```{r}
library(AER)
plot(cars$speed, cars$dist)
mod <- lm(cars$dist ~ cars$speed)
```

*** =instructions

- Add the regression line for the model `mod` to the plot

By means of simply plotting the data, it is not always easy to decide whether there is heteroskedasticity or not, especially if your data set has more than two variables. Here, it seems that there is more dispersion in `dist` for observations around the mean of `speed`. Let us have a closer look:

- Applying `plot()` to a model model object like `model` produces a whole battery of diagnostic plots. Check this (Your can navigate through the different plots using the buttons).

An indicator for heteroskedasticity is dependence of residuals on the level of fitted values.

- See how fitted values relate to residuals: `plot(mod,1)`

*** =sample_code
```{r}
# Add the regression line for the model mod to the plot
plot(cars$speed, cars$dist)

# Call plot() on your model


# See how fitted values relate to residuals


```

*** =solution
```{r}
# Add the regression line for the model mod to the plot
plot(cars$speed, cars$dist)
abline(mod)

# Call plot() on your model
plot(mod)

# See how fitted values relate to residuals
plot(mod,1)
```

*** =sct
```{r}
test_function("abline")
test_function("plot", index=1)
test_function("plot", index=2)
test_student_typed("plot(mod,1)", not_typed_msg = "Make sure to call the plot as proposed in the instruction.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2905f44fda
## Heteroskedasticity III

A formal test for heteroskedasticity was proposed by Breusch & Pagan in 1979. This test checks for heteroskedasticity by fitting a linear model to regression residuals and then tests if regressors are significant in explaining observed variance in the residuals. The null hypothesis is no heteroskedasticity.
An R implementation can be found in the package `lmtest`. The function is named `bptest`.

*** =instructions
```{r}
The object `mod` from the previous exercise is available in Your R session. 

- Load the `lmtest` package
- Check the help file entry for `bptest`
- Conduct the Breusch-Pagan test
```

*** =pre_exercise_code
```{r}
# Load the `lmtest` package


# Check the help file entry for `bptest`


# Conduct the Breusch-Pagan test
```

*** =solution
```{r}
# Load the `lmtest` package
library(lmtest)

# Check the help file entry for `bptest`
?bptest

# Conduct the Breusch-Pagan test
bptest(mod)
```

*** =sct
```{r}
test_student_typed("library(lmtest)")
test_student_typed("?bptest")
test_function("bptest", args = "formula", eq_condition = "equal")
```


--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:726a6d460b
## The Gauss Markov Theorem I

Suppose you got the regression model

# Not Working

$$ y_i=\beta_0 $$

# Working

$$ \chi^2_{12} $$

In the plotting area on the right you see the result of a Monte Carlo simulation analysing distributional properties of the OLS estimator for $ \beta $ in the model above and another linear estimator $\overset{\sim}{\beta}$ which uses different weights than OLS. Say, $\beta=0$. Is the result consistent with what you expect beeing aware of the Gauss-Markov Theorem?  

*** =instructions
- Yes, both estimators seem to be unbiased but the OLS estimator has less dispersion.
- No, the distribution of $\overset{\sim}{\beta}$ looks more like a standard normal distribution.
- Cannot be answered without prior inspection of data.

*** =pre_exercise_code
```{r}
# Set sample size and number of repititionas

n <- 100      
reps <- 1e5

# Choose epsilon and create a vector of weights as defined above

epsilon <- 0.8
w <- c( rep((1+epsilon)/n,n/2), rep((1-epsilon)/n,n/2) )

# Draw random sample y_1,...,y_N from the standard normal distribution 
# Compute both estimates 1e6 times and store the result in vectors  

ols <- rep(NA,reps)
weightedestimator <- rep(NA,reps)

for (i in 1:reps)
{
  y <- rnorm(n)
  ols[i] <- mean(y)
  weightedestimator[i] <- crossprod(w,y)
}

# Plot estimates of the estimators distribution 

plot(density(ols),col="purple", lwd=3, main="Density of OLS and Weighted Estimator",xlab="")
lines(density(weightedestimator),col="steelblue", lwd=3) 
abline(v=0,lty=2)
legend('topright', c("OLS","weighted"), col=c("purple","steelblue"),lwd=3)
```
*** =sct
```{r}
msg_bad <- "That is not correct!"
msg_success <- "Exactly!"
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:9ad3c5911e
## Introduction to Multiple Regression

