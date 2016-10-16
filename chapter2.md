---
title       : OLS Basics 
description : This section contains exercises dealing with the simple linear regression model and estimation using ordinary least squares. 
attachments :
  slides_link : https://github.com/Emwikts1970/URFITE_DC/raw/master/Econometrics

--- type:NormalExercise lang:r xp: skills: key:9d4515394d
## Class Sizes and Test Scores

A researcher wants to analyse the relationship between class size and pupils' average test score. Therefore he measures both variables in 10 different classes obtaining the following results:

  <table>
      <tr>
        <td><b>Class Size</b></td>
        <td>23</td>
        <td>19</td>
        <td>30</td>
        <td>22</td>
        <td>23</td>
        <td>29</td>
        <td>35</td>
        <td>36</td>
        <td>33</td>
        <td>25</td>
      </tr>
      <tr>
        <td><b>Test Score</b></td>
        <td>430</td>
        <td>430</td>
        <td>333</td>
        <td>410</td>
        <td>390</td>
        <td>377</td>
        <td>325</td>
        <td>310</td>
        <td>328</td>
        <td>375</td>
      </tr>
    </table>


*** =instructions

- Create vectors, `cs` for class sizes and `ts` for test scores, containing the obeservations above.
- Draw a scatter plot of the results using `plot`.
Use <b>*existing*</b> R functions for the following exercises.
- Compute mean, median and variance and standard deviation of test scores.
- Compute both the covariance and pearson's correlation coefficient for `cs` and `ts`.

*** =sample_code
```{r}
# Create both vectors


# Draw the scatter plot


# Compute mean, median, variance & standard deviation of test score


# Compute the covariance and the correlation coefficient


```

*** =solution
```{r}
# Create both vectors
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)

# Draw the scatter plot
plot(cs,ts)

# Compute mean, median, variance & standard deviation of test score
mean(ts)
median(ts)
var(ts)
sd(ts)

# Compute the covariance and the correlation coefficient
cov(cs,ts)
cor(cs,ts)
```

*** =sct
```{r}
test_object("cs")
test_object("ts")

test_function("plot")

test_function("mean")
test_function("median")
test_function("var")
test_function("sd")
test_function("cov")
test_output_contains("cov(cs,ts)")
test_function("cor")
test_output_contains("cor(cs,ts)")
```

--- type:MultipleChoiceExercise lang:r xp: skills: key:2ad42e1384
## A Linear Relationship? 

Have a look at the scatter plot again (we made it a little bit more fancy here) and remember the result of the `cor()` function

```{r}
> cor(cs,ts)
[1] -0.9474424 
```
providing the empirical correlation coefficient of class size and test score.

Which of the following statements describes best the relationship between both variables?

*** =instructions

- The relationship is negative, linear and strong. There are no outliers.
- Both variables relate in a strongly nonlinear manner of unkown kind.
- The relationship is positive, linear and weak. There are outliers.
- It seems that $ClassSize = e^{TestScore}$.

*** =hint

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
plot(cs,ts, pch=20, cex=1.8, col="steelblue", ylab="Test Score", xlab="Class Size", main="Scatter Plot")
```

*** =sct
```{r}
msg_bad <- "That is not correct!"
msg_success <- "Exactly! Remember that the empirical correlation between both variables is about $-0.95$ indicating a strongly negative linear relation. The scatter plot delivers visual evidence. Furthermore, there are no outliers."
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp: skills: key:54c5e502ca
## The Linear Model 

In this exercise, you will learn how to estimate a simple linear regression model.

In R, linear models can be fitted by use of the `lm()` function. 

An argument to be always specified in `lm()` is a regression formula of the form `y ~ x`. This expression states that you want to estimate a linear model of `y` as a function of `x`.  Type and execute `?lm` or `?formula` for further info on subjects.

*Data vectors from the previous exercise is available in your workspace.*

*** =instructions
- Estimate a linear regression of test score on class size using `lm()`. Store the result in `mod`.
- `mod` is an object of type `list` with named entries. Check this using the function `is.list()`.
- See what information you can obtain from `mod` using the `$` operator. Read out an arbitrary property of the object `mod`.


*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
```

*** =sample_code
```{r}
# Estimate the regression model and store it in mod


# Check that mod is an object of type list 


# Read out some arbitrary entry of mod using. E.g. fitted values:


```

*** =solution
```{r}
# Estimate the regression model and store it in mod
mod <- lm(ts ~ cs)

# Check that mod is an object of type list 
is.list(mod)

