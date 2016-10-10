---
title       : Multiple Regression (test)
description : This section contains exercises dealing with the multiple linear regression model and discusses estimation using ordinary least squares. 
attachments :
  slides_link : https://github.com/Emwikts1970/URFITE_DC/raw/master/Econometrics

--- type:NormalExercise lang:r xp: skills: key:af07cdc945
## Multiple Regression: Boston Housing Data

For the course of this section, we will use the `Boston` data set which contains 506 observations concerning housing values in suburbs of Boston. The data set comes with the package `MASS`.

*** =instructions

- Load the package and the data set
- Get youself an overview over the data set using the `summary` function. Hint: Use `?Boston` for detailed info on variables
- Estimate a simple linear regression model explaining *median house value*, `medv`. by the *percent of household with low socioecononomic status*, `lstat`, and a constant. 
- Inspect the model's summary

*** =hint

You only need basic functions here: `library()`, `data()`, `lm()` and `summary`.

*** =pre_exercise_code
```{r}
library(MASS)
```

*** =sample_code
```{r}
# Load the package and the data set


# Get yourself an overview over the data set


# Estimte the model


# Call summary on Your model


```

*** =solution
```{r}
# Load the package and the data set
library(MASS)
data("Boston")

# Get yourself an overview over the data set
summary(Boston)

# Estimate the model
lm(medv ~ lstat, data = Boston)

# Call summary on Your model
summary(lm(medv ~ lstat, data = Boston))
```

*** =sct
```{r}
test_function("library")
test_function("data")
test_function("summary", index=1, args="object")


test_or({
  fun <- ex() %>% check_function('lm')
  fun %>% check_arg('formula') %>% check_equal()
  fun %>% check_arg('data') %>% check_equal()
}, {
  ex() %>% override_solution('lm(Boston$medv ~ Boston$lstat)') %>% check_function('lm') %>% check_arg('formula') %>% check_equal()
})

test_or({
    test_function("summary", args="object", index=2)
}, {
    ex() %>% override_solution("summary(lm(Boston$medv ~ Boston$lstat))") %>% check_function('summary') %>% check_arg('object') %>% check_equal()
})
```


--- type:NormalExercise lang:r xp:50 skills:1 key:35167f113b
## Multiple Regression: Boston Housing Data II

Now, let us expand the idea from the previous exercise by adding additional regressors to the model and estimate it again.

*** =instructions

- Regress the median housing value in a destrict, `medv`, on the average age of the buildings, `age`, the per capita crime rate, `crim`, the percentage of individuals with low socioeconomic status, `lstat`, and a constant. 
- Inspect the model summary
- The simple regression model's R^2 is stored in `R2_res`. Save the multiple regression model's $R^2$ to `R2_unres` and check whether the extended model yields a higher $R^2$. Hint: Use operators `<` or `>` for comparison.

*** =hint
You only need basic functions here: `library()`, `data()`, `lm()` and `summary`.
Use the help function to see how to specify additional regressors in the `formula` argument of `lm()`.

*** =sample_code
```{r}
# Conduct the regression


# Inspect the model summary


# Compare the determination coefficients

```

***=pre_exercise_code
```{r}
library(MASS)
R2_res <- summary(lm(medv ~ lstat, data = Boston))$r.squared
```

*** =solution
```{r}
# Conduct the regression
mod <- lm(medv ~ lstat + age + crim, data = Boston)

# Inspect the model summary
summary(mod)

# Save the R^2 to R_unres. Compare the determination coefficients
R2_unres <- summary(mod)$r.squared
R2_unres > R2_res

```


*** =sct
```{r}

test_or({
  ex() %>% check_function('lm') %>% check_result()
}, {
  ex() %>% override_solution('lm(Boston$medv ~ Boston$lstat + Boston$crim + Boston$age)') %>% check_function('lm') %>% check_result()
})

test_or({
    test_function_result("summary")
}, {
    ex() %>% override_solution("summary(lm(Boston$medv ~ Boston$lstat + Boston$crim + Boston$age))") %>% check_function('summary') %>% check_arg('object') %>% check_equal()
})

test_object("R2_unres")
test_predefined_objects("R2_res")

test_or(
    test_student_typed("R2_unres > R2_res"),
    test_student_typed("R2_unres < R2_res")
)

success_msg("Great! We see that the extended Model's R^2 is bigger than for the simple model. The extended model adapts better to the sample data.")
```



--- type:MultipleChoiceExercise lang:r xp: skills: key:e71f15da13
## Multiple Regression: Boston Housing Data III

Use the summary function again and have a look at the coefficient section of the output printed to the console.

Do the signs of the coefficient estimates correspond with your expectations? 

*The multiple regression model from the previous exercise is available in your environment (`mod`).* 


*** =instructions

- No, the sign of age is implausible since old houses are always decripit such that their value decreases with their age.
- No, the intercept is just around $32.82 what is far to low for new build houses in districts with zero crime rate and only high-income people.
- One would expect a nonnegative intercept ($house \, value \geq 0$). House values should be lower in districts with high crime rates and a high percentage of low income individuals.
- It can hardly be said if the signs of the estimated coefficients are right as coefficients could be biased.

*** =hint

Think about how housing value relates to the regressors used. Execute `?Boston` to check what exactly the variables measure.  

*** =pre_exercise_code
```{r}
library(MASS)
data("Boston")
mod <- lm(medv ~ lstat + age + crim, data = Boston)
```

