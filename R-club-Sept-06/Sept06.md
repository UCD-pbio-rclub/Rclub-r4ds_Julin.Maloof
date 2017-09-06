# Sept-06
Julin N Maloof  
9/5/2017  




## 21.2.1

### 1

_Write for loops to:_

_1. Compute the mean of every column in mtcars._


```r
result <- vector("double",ncol(mtcars))
names(result) <- colnames(mtcars)
for(i in seq_along(mtcars)) {
  result[i] <- mean(mtcars[[i]],na.rm=TRUE)
}
result
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

_2. Determine the type of each column in nycflights13::flights._


```r
flights <- nycflights13::flights
result <- vector("character",ncol(flights))
names(result) <- colnames(flights)
for(i in seq_along(flights)) {
  result[i] <- typeof(flights[[i]])
}
result
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

_3. Compute the number of unique values in each column of iris._


```r
result <- vector("integer",ncol(iris))
names(result) <- colnames(iris)
for(i in seq_along(iris)) {
  result[i] <- length(unique(iris[[i]]))
}
result
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

_4. Generate 10 random normals for each of  `μ=−10 ,  0 ,  10 , and  100.`_


```r
means <- c(-10,0,10,100)
result <- matrix(NA,ncol=length(means),nrow=10)
for(i in seq_along(means)) {
  result[,i] <- rnorm(10,mean=means[i])
}
result
```

```
##             [,1]       [,2]      [,3]      [,4]
##  [1,] -10.228587 -2.1061345  8.426778  98.18115
##  [2,] -12.329909  0.5159346  7.519531 100.67825
##  [3,] -12.050915 -0.4850558 10.013525 100.91852
##  [4,]  -9.205787 -0.8590332  7.653696 100.25102
##  [5,] -10.380779  0.2444017 10.359512  98.15901
##  [6,] -10.183432  0.1741995 11.658191 100.24772
##  [7,] -11.078382 -0.3588931 10.383875  99.19249
##  [8,] -10.570890  0.6642812  9.946180  98.90094
##  [9,] -10.519612 -0.3425584  8.569020 100.77356
## [10,] -10.012191  0.1424093 11.398326  98.29296
```

### 2
_Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors:_

```r
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}
out
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```


```r
out <- stringr::str_c(letters,collapse="")
out
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```


```r
x <- sample(100)
sd <- 0
for (i in seq_along(x)) {
  sd <- sd + (x[i] - mean(x)) ^ 2
}
sd <- sqrt(sd / (length(x) - 1))
sd
```

```
## [1] 29.01149
```


```r
sd <- sd(x)
sd
```

```
## [1] 29.01149
```


```r
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}
out
```

```
##   [1]  0.07533472  1.02971109  1.55125572  1.72719884  2.00960530
##   [6]  2.20052100  2.56391752  3.40980766  3.97227836  4.03814911
##  [11]  4.21343340  4.29149448  5.06393836  5.24117872  5.76844474
##  [16]  6.09123777  6.37400057  6.69713688  6.76326622  7.45869078
##  [21]  8.07817059  9.07041277  9.41771571  9.98503145 10.62056637
##  [26] 11.37497670 11.82648578 11.99866107 12.67061361 12.72811164
##  [31] 13.24248698 13.42395141 14.02976946 14.67265305 15.06283062
##  [36] 15.37238770 15.64686543 15.68203711 16.44613294 16.55236535
##  [41] 17.32793195 18.10905895 19.02167324 19.41271437 20.13158924
##  [46] 21.12252760 21.93983631 22.40032146 22.62635577 22.63867951
##  [51] 23.54412988 24.46068880 25.19083510 25.65678826 25.85560429
##  [56] 25.90124723 25.99516207 26.27328907 26.89084296 26.95292837
##  [61] 27.39341443 28.10714600 28.92482685 29.71908877 30.39817579
##  [66] 31.25816091 31.43471778 32.14662366 32.70294679 33.31661530
##  [71] 34.18495807 34.48158957 35.24718803 35.65378010 36.02565890
##  [76] 36.16484129 36.37833418 36.75419378 37.23507868 37.43112879
##  [81] 37.69778154 38.56464836 38.97772820 39.27067996 39.51789729
##  [86] 39.60890182 39.89821234 40.71172819 41.31484425 42.12362078
##  [91] 43.07658736 43.83304300 44.05072298 44.18441698 44.98457342
##  [96] 45.23322959 45.44974959 46.17686166 46.44017469 46.93215559
```


```r
cumsum(x)
```

```
##   [1]  0.07533472  1.02971109  1.55125572  1.72719884  2.00960530
##   [6]  2.20052100  2.56391752  3.40980766  3.97227836  4.03814911
##  [11]  4.21343340  4.29149448  5.06393836  5.24117872  5.76844474
##  [16]  6.09123777  6.37400057  6.69713688  6.76326622  7.45869078
##  [21]  8.07817059  9.07041277  9.41771571  9.98503145 10.62056637
##  [26] 11.37497670 11.82648578 11.99866107 12.67061361 12.72811164
##  [31] 13.24248698 13.42395141 14.02976946 14.67265305 15.06283062
##  [36] 15.37238770 15.64686543 15.68203711 16.44613294 16.55236535
##  [41] 17.32793195 18.10905895 19.02167324 19.41271437 20.13158924
##  [46] 21.12252760 21.93983631 22.40032146 22.62635577 22.63867951
##  [51] 23.54412988 24.46068880 25.19083510 25.65678826 25.85560429
##  [56] 25.90124723 25.99516207 26.27328907 26.89084296 26.95292837
##  [61] 27.39341443 28.10714600 28.92482685 29.71908877 30.39817579
##  [66] 31.25816091 31.43471778 32.14662366 32.70294679 33.31661530
##  [71] 34.18495807 34.48158957 35.24718803 35.65378010 36.02565890
##  [76] 36.16484129 36.37833418 36.75419378 37.23507868 37.43112879
##  [81] 37.69778154 38.56464836 38.97772820 39.27067996 39.51789729
##  [86] 39.60890182 39.89821234 40.71172819 41.31484425 42.12362078
##  [91] 43.07658736 43.83304300 44.05072298 44.18441698 44.98457342
##  [96] 45.23322959 45.44974959 46.17686166 46.44017469 46.93215559
```

### 3
_Combine your function writing and for loop skills:_

### 4
_It’s common to see for loops that don’t preallocate the output and instead increase the length of a vector at each step:_


```r
x <- lapply(floor(runif(10000,min = 1,max = 1000)), rnorm) # a list of vectors of variable lengths

