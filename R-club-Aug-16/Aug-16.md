# Aug 16th
Julin N Maloof  
8/12/2017  

## 19.2.1 Practice

### 1. 
_Why is TRUE not a parameter to rescale01()? What would happen if x contained a single missing value, and na.rm was FALSE?_

if na.rm=FALSE, then the return would be a single NA

### 2. 
_In the second variant of rescale01(), infinite values are left unchanged. Rewrite rescale01() so that -Inf is mapped to 0, and Inf is mapped to 1._


```r
x <- c(-Inf,1:10, Inf)

rescale01 <- function(x) {
  rng <- range(x, na.rm = TRUE, finite = TRUE)
  x <- (x - rng[1]) / (rng[2] - rng[1])
  x[x==Inf] <- 1
  x[x==-Inf] <- 0
  x
}


rescale01(x)
```

```
##  [1] 0.0000000 0.0000000 0.1111111 0.2222222 0.3333333 0.4444444 0.5555556
##  [8] 0.6666667 0.7777778 0.8888889 1.0000000 1.0000000
```


### 3. 
_Practice turning the following code snippets into functions. Think about what each function does. What would you call it? How many arguments does it need? Can you rewrite it to be more expressive or less duplicative?_

```
mean(is.na(x))

x / sum(x, na.rm = TRUE)

sd(x, na.rm = TRUE) / mean(x, na.rm = TRUE)
```


```r
x <- c(1:5,NA,NA)

meanNAs <- function(x) mean(is.na(x))

meanNAs(x)
```

```
## [1] 0.2857143
```


```r
fraction_of_total <- function(x) x / sum(x, na.rm = TRUE)

fraction_of_total(x)
```

```
## [1] 0.06666667 0.13333333 0.20000000 0.26666667 0.33333333         NA
## [7]         NA
```


```r
cv <- function(x) sd(x, na.rm = TRUE) / mean(x, na.rm = TRUE)
cv(x)
```

```
## [1] 0.5270463
```


### 4. 
_Follow http://nicercode.github.io/intro/writing-functions.html to write your own functions to compute the variance and skew of a numeric vector._



### 5. 
_Write both_na(), a function that takes two vectors of the same length and returns the number of positions that have an NA in both vectors._


```r
x <- c(1,2,3,NA,5,NA)
y <- c(NA,2,3,NA,5,6)
z <- c(NA,NA,NA,NA,NA,NA)
both_na <- function(x,y) {
  if(length(x) != length(y)) stop("x and y must be of equal length")
  sum(is.na(x) & is.na(y))
}

both_na(x,y)
```

```
## [1] 1
```

```r
both_na(x,z)
```

```
## [1] 2
```


### 6. 
_What do the following functions do? Why are they useful even though they are so short?_

```
is_directory <- function(x) file.info(x)$isdir
is_readable <- function(x) file.access(x, 4) == 0
```

They are useful because they are expressive about that they do and save typing.

### 7. 
_Read the complete lyrics to “Little Bunny Foo Foo”. There’s a lot of duplication in this song. Extend the initial piping example to recreate the complete song, and use functions to reduce the duplication._

## 19.4.4 Exercises

### 1. 
_What’s the difference between if and ifelse()? Carefully read the help and construct three examples that illustrate the key differences._

`if` works on a single value and takes a single action whereas `ifelse` is vectorized.  `ifelse` even retains dimensions...


```r
x <- 1:10

if (x < 5) "small" else "big"
```

```
## Warning in if (x < 5) "small" else "big": the condition has length > 1 and
## only the first element will be used
```

```
## [1] "small"
```

```r
ifelse(x < 5,"small","big")
```

```
##  [1] "small" "small" "small" "small" "big"   "big"   "big"   "big"  
##  [9] "big"   "big"
```

```r
y <- matrix(1:9,ncol=3)

ifelse(y < 5,"small","big")
```

```
##      [,1]    [,2]    [,3] 
## [1,] "small" "small" "big"
## [2,] "small" "big"   "big"
## [3,] "small" "big"   "big"
```




### 2. 
_Write a greeting function that says “good morning”, “good afternoon”, or “good evening”, depending on the time of day. (Hint: use a time argument that defaults to lubridate::now(). That will make it easier to test your function.)_


```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
library(stringr)
greeting <-function(time=lubridate::now()) {
  if(class(time)=="character") {
    if (str_count(time,"[:punct:]")==1) time <- hm(time) else time <- hms(time)
  }
  hourofday <- hour(time)
  if(hourofday <0 || hourofday > 23) stop("could not correctly parse time")
  as.character(cut(hour(time), breaks=c(0,12,18,24), labels=c("good morning","good afternoon", "good evening")))
}

greeting()
```

```
## Warning in if (class(time) == "character") {: the condition has length > 1
## and only the first element will be used
```

```
## [1] "good morning"
```

```r
greeting("09:00")
```