*** =sct
```{r}
msg_bad1 <- "No, not necessairily. Think about old mansions."
msg_bad2 <- "The intercept might not be that realistic but it certainly is not $32.82. See `?Boston`."
msg_bad3 <- "Bias might be a problem. But You were asked if your expectations are met by the result."
msg_success <- "Right, that sounds plausible. Good job!"
test_mc(correct = 3, feedback_msgs = c(msg_bad1, msg_bad2, msg_success, msg_bad3))
```

--- type:MultipleChoiceExercise lang:r xp: skills: key:76501d8817
## Validity of Model Assumptions 

Think about $p$-values and parameter estimates. As in the simple regression model, You should only trust the results if the conditions for the regression do meet your assumptions reasonably well. Can You conclude that this holds using diagnostic plots?

*The multiple regression model from the previous exercise is available in your environment (`mod`).* 

*** =instructions

- Yes
- No

*** =hint

*** =pre_exercise_code
```{r}
library(MASS)
data("Boston")
mod <- lm(medv ~ lstat + age + crim, data = Boston)
```

*** =sct
```{r}
msg_success <- "Right. Diagnostic plots 1 and 3 indicate heteroskedasticity. You should be careful judging parameter significance here."
msg_bad <- "No, that is not right. Look again at the plots."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success))
```

--- type:NormalExercise lang:r xp: skills: key:2b32755b9d
## A Fully Fledged Model for Housing Values?

*The multiple regression model from the previous exercise, `mod`, is available in your environment.* 

*** =instructions

- Regress `medv` on all remaining variables that You find in the `Boston` data set. 
- Have a look at the model summary.
- What can you say about the regression's *adjusted* $R^2$? Does this model improve on the previous one? (*no code submission needed*)

*** =hint

- For brevity, you may use the regression formula `medv ~.`. This specifies a regression of `medv` on *all* remaining variables in the selected data set.
- Use `summary` on both models for comparison of adjusted R^2.

*** =pre_exercise_code
```{r}
library(MASS)
data("Boston")
mod <- lm(medv ~ lstat + age + crim, data = Boston)
```

*** =sample_code
```{r}
# Regress medv on all other variables provided with the Boston data set


# Inspect the model summary


```

*** =solution
```{r}
# Regress medv on all other variables provided with the Boston data set
mod <- lm(medv ~., data = Boston)

# Inspect the model summary
summary(mod)
```

*** =sct
```{r}
test_or({
  ex() %>% check_function('lm') %>% check_result()
}, {
  ex() %>% override_solution('lm(Boston$medv ~ Boston$lstat + Boston$crim + Boston$age + Boston$black + Boston$chas + Boston$dis + Boston$indus + Boston$nox + Boston$ptratio + Boston$rad + Boston$rm + Boston$tax + Boston$zn)') %>% check_function('lm') %>% check_result()
}, {
  ex() %>% override_solution('lm(medv ~ lstat + crim + age + black + chas + dis + indus + nox + ptratio + rad + rm + tax + zn, data = Boston)') %>% check_function('lm') %>% check_result()
})
test_function("summary")
success_msg("Okay. You can see that the full models $adj. R^2$ is about $0.73$ renders it better than the model `medv ~ lstat + age + crim` ($adj. R^2 = 0.55$)")
```

--- type:NormalExercise lang:r xp: skills: key:d9760cf640
## Model Selection: Adjusted R squared  

Maybe we can improve the model by dropping a variable? 

In this exercise, you will create several new models, each time dropping one of the explanatory variables used in the previous regression and control for the models $adj. R^2$

*The full regression model from the previous exercise, `mod`, is available in your environment.* 


*** =instructions

- Start by estimating a model, `mod_new` say, where you remove e.g. `lstat` from the list of explanatory variables.
- Next, access this model's $adj. R^2$ using the `summary()`. You can do this by `summary(mod_new)$adj.r.squared`
- Compare the model's $adj. R^2$ to the $adj. R^2$ of the full model ($0.7338$). 
- Repeat this for all explanatory variables used in the full regression model from the previous exercise
- Memorize the variable which removal leads to the highest improvement in $adj. R^2$ and the corresponding value. *You will need it in the next exercise!*

*** =hint

*** =pre_exercise_code
```{r}
library(MASS)
data("Boston")
mod <- lm(medv ~., data = Boston)
```

*** =sample_code
```{r}
# Estimate a model using all variables as regressors but crim (the first column) and extract adj. R^2
d <- Boston[,-1]
mod_new <- lm(medv ~., data=d)
summary(mod_new)$adj.r.squared

# Estimate a model using all variables as regressors but zn (the second column) and extract adj. R^2
d <- Boston[,-2]
mod_new <- lm(medv ~., data=d)
summary(mod_new)$adj.r.squared

# Repeat this for all remaining variables
# ...
# Select the variable which omission leads to the highest improvement in adj. R^2

# Hint: Do it with a loop! 

```

*** =solution
```{r}
# This solution is a bit technical but efficient

# Looped estimation of models
l <- list()
for (i in 1:13) {
  d <- Boston[,-i]
  l[[i]] <- summary(lm(medv ~., data=d))$adj.r.squared # save each adj. R^2 as a list entry in l
}
names(l) <- names(Boston[,1:13]) # assign variable names to the list entries

# select the variable which omission leads to the highest improvement in adj. R^2
which.max(l)
```


*** =sct
```{r}

```