system.time({
  output <- vector("integer", 0)
  for (i in seq_along(x)) {
    output <- c(output, length(x[[i]])) #changed from lengths to length because that made more sense to me
  }
})
```

```
##    user  system elapsed 
##   0.115   0.065   0.181
```

```r
head(output)
```

```
## [1] 610 332 495 287 844 775
```


```r
system.time({
  output <- vector("integer", length(x))
  for (i in seq_along(x)) {
    output[i] <- length(x[[i]]) #changed from lengths to length because that made more sense to me
  }
})
```

```
##    user  system elapsed 
##   0.004   0.000   0.004
```

```r
head(output)
```

```
## [1] 610 332 495 287 844 775
```


## 21.3.5

### 1
_Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, files <- dir("data/", pattern = "\\.csv$", full.names = TRUE), and now want to read each one with read_csv(). Write the for loop that will load them into a single data frame._

Vague question, asssuming binding by rows...


```r
results <- vector("list",length=length(files))
for(i in seq_along(files)) {
  results[i] <- read_csv(file=files[[i]])
}
results <- bind_rows(results)
```

### 2
_What happens if you use for (nm in names(x)) and x has no names? What if only some of the elements are named? What if the names are not unique?_


```r
x <- 1:10

for(nm in names(x)) {
  print(nm)
  print(x[[nm]])
}
```
blank!


```r
names(x)[1:4] <- letters[1:4]
for(nm in names(x)) {
  print(nm)
  print(x[[nm]])
}
```

```
## [1] "a"
## [1] 1
## [1] "b"
## [1] 2
## [1] "c"
## [1] 3
## [1] "d"
## [1] 4
## [1] NA
```

```
## Error in x[[nm]]: subscript out of bounds
```


```r
names(x)[5:6] <- c("e","e")
x
```

```
##    a    b    c    d    e    e <NA> <NA> <NA> <NA> 
##    1    2    3    4    5    6    7    8    9   10
```

```r
for(nm in names(x)) {
  print(x[[nm]])
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
## [1] 5
```

```
## Error in x[[nm]]: subscript out of bounds
```

multiple names return first element with that name

### 3
_Write a function that prints the mean of each numeric column in a data frame, along with its name. _


```r
show_mean <- function(df) {
  for(i in seq_along(df)) {
    if(is.numeric(df[[i]])) {
      mean <- round(mean(df[[i]]),2)
      names(df)[[i]] %>% 
        str_c(":") %>% 
        str_pad(max(str_length(names(df))+2),"right") %>%
        str_c(mean) %>%
        print()
    }
  }
}

show_mean(iris)
```

```
## Error in str_c(., ":"): could not find function "str_c"
```

### 4
_What does this code do? How does it work?_


```r
trans <- list( 
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}

mtcars
```

```
##                      mpg cyl     disp  hp drat    wt  qsec vs     am gear
## Mazda RX4           21.0   6 2.621936 110 3.90 2.620 16.46  0 manual    4
## Mazda RX4 Wag       21.0   6 2.621936 110 3.90 2.875 17.02  0 manual    4
## Datsun 710          22.8   4 1.769807  93 3.85 2.320 18.61  1 manual    4
## Hornet 4 Drive      21.4   6 4.227872 110 3.08 3.215 19.44  1   auto    3
## Hornet Sportabout   18.7   8 5.899356 175 3.15 3.440 17.02  0   auto    3
## Valiant             18.1   6 3.687098 105 2.76 3.460 20.22  1   auto    3
## Duster 360          14.3   8 5.899356 245 3.21 3.570 15.84  0   auto    3
## Merc 240D           24.4   4 2.403988  62 3.69 3.190 20.00  1   auto    4
## Merc 230            22.8   4 2.307304  95 3.92 3.150 22.90  1   auto    4
## Merc 280            19.2   6 2.746478 123 3.92 3.440 18.30  1   auto    4
## Merc 280C           17.8   6 2.746478 123 3.92 3.440 18.90  1   auto    4
## Merc 450SE          16.4   8 4.519562 180 3.07 4.070 17.40  0   auto    3
## Merc 450SL          17.3   8 4.519562 180 3.07 3.730 17.60  0   auto    3
## Merc 450SLC         15.2   8 4.519562 180 3.07 3.780 18.00  0   auto    3
## Cadillac Fleetwood  10.4   8 7.734711 205 2.93 5.250 17.98  0   auto    3
## Lincoln Continental 10.4   8 7.538066 215 3.00 5.424 17.82  0   auto    3
## Chrysler Imperial   14.7   8 7.210324 230 3.23 5.345 17.42  0   auto    3
## Fiat 128            32.4   4 1.289665  66 4.08 2.200 19.47  1 manual    4
## Honda Civic         30.4   4 1.240503  52 4.93 1.615 18.52  1 manual    4
## Toyota Corolla      33.9   4 1.165123  65 4.22 1.835 19.90  1 manual    4
## Toyota Corona       21.5   4 1.968091  97 3.70 2.465 20.01  1   auto    3
## Dodge Challenger    15.5   8 5.211098 150 2.76 3.520 16.87  0   auto    3
## AMC Javelin         15.2   8 4.981678 150 3.15 3.435 17.30  0   auto    3
## Camaro Z28          13.3   8 5.735485 245 3.73 3.840 15.41  0   auto    3
## Pontiac Firebird    19.2   8 6.554840 175 3.08 3.845 17.05  0   auto    3
## Fiat X1-9           27.3   4 1.294581  66 4.08 1.935 18.90  1 manual    4
## Porsche 914-2       26.0   4 1.971368  91 4.43 2.140 16.70  0 manual    5
## Lotus Europa        30.4   4 1.558413 113 3.77 1.513 16.90  1 manual    5
## Ford Pantera L      15.8   8 5.751872 264 4.22 3.170 14.50  0 manual    5
## Ferrari Dino        19.7   6 2.376130 175 3.62 2.770 15.50  0 manual    5
## Maserati Bora       15.0   8 4.932517 335 3.54 3.570 14.60  0 manual    5
## Volvo 142E          21.4   4 1.982839 109 4.11 2.780 18.60  1 manual    4
##                     carb
## Mazda RX4              4
## Mazda RX4 Wag          4
## Datsun 710             1
## Hornet 4 Drive         1
## Hornet Sportabout      2
## Valiant                1
## Duster 360             4
## Merc 240D              2
## Merc 230               2
## Merc 280               4
## Merc 280C              4
## Merc 450SE             3
## Merc 450SL             3
## Merc 450SLC            3
## Cadillac Fleetwood     4
## Lincoln Continental    4
## Chrysler Imperial      4
## Fiat 128               1
## Honda Civic            2
## Toyota Corolla         1
## Toyota Corona          1
## Dodge Challenger       2
## AMC Javelin            2
## Camaro Z28             4
## Pontiac Firebird       2
## Fiat X1-9              1
## Porsche 914-2          2
## Lotus Europa           2
## Ford Pantera L         4
## Ferrari Dino           6
## Maserati Bora          8
## Volvo 142E             2
```

we have a list of functions.  The for loop goes through that list.  Because the function names matches the colnames of mtcars we can apply it specifically to those columns to transform them.

## 21.4.1

### 1.
_Read the documentation for apply(). In the 2d case, what two for loops does it generalise?_

doing something for every row or for every column of a matrix

### 2.

```r
col_summary <- function(df, fun) {
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[i] <- fun(df[[i]])
  }
  out
}

col_summary2 <- function(df, fun) {
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    if(is.numeric(df[[i]])) {
      out[i] <- fun(df[[i]])
    } else {
      out[i] <- NA
    }
  }
  out
}

col_summary2(iris,mean)
```

```
## [1] 5.843333 3.057333 3.758000 1.199333       NA
```

```r
col_summary(iris,mean)
```

```
## Warning in mean.default(df[[i]]): argument is not numeric or logical:
## returning NA
```

```
## [1] 5.843333 3.057333 3.758000 1.199333       NA
```