# Read out some arbitrary entry of mod using. E.g. fitted values:
mod$fitted.values
```

*** =sct
```{r}
test_or(
  test_function("lm", eq_condition = "equal", not_called_msg = "You didn't call `lm()`!",
              incorrect_msg = "You didn't call `lm()` with the correct argument, `formula`."),
  test_function("lm", eq_condition = "equivalent", not_called_msg = "You didn't call `lm()`!",
              incorrect_msg = "You didn't call `lm()` with the correct argument, `formula`.")
)

test_object("mod")

test_function("is.list", args="x")

test_student_typed("mod$")
```

--- type:NormalExercise lang:r xp: skills: key:6e64f67c78
## Model Objects

Consider again the relation of class size and test score. The model object `mod` from the previous exercise is available in your workspace. This means you can use it for subsequent tasks. Convince yourself by typing `summary(mod)` in to the console and get yet again detailed information on the estimated model!



*** =instructions

- Obtain an overview over the model object `mod` using `summary()`
- Add the regression line to the scatterplot. Hint: use `abline()`
- Store the regression summary into `summary`. Discover what information you can extract from `summary` by use of the `$` operator
- Suppose you are interested in how good the model fits the data. Create a new variable `R2` storing the regression's $R^2$
- You can extract a named $2\times4$ matrix with, amongst other things, estimated coefficients and standard errors. Save this matrix to an object named `coef`

*** =hint

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
mod <- lm(ts ~ cs)
```

*** =sample_code
```{r}
# Use summary() the get an overview over your model


# Add the regression line the the scatterplot
plot(cs,ts)

# Store summary(mod) into summary


# Store the regression's R^2 in R2


# Save the coefficient matrix to coef
```

*** =solution
```{r}
# Use summary() the get an overview over your model
summary(mod)

# Add the regression line the the scatterplot
plot(cs,ts)
abline(mod)

# Store summary(mod) into summary
summary <- summary(mod)

# Store the regression's R^2 in R2
R2 <- summary$r.squared

# Save the coefficient matrix to coef
coef <- summary$coefficients

```

*** =sct
```{r}
test_predefined_objects("mod")
test_function("summary")
test_function("abline")
test_object("summary")
test_object("R2")
test_object("coef")
```

--- type:NormalExercise lang:r xp: skills: key:449f4c19de
## Dropping the Intercept

So far, You have conducted regressions where the model consisted of an intercept and another regressor. In this exercise you will learn ways to specify and to estimate regression models without the intercept.

Notice that excluding the intercept from Your regression model might be a dodgy practice in some models as You impose the mean of the dependend variable to be zero if all other regressors are zero.

*Vectors `cs` and `ts` as well as the list object `mod` from previous exercises are availabe in your working environment.*


*** =instructions

- Use the help function on `lm` (`?lm`) to find out how to specify a `formula` for a regression of `ts` solely on `cs`, i.e. a regression witouh intercept
- Estimate the regression model without intercept and store it in `mod_ni`
- Convince yourself that everything went right. Extract the coefficient matrix from the models summary and store it to `coef`
- Plot both regression lines

*** =hint

- In `lm()` use `ts ~ cs - 1` as the `formula` argument to estimate a regession model lacking an intercept

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
mod <- lm(ts ~ cs)
```

*** =sample_code
```{r}
# Regress ts solely and cs, sore it to mod_ni


# Extract the coefficient matrix from the models summary, save it to coef


# Plot regression lines for both models
plot(cs,ts)

```

*** =solution
```{r}
# Regress ts solely and cs, sore it to mod_ni
mod_ni <- lm(ts ~ cs - 1) # or: lm(ts ~ cs + 0)

# Extract the coefficient matrix from the models summary, save it to coef
coef <- summary(mod_ni)$coefficients

# Plot regression lines for both models
plot(cs,ts)
abline(mod)
abline(mod_ni)

```

*** =sct
```{r}
test_predefined_objects("mod")

test_correct(test_function("lm"),
    {
    test_student_typed("ts ~ cs + 0")
    }
)

test_function("lm")

test_object("mod_ni", eval=F)
test_object("coef")