```
## [1] "good morning"
```

```r
greeting("09:00:10")
```

```
## [1] "good morning"
```

```r
greeting("13:00")
```

```
## [1] "good afternoon"
```

```r
greeting("19:00")
```

```
## [1] "good evening"
```

```r
greeting("25:00")
```

```
## Error in greeting("25:00"): could not correctly parse time
```

well that is a reasonable start. 

### 3. 
_Implement a fizzbuzz function. It takes a single number as input. If the number is divisible by three, it returns “fizz”. If it’s divisible by five it returns “buzz”. If it’s divisible by three and five, it returns “fizzbuzz”. Otherwise, it returns the number. Make sure you first write working code before you create the function._


```r
fizzbuzz <- function(x) {
  div3 <- x %% 3 == 0
  div5 <- x %% 5 == 0
  if(div3 && div5) return("fizzbuzz")
  if(div3) return("fizz")
  if(div5) return("buzz")
  return(x)
}
 fizzbuzz(12)
```

```
## [1] "fizz"
```

```r
 fizzbuzz(10)
```

```
## [1] "buzz"
```

```r
 fizzbuzz(15)
```

```
## [1] "fizzbuzz"
```

```r
 fizzbuzz(8)
```

```
## [1] 8
```


### 4. 
_How could you use cut() to simplify this set of nested if-else statements?_


```r
if (temp <= 0) {
  "freezing"
} else if (temp <= 10) {
  "cold"
} else if (temp <= 20) {
  "cool"
} else if (temp <= 30) {
  "warm"
} else {
  "hot"
}
```


```r
temp_rating <- function(temp) {
  cut(temp,c(-Inf,0,10,20,30,Inf),labels = c("freezing","cold","cool","warm","hot")) %>%
    as.character()
}
temp_rating(-5)
```

```
## [1] "freezing"
```

```r
temp_rating(0)
```

```
## [1] "freezing"
```

```r
temp_rating(5)
```

```
## [1] "cold"
```

```r
temp_rating(15)
```

```
## [1] "cool"
```

```r
temp_rating(30)
```

```
## [1] "warm"
```

```r
temp_rating(35)
```

```
## [1] "hot"
```

_How would you change the call to cut() if I’d used < instead of <=? What is the other chief advantage of cut() for this problem? (Hint: what happens if you have many values in temp?)_

Use right=FALSE for the equivalent of `<`

cut is vectorized


```r
temp_rating <- function(temp) {
  cut(temp,c(-Inf,0,10,20,30,Inf),right=FALSE,labels = c("freezing","cold","cool","warm","hot")) %>%
    as.character()
}
temp_rating(-5)
```

```
## [1] "freezing"
```

```r
temp_rating(0)
```

```
## [1] "cold"
```

```r
temp_rating(5)
```

```
## [1] "cold"
```

```r
temp_rating(15)
```

```
## [1] "cool"
```

```r
temp_rating(20)
```

```
## [1] "warm"
```

```r
temp_rating(30)
```

```
## [1] "hot"
```

```r
temp_rating(35)
```

```
## [1] "hot"
```

```r
temp_rating(c(5,10,15,20,40))
```

```
## [1] "cold" "cool" "cool" "warm" "hot"
```


### 5. 
_What happens if you use switch() with numeric values?_


```r
numbertotext <- function(x) {
  switch(x,
         1 = "one",
         2 = "two",
         3 = "three")
}
numbertotext(2)
numbertotext("2")
```

```
## Error: <text>:3:12: unexpected '='
## 2:   switch(x,
## 3:          1 =
##               ^
```


```r
numbertotext <- function(x) {
  switch(x,
         "0" = "zero",
         "1" = "one",
         "2" = "two",
         "3" = "three")
}
numbertotext(2) #CAREFUL!!
```

```
## [1] "one"
```

```r
numbertotext("3")
```

```
## [1] "three"
```


### 6. 
_What does this switch() call do? What happens if x is “e”?_


```r
switch(x, 
  a = ,
  b = "ab",
  c = ,
  d = "cd"
)
```
_Experiment, then carefully read the documentation._


```r
testswitch <- function(x) {
  switch(x, 
  a = ,
  b = "ab",
  c = ,
  d = "cd"
)
}

testswitch("a")
```

```
## [1] "ab"
```

```r
testswitch("b")
```

```
## [1] "ab"
```

```r
testswitch("e")
```

returns NULL (but if we had an unmamed element that would be returned.  also note that for named but undefined elements the next element is used (hence "a" goes to "ab")


```r
testswitch <- function(x) {
  switch(x, 
  a = ,
  b = "ab",
  c = ,
  d = "cd",
  "no match"
)
}

testswitch("a")
```

```
## [1] "ab"
```

```r
testswitch("b")
```

```
## [1] "ab"
```

```r
testswitch("e")
```

```
## [1] "no match"
```


