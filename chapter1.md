---
title       : Basics in R
description : This section teaches you basic commands in R.

--- type:NormalExercise lang:r xp:100 skills:1 key:3db79c581d
## Data Handling I

In this exercise, you will learn how to use data that comes with R packages. We use the `CPSSWEducation` data set contained in the AER package.

*** =instructions
- Load the AER package using the `library()` function by executing `library(AER)`. Add the `CPSSWEducation` data set to the workspace with `data("CPSSWEducation")` 
- Get an overview over the data stored in `CPSSWEducation` with help of the `summary()` function. Type and execute `summary(CPSSWEducation)`. Notice that `summary()` is called on an object. In R, an object's name has to be given without quotation marks.
- `CPSSWEducation` is a `data.frame` object. For now, you can think of it as a matrix where variables are stored in named columns. You can select a specific variable using the `$` operator. Print observations for `education` using the command `CPSSWEducation$education` 
- Use the `attach(CPSSWEducation)` command to attach the data set to R's search path. You are now able to access variables stored in the dataset by simply giving their names. Have a try: Execute `education`!
- Now suppose you are interested in the relation between earnings and education. Use `plot(education, earnings)` to create a scatter plot of observations on these variables

*** =sample_code
```{r}
# Load the AER package and add data to workspace


# Use the summary() function on the CPSSWEducation data set 


# Print observations of `education`.


# Attach the data set to R's search path


# Plot observations on education and earnings


```

*** =solution
```{r}
# Load the AER package and add data to workspace
library(AER)
data("CPSSWEducation")

# Use the summary() function on the CPSSWEducation data set 
summary(object=CPSSWEducation)

# Print observations of `education`
CPSSWEducation$education

# Attach the data set to R's search path
attach(CPSSWEducation)

# Plot observations on education and earnings
plot(education, earnings)

```

*** =sct
```{r}

test_function("library")

test_function("data",
              not_called_msg = "You didn't call `data()`!",
              incorrect_msg = "You did call `data()` with the wrong argument")
              
test_function("summary", args = "object",
              not_called_msg = "You didn't call `summary()`!",
              incorrect_msg = "You did call `summary()` with the wrong argument, `object`!")

test_student_typed("CPSSWEducation$education", 
                    not_typed_msg = "You failed printing observations of `education`. There must be a typo! Look again at your Code!")

test_function("attach", args = "what",
              not_called_msg = "You didn't call `attach()`!",
              incorrect_msg = "You did call `attach()` with the wrong argument, `what`!")

test_function("plot", args = c("x","y"),
              not_called_msg = "You didn't call `plot()`!",
              incorrect_msg = "You did call `attach()` with the arguments, `x` and `y`!")

test_error()
success_msg("Great! In the next exercise, You will learn some basic data wrangling techniques.")
```

--- type:NormalExercise lang:r xp:100 skills:1  key:89349eccc2
## Data Handling II

In this exercise, you will learn some more tricks in data wrangling. We already loaded the `cps1985` dataset for you, it is now available in the environment of you R session.
`cps1985` contains a subset of observations from the <a href="http://www.census.gov/programs-surveys/cps.html">Current Population Survey</a>.   

*** =pre_exercise_code
```{r}
cps1985 <- read.table("http://s3.amazonaws.com/assets.datacamp.com/production/course_1276/datasets/CPS1985.txt", sep=",", header=TRUE)
```

*** =instructions
- Get an overview over the data set. Check its dimensions using `dim()` and compute some descriptive statistics with `summary()`
- Print the first 100 observations of `wage` to the console using `[]`
- Store all variables contained in `cps1985` except for `union` in a new object named `cps1985new`

*** =sample_code
```{r}
# Check dimensions and compute descriptive statistics


# Use the [] operators to print the first 100 observations of `wage` to the console


# Create a new object `cps1985new` containing all variables from `cps1985` except for `union`


```

*** =solution
```{r}
# Check dimensions and compute descriptive statistics
dim(cps1985)
summary(cps1985)

# Use the [] operators to print the first 100 observations of `wage` to the console
cps1985[100,1]

# Create a new object `cps1985new` containing all variables from `cps1985` except for `union`
cps1985new <- cps1985[,-1]

```

*** =sct
```{r}
test_function("dim", args="x",
              not_called_msg = "You didn't call `dim()`!",
              incorrect_msg = "You did call `dim()` with the wrong argument")

test_function("summary", args = "object",
              not_called_msg = "You didn't call `summary()`!",
              incorrect_msg = "You did call `summary()` with the wrong argument, `object`!")

test_output_contains("cps1985[100,1]",
                         incorrect_msg = "Have you used `[100, 1]` to print the first 100 obs. from `wage` in `cps1985`?")
                         
test_object("cps1985new",
            undefined_msg = "You did not define an object named `cps1985new`!",
            incorrect_msg = "The data set does not look the way it is supposed to be... Maybe you did somethind wrong with the indexing?")

test_error()
success_msg("Good work!")
```

--- type:NormalExercise lang:r xp:100 skills:1  key:e0d56bf08c
## Data Handling III

This exercise teaches You some tricks to generate data yourself. 

*** =instructions

- Construct a 3x3 matrix `X` containing numbers from 1 to 9 rowwise using `matrix()`
- Create two arbitrary numeric column vectors of length three named `x` and `y` and join them columnwise using `cbind()`. Store the resulting 3x2 matrix in `Y`
- Compute the matrix product of `X` and `Y` using the `%*%` operator. Store the resulting 3x2 matrix in `A`
- Transpose `A` using `t()`

*** =sample_code
```{r}
# Construct a 3x3 matrix `X` containing numbers from 1 to 9 rowwise using `matrix()`


# Create two two vectors `x` = (1 2 3)'  and `y` = (4 5 6)' and join them using `cbind()`. Store the result in `Y`


# Determine the matrix product of `X` and `Y`. Store the result in `A`


# Transpose `A`


```

*** =solution
```{r}
# Construct a 3x3 matrix `X` containing numbers from 1 to 9 rowwise using `matrix()`
X <- matrix(c(1,2,3,4,5,6,7,8,9), nrow=3, byrow=T)

# Create two vectors `x` = (1 2 3)'  and `y` = (4 5 6)' and join them using `cbind()`. Store the result in `Y`
x <- c(1,2,3)
y <- c(4,5,6)
Y <- cbind(x,y)

# Determine the matrix product of `X` and `Y`. Store the result in `A`
A <- X %*% Y

# Transpose `A`
t(A)


```

*** =sct
```{r}
test_object("X",
            undefined_msg = "You did not define an object named `X`!",
            incorrect_msg = "The matrix does not look the way it is supposed to be... Maybe you confused rows and columns?")

test_object("x",
            undefined_msg = "You did not define an object named `x`!",
            incorrect_msg = "The matrix does not look the way it is supposed to be... Maybe you confused rows and columns?")


test_object("y",
            undefined_msg = "You did not define an object named `y`!",
            incorrect_msg = "The matrix does not look the way it is supposed to be... Maybe you confused rows and columns?")

test_function("cbind")

test_object("Y",
            undefined_msg = "You did not define an object named `y`!",
            incorrect_msg = "The matrix does not look the way it is supposed to be... Maybe you confused rows and columns?")

test_object("A")

test_output_contains("t(A)",
                         incorrect_msg = "Have you used `t()` to transpose A?")

```