test_function("abline", index=1)
test_function("abline", index=2)
```

--- type:NormalExercise lang:r xp: skills: key:673ab4d8fc
## The R^2 of a Regression Model  

We now go back to the simple model including an intercept. The estimated regression line for `mod` was

$$ \widehat{TestScore} = 567.43 - 7.15 \times ClassSize, \, R^2 = 0.8976, \, SER=15.19 $$

*You can check this as `mod` and vectores `cs` and `ts` are available in your working environment again.*

*** =instructions
- Compute $SSR$, the sum of squared residuals, and save it to `ssr`
- Compute $TSS$, the total sum of squares, and save it to `tss`
- Use `ssr` and `tss` to compute $R^2$, the coefficient of determination. Save it to R2
- Use the logical expression `==` to check whether your result for $R^2$ equals the one mentioned above

*** =hint

The solutions can be found in several ways, e.g. using formulas and/or knowledge about the structure of `lm` objects. Don't forget about the `$` operator!

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
mod <- lm(ts ~ cs)
```

*** =sample_code
```{r}
# Compute the SSR and save it to ssr


# Compute the TSS and save it to tss


# Compute R^2 and save it to R2


# Check whether your result is correct


```

*** =solution
```{r}
# Compute the SSR and save it to ssr
ssr <- sum(mod$residuals^2)

# Compute the TSS and save it to tss
tss <- 9*var(ts) # var() computes the unbiased sample variance! => Correct by multiplying with n-1 = 9

# Compute R^2, round it by 4 decimal places and save it to R2
R2 <- round(1-ssr/tss,4)

# Check whether your result is correct
R2 == 0.8976
```

*** =sct
```{r}
test_object("ssr")
test_object("tss")
test_object("R2")

test_predefined_objects("mod")

test_function("round", args="digits", eq_condition="equal")

test_student_typed("R2 == 0.8976", not_typed_msg = "Something is wrong. Make sure You type `R2 ==` followed by the value for R^2 mentioned above.")

test_output_contains("R2 == 0.8976")
```

--- type:MultipleChoiceExercise lang:r xp: skills: key:c0b8375eb8
## What was R^2 again?  

The coefficient of determination, denoted $R^2$, is a measure of a model's goodness of fit. It is defined as

$$ R^2 = \frac{ESS}{TSS} = 1 - \frac{SSR}{TSS}. $$

Where $TSS = \text{total sum of squares}$, $ESS = \text{explained sum of squares}$ and $TSS = \text{sum of squared residuals}$.

Which of the following statements about $R^2$ is right?

*** =instructions

- The coefficient of determination can only be computed for linear regression models.
- A model's $R^2$ shrinks if the absolute value of the residual sum decreases.
- The coefficient of determination is always defined on $[-1,1]$. 
- $R^2$ is a measure indicating the proportion of the variance in the dependent variable that is explained by the independent variable(s).


*** =sct
```{r}
msg_bad <- "That is not correct!"
msg_success <- "Wow!"
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success))
```



--- type:MultipleChoiceExercise lang:r xp: skills: key:01c813746a
## Recap: The P-Value 

What was the definition of the $p$-value again? 

Maybe the plot displayed on the right may help You to remember. It depicts the case of a two-sided test where $|t|$ is the computed test statistic.

Choose the *right* statement.

*** =instructions

- The $p$-value is the proportion of cases for which we can reject the null hypothesis.
- The $p$-value is a measure for the probability that the null hypothesis is true.
- The $p$-value is defined as the probabilty of observing a result that is at least as extrem as the observed result, provided the null hypothesis is true. If the $p$-value is less or equals the choosen level of significance, the null is rejected.
- The $p$-value has to be choosen before conducting a significance test. If the t-statistic exceeds the $p$-value, the null hypothesis is not rejected.

*** =hint


***=pre_exercise_code
```{r}
z <- seq(-6,6,0.01)
tact <- 1.3
plot(z, dnorm(z,0,1), type = "l", col="steelblue", lwd=2, yaxs="i", bty = "n", axes=F, ylab = "", cex.lab=0.7)
axis(1, at = c(0,-tact,tact), cex.axis=0.7)

# Shade the critical regions
polygon(c(-6,seq(-6,-tact,0.01),-tact),c(0,dnorm(seq(-6,-tact,0.01)),0),col='orange')
polygon(c(tact,seq(tact,6,0.01),6),c(0,dnorm(seq(tact,6,0.01)),0),col='orange')

# Add ticks indicating critical values at the 0.05-level, t^act and -t^act 
rug(c(-tact,tact), ticksize  = 0.42, lwd = 2, col = "darkred")
text(-3,0.18, labels = "-|t|", cex = 0.7)
text(3,0.18, labels = "|t|", cex = 0.7)
arrows(3,0.16,tact,0, length = 0.1)
arrows(-3,0.16,-tact,0, length = 0.1)
```

