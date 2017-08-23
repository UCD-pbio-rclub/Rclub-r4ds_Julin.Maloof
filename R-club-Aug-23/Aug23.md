# August 23rd
Julin N Maloof  
8/22/2017  

## 19.5.5 exercises

### 1
_What does commas(letters, collapse = "-") do? Why?_

This will return `a-b-c-d-e` etc because we are over-riding the default

### 2
_It’d be nice if you could supply multiple characters to the pad argument, e.g. rule("Title", pad = "-+"). Why doesn’t this currently work? How could you fix it?_


```r
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}

rule("Title", pad = "-+")
```

```
## Title -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

Fails because the width of the pad is not being taken into account


```r
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- (getOption("width") - nchar(title) - 5)/nchar(pad)
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}

rule("Title", pad = "-+")
```

```
## Title -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### 3
_What does the trim argument to mean() do? When might you use it?_

```r
?mean
```

Trims the fraction of observations from each end of the input.  Would be good if input was multiple observations from a device where the beginning and end points were noisy.

### 4
_The default value for the method argument to cor() is c("pearson", "kendall", "spearman"). What does that mean? What value is used by default?_

This is providing the various options and the first one is used by default.

See 

```r
?match.arg
```
which cor() uses

## 20.3.5

### 1
_Describe the difference between is.finite(x) and !is.infinite(x)._

```r
x <- c(1,2,Inf,NA)
is.finite(x)
```

```
## [1]  TRUE  TRUE FALSE FALSE
```

```r
!is.infinite(x)
```

```
## [1]  TRUE  TRUE FALSE  TRUE
```

NA is not finite and also is not infinite


### 2
_Read the source code for dplyr::near() (Hint: to see the source code, drop the ()). How does it work?_


```r
dplyr::near
```

```
## function (x, y, tol = .Machine$double.eps^0.5) 
## {
##     abs(x - y) < tol
## }
## <environment: namespace:dplyr>
```

determines whether the difference between x and y is less than a predefined tolerance level

### 3
_A logical vector can take 3 possible values. How many possible values can an integer vector take? How many possible values can a double take? Use google to do some research._

Integer values:

```r
2* .Machine$integer.max + 1
```

```
## [1] 4294967295
```

```r
as.integer(2* .Machine$integer.max + 1)
```

```
## Warning: NAs introduced by coercion to integer range
```

```
## [1] NA
```

```r
as.integer(.Machine$integer.max)
```

```
## [1] 2147483647
```

Doubles are harder, essentially infinite number, but limited by precision.


```r
#range of positive values:
c(.Machine$double.xmin, .Machine$double.xmax)
```

```
## [1] 2.225074e-308 1.797693e+308
```

```r
#precision (but this is base 2...)
.Machine$double.digits
```

```
## [1] 53
```

### 4
_Brainstorm at least four functions that allow you to convert a double to an integer. How do they differ? Be precise._

Well we could

1. truncate it
2. round it
3. go to the next higher
4?

Is this what he is asking or is he asking for actual methods...or existing methods?


```r
x <- c(1,1.4,1.5,1.6,2)
as.integer(x)
```

```
## [1] 1 1 1 1 2
```

```r
as.integer(round(x,digits = 0))
```

```
## [1] 1 1 2 2 2
```

```r
floor(x)
```

```
## [1] 1 1 1 1 2
```

```r
ceiling(x)
```

```
## [1] 1 2 2 2 2
```

### 5
_What functions from the readr package allow you to turn a string into logical, integer, and double vector?_


```r
library(readr)
x <- c("TRUE","FALSE")
typeof(x)
```

```
## [1] "character"
```

```r
parse_logical(x)
```

```
## [1]  TRUE FALSE
```

```r
typeof(parse_logical(x))
```

```
## [1] "logical"
```

```r
x <- c("1","2")
parse_integer(x)
```

```
## [1] 1 2
```

```r
typeof(parse_integer(x))
```

```
## [1] "integer"
```

```r
x <- c("1.3","2.3")
parse_double(x)
```

```
## [1] 1.3 2.3
```

```r
typeof(parse_double(x))
```

```
## [1] "double"
```

