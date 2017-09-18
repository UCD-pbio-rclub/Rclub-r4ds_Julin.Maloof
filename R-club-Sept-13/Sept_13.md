# Sept 13
Julin N Maloof  
9/13/2017  





```r
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

## 21.5.3 Exercises

### 1. _Write code that uses one of the map functions to:_

_Compute the mean of every column in mtcars._


```r
mtcars %>% map_dbl(mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

_Determine the type of each column in nycflights13::flights._


```r
nycflights13::flights %>%
  map_chr(typeof)
```

```
##           year          month            day       dep_time sched_dep_time 
##      "integer"      "integer"      "integer"      "integer"      "integer" 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##       "double"      "integer"      "integer"       "double"    "character" 
##         flight        tailnum         origin           dest       air_time 
##      "integer"    "character"    "character"    "character"       "double" 
##       distance           hour         minute      time_hour 
##       "double"       "double"       "double"       "double"
```

_Compute the number of unique values in each column of iris._


```r
iris %>% map_int(~length(unique(.)))
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

_Generate 10 random normals for each of $mu$ = -10, 0, 10, and 100._


```r
mus <- c(-10,0,10,100)
mus %>% map(~rnorm(10,.)) 
```

```
## [[1]]
##  [1]  -9.762130 -10.609974  -8.374387  -9.261516 -11.422909 -10.417038
##  [7] -10.964986 -10.026771  -9.045327  -8.201929
## 
## [[2]]
##  [1]  0.40991758 -0.92162473  0.98229693  1.20248031 -0.05912062
##  [6] -1.05081554 -0.13874260  1.18146638 -0.73942014  0.31005452
## 
## [[3]]
##  [1] 11.809047  8.225649 10.162242  8.891787 10.262150 10.110826  9.327116
##  [8] 10.500561  9.603923 10.764356
## 
## [[4]]
##  [1] 102.28229  99.85318  99.79053 101.21441  98.09698 101.22303 100.77513
##  [8] 100.14549 100.14130 100.39677
```

```r
#or
mus %>% map(rnorm,n=10)
```

```
## [[1]]
##  [1]  -9.238204  -9.269134  -8.774567  -9.451472 -10.250123  -9.364103
##  [7] -10.643588  -9.960234  -9.479814  -7.784806
## 
## [[2]]
##  [1] -0.1688765  0.1069220  0.8391670  0.7058535 -0.9938654 -0.2456652
##  [7] -0.2547297  0.0673699 -1.5237483  0.4861129
## 
## [[3]]
##  [1]  9.205129  9.078512 12.085631 11.420180 10.259737 10.632469 10.213488
##  [8]  9.331497 10.647662 10.757036
## 
## [[4]]
##  [1] 101.70615  99.51234 101.33764 100.92347  99.80313  98.05243 100.55097
##  [8]  98.49647 100.59001  99.77147
```

### 2. _How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?_


```r
iris %>% map_lgl(is.factor)
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##        FALSE        FALSE        FALSE        FALSE         TRUE
```


### 3. _What happens when you use the map functions on vectors that aren’t lists? What does map(1:5, runif) do? Why?_

This works fine; map is applied to each element of an atomic list


```r
map(1:5,runif)
```

```
## [[1]]
## [1] 0.421939
## 
## [[2]]
## [1] 0.64927428 0.07564328
## 
## [[3]]
## [1] 0.9650866 0.7819228 0.9905562
## 
## [[4]]
## [1] 0.4910845 0.2373575 0.4008792 0.2108450
## 
## [[5]]
## [1] 0.5245517 0.4321717 0.3742254 0.4547538 0.3147184
```

### 4. _What does map(-2:2, rnorm, n = 5) do? Why? What does map_dbl(-2:2, rnorm, n = 5) do? Why?_


```r
map(-2:2, rnorm, n = 5) 
```

```
## [[1]]
## [1] -0.9190490 -2.3013103 -0.6300449 -1.5558707 -1.7973781
## 
## [[2]]
## [1]  0.117388 -2.427943 -1.729412 -0.497820  0.283978
## 
## [[3]]
## [1] -1.1090181  0.9392874 -1.4770305  0.1503266 -1.4196064
## 
## [[4]]
## [1] 0.9465018 1.6292491 1.4436960 1.9760713 2.0020657
## 
## [[5]]
## [1] 0.8718318 2.7288840 2.4059289 2.8575822 0.4340922
```

so map will fill in the chunked data into the first empty argument?  cool!



```r
map_dbl(-2:2, rnorm, n = 5)
```

```
## Error: Result 1 is not a length 1 atomic vector
```
error because output of each chunk is a vector of length 5

### 5 _Rewrite map(x, function(df) lm(mpg ~ wt, data = df)) to eliminate the anonymous function._


```r
mtcars %>%
  split(.$cyl) %>%
  map(~lm(mpg~wt, data = .))
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

## 21.9.3 Exercises

### 1.
_Implement your own version of every() using a for loop. Compare it with purrr::every(). What does purrr’s version do that your version doesn’t?_


```r
my_every <- function(x,test) {
  results <- vector("logical",length=length(x))
  for(i in seq_along(x))  {
    results[[i]] <- test(x[[i]])
  }
  if(sum(results) == length(x)) TRUE else FALSE
}

x <- 1:5

my_every(x, is.integer)
```

```
## [1] TRUE
```

```r
my_every(iris, is.factor)
```

```
## [1] FALSE
```

```r
every(x, "> 5")
```

```
## [1] TRUE
```

```r
my_every(x, "> 5")
```

```
## Error in test(x[[i]]): could not find function "test"
```

my_every can only take functions

But...


```r
my_every <- function(x,test) {
  results <- vector("logical",length=length(x))
  for(i in seq_along(x))  {
    if(is.function(test)) 
      results[[i]] <- test(x[[i]])
    else
      eval(parse(text=paste(x,test)))
  }
  if(sum(results) == length(x)) TRUE else FALSE
}

x <- 1:5

my_every(x, is.integer)
```

```
## [1] TRUE
```

```r
my_every(iris, is.factor)
```

```
## [1] FALSE
```

```r
every(x, "> 5")
```

```
## [1] TRUE
```

```r
my_every(x, "> 5")
```

```
## [1] FALSE
```

### 2. _Create an enhanced col_sum() that applies a summary function to every numeric column in a data frame._

Didn't we already do this?

### 3._A possible base R equivalent of col_sum() is:_



```r
col_sum3 <- function(df, f) {
  is_num <- sapply(df, is.numeric)
  df_num <- df[, is_num]

  sapply(df_num, f)
}
```


But it has a number of bugs as illustrated with the following inputs:


```r
df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)
# OK
col_sum3(df, mean)
```

```
## x y 
## 2 2
```

```r
# Has problems: don't always return numeric vector
col_sum3(df[1:2], mean)
```

```
## x y 
## 2 2
```

```r
col_sum3(df[1], mean)
```

```
## x 
## 2
```

```r
col_sum3(df[0], mean)
```

```
## Error: Unsupported index type: list
```

What causes the bugs?

This is because extracting from a data frame with [] changes types.