*** =sct
```{r}
msg_bad <- "That is not correct!"
msg_success <- "Wow!"
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success))
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:7860286aef
## A Null Hypothesis You Can't Reject... Can You?

Suppose You are confronted with the following regression output providing the t-statistic for the hypothesis $\beta_{cs} = 0$ and the corresponding $p$-value.

```{r}
Coefficients:
          t value   Pr(>|t|)    
cs        -1.1      0.27133
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

Which of the following statements is *false*?

*** =instructions

- The coefficient of `cs` *is not* statistically significantly different from zero at any level of significance used in practice.
- We can reject the hypothsis at the $0.05$ level of significant since $p\text{-value} > 0.05$.
- The value of the t-statistic is not an element of the set of values for which the null would be rejected.
- Given the info above, it is not possible to say whether the null is rejected.

*** =sct
```{r}
msg_bad <- "No, that is not correct!"
msg_success <- "Exactly!"
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp: skills: key:4d6c0982cd
## <<<TITLE>>> 

Now consider again the estimated regression line

$$ \widehat{TestScore} = \underset{(23.9606)}{567.43} - \underset{(0.8536)}{7.15} \times Cs, \, R^2 = 0.8976, \, SER=15.19 $$

with standard errors in parantheses.

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}

```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:13d4cf0fb6
## Interpreting OLS Regressions I

Suppose that a researcher, using data on class size $CS$, and average test score, $TestScore$, from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

<i>Say a class room has 25 students. What is the regression's prediction for that classroom's average test score? <br> Don't forget: You can use the R console as a calculator!</i>

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
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:50 skills:1 key:b0b7650bb3
## Interpreting OLS Regressions II

Suppose that a researcher, using data on class size, $CS$, and average test score, $TestScore$, from 50 third-grade classes, estimates the OLS regression:

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

Suppose that a researcher, using data on class size $CS$, and average test score, $TestScore$, from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

*** =instructions
The sample average class size across 50 classrooms is 22.8. What is the sample average of the test scores across the 50 classrooms?

*** =hint

Review the formulas for the OLS estimators!

*** =sample_code
```{}
# What is the sample average of the test score across the 50 classrooms?


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

Suppose that a researcher, using data on class size $CS$, and average test score, $TestScore$, from 50 third-grade classes, estimates the OLS regression:

$$ \widehat{TestScore} = 640.3 - 4.93 \times CS, R^2 = 0.11, SER= 8.7 $$

*** =instructions
What is the sample standard deviation of test scores across the 50 classrooms?

Hint: Remember Your regression formulas! For correct evaluation of this exercise, it suffices if You print the solution to the console.

*** =hint

Review the formulas for the $R^2$ and $SER$ 

$ R^2 = \frac{SSR}{TSS} $

$ SER = \sqrt{\frac{SSR}{n-2}} $

and

$ SSR = \sum_{i=1}^n u^2 $

*** =sample_code
```{r}
# What is the sample standard deviation of test scores across the 50 classrooms?


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

--- type:NormalExercise lang:r xp:100 skills:1 key:c4b2c27865
## Regression when X is a Dummy Variable I

Instead of using the regressor $CS$, we might be interested in running a regression where the regressor, $D$ say, is binary variable or so-called dummy variable. 

For example, we may define $D_i$ in the following way:

$$ D_i = \begin{cases} 1 \ \ \text{if $CS$ in the $i^{th}$ class < 26} \\\\ 0 \ \ \text{if $CS$ in the $i^{th}$ class $\geq$ 26} \end{cases} $$

Using R, one way to do this is as follows:

```{r}
# Define an empty vector D
D <- c()
# Assign values accordingly using a loop
for (i in 1:length(cs)) {
  if (cs[i] < ???) { 
    D[i] <- ???
    } else {
      D[i] <- 0
    }
  }
```

Notice that the code above contains two gaps indicated by `???`. Can you replace them with the right expressions?

<br>
*Vectors `cs` and `ts` are availabe in your working environment.*
<br>

***=instructions

- Replace the `???` and create the dummy regressor `D` using the proposed loop above.
- Convince Yourself that `D` is a vector. Check its length.
- Estimate a regression of $cs$ on $D$.
- Call summary on Your model. Think about how the estimated coefficients are interpreted!

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
```

***=hint
- Check if an object is a vector using the function `is.vector()`
- An object's length can be checked with `length`

***=sample_code
```{r}
# Create D


# Convince yourself that D is a vector. Check its length.


# Regress ts on D and constant


# Inspect the your model using summary
```

***=solution
```{r}
## Create D
# Define an empty vector D
D <- c()
# Assign values accordingly using the loop
for (i in 1:length(cs)) {
  if (cs[i] < 26) { 
    D[i] <- 1
    } else {
      D[i] <- 0
    }
  }

# Convince yourself that D is a vector and Check its length
is.vector(D)
length(D)

# Regress ts on D and constant
mod <- lm(ts ~ D)

# Call summary on the model object
summary(mod)
```

*** =sct
```{r}
test_student_typed("for (i in 1:length(cs)) {
  if (cs[i] < 26) { 
    D[i] <- 1
    } else {
      D[i] <- 0
    }
  }", not_typed_msg="Make sure You use the loop proposed above. If you have done so, check Your replacement for both `???`.")
test_object("D")
test_function("length")
test_function("is.vector")
test_function_result("lm")
test_function("summary")
```

--- type:MultipleChoiceExercise lang:r xp: skills: key:b496c011e5
## Interpretation When X is a Dummy Variable 

Consider again the dummy regression model from the previous exervise. The estimated regression equation was:

$$ \widehat{TestScore} = 334.60 + 72.40 \times D $$

*The model object is available in your workspace (`mod`)*

<br>
Which of the following statements is *wrong*?

*** =instructions

- The intercept can be interpreted as the estimated average test scrore among classes with at least 26 pupils. 
- Increasing the class size by one unit leads to an estimated increase of 72.40 in test score on average.
- The estimated average test score in classes with less than 26 pupils is 407.
- The regression equation indicates that test score is higher for smaller class sizes.

*** =hint

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
D <- c(1,1,0,1,1,0,0,0,0,1)
mod <- lm(ts ~ D)
```

*** =sct
```{r}
msg_bad <- "Sorry, this statement is correct..."
msg_success <- "Exactly! Remember: The regressor $D$ is a dummy variable! <br> The intercept, $334.60$, can be interpreted as the estimated average test scrore among classes with at least 26 pupils. For classes with less than 26 pupils, the dummy $D$ switches to one such that the estimated average test score in classes with less than 26 pupils is $334.60 + 72.40 = 407$. <br> This indicates that test score is higher for smaller class sizes. <br> Good Job!"
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```



--- type:NormalExercise lang:r xp: skills: key:2bb531e9cd
## Regression when X is a Dummy Variable 

In this exercise, you will visualize some of the results from the previously conducted dummy regression. 

$$ \widehat{TestScore} = 334.60 + 72.40 \times D $$

Start by drawing a visually appealing plot of observations based on the following code chunk:

```{r}
plot(x = ??? , y = ???, 
     pch=20, cex=1 ,col='Steelblue',
     xlab=expression(D[i]), ylab='Test Score',
     main = 'Dummy Regression'
     )"
```

Can you replace the `???` with correct arguments?

<br>
*Vectors `D` and `ts` as well as the model object `mod` are available in your workspace.*

*** =instructions

- Draw a scatter plot of observations using the code snippet above. Replace the `???` with the correct expressions!
- Add the regression line to the plot using `abline()`.
- Draw in the group specific means using `points()`. Orientate on the snippet provided in `script.R`. What has to be changed to also get the mean test score for classes with size < 26?

*** =hint

*** =pre_exercise_code
```{r}
cs <- c(23, 19, 30, 22, 23, 29, 35, 36, 33, 25)
ts <- c(430, 430, 333, 410, 390, 377, 325, 310, 328, 375)
D <- c(1,1,0,1,1,0,0,0,0,1)
mod <- lm(ts ~ D)
```

*** =sample_code
```{r}
# Plot the data




# Add the regression line


# Add the mean test score for classes with size >= 26 to the plot
points(x = 0, y = mean(ts[cs>=26]), col = "red", pch=20, cex=1.8)

# Add the mean test score for classes with size < 26 to the plot


```

*** =solution
```{r}
# Plot the data
plot(x = D , y = ts, 
     pch=20, cex=1 ,col="Steelblue",
     xlab=expression(D[i]), ylab="Test Score",
     main = "Dummy Regression"
     )

# Add the regression line
abline(mod)

# Add the mean test score for classes with size >= 26 to the plot
points(x = 0, y = mean(ts[cs>=26]), col = "red", pch=20, cex=1.8)

# Add the mean test score for classes with size < 26 to the plot
points(x = 1, y = mean(ts[cs<26]), col = "red", pch=20, cex=1.8)
```

*** =sct
```{r}
test_function("plot", args=c("x","y"))
test_predefined_objects("mod")
test_function("abline")
test_function("points", index = 1, args=c("x","y"))
test_function("points", index = 2, args=c("x","y"))
success_msg("Great!")
```