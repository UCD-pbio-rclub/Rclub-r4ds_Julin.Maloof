# June 14 Tidy Data 12.1 - 12.4
Julin N Maloof  
6/13/2017  




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

## 12.2.1 Exercises

### 1.

### 2. 

## 12.3.1 Exercises

### 1 
_Why are gather() and spread() not perfectly symmetrical?
Carefully consider the following example:_


```r
stocks <- tibble(
  year   = c(2015, 2015, 2016, 2016),
  half  = c(   1,    2,     1,    2),
  return = c(1.88, 0.59, 0.92, 0.17)
)
stocks
```

```
## # A tibble: 4 × 3
##    year  half return
##   <dbl> <dbl>  <dbl>
## 1  2015     1   1.88
## 2  2015     2   0.59
## 3  2016     1   0.92
## 4  2016     2   0.17
```

```r
stocks %>% 
  spread(year, return)
```

```
## # A tibble: 2 × 3
##    half `2015` `2016`
## * <dbl>  <dbl>  <dbl>
## 1     1   1.88   0.92
## 2     2   0.59   0.17
```

```r
stocks %>% 
  spread(year, return) %>% 
  gather("year", "return", `2015`:`2016`)
```

```
## # A tibble: 4 × 3
##    half  year return
##   <dbl> <chr>  <dbl>
## 1     1  2015   1.88
## 2     2  2015   0.59
## 3     1  2016   0.92
## 4     2  2016   0.17
```
`year` gets converted to a character (since it was cycled through a column name)

the order changes

the `convert` argument converts data types, so:


```r
stocks %>% 
  spread(year, return) %>% 
  gather("year", "return", `2015`:`2016`, convert=TRUE)
```

```
## # A tibble: 4 × 3
##    half  year return
##   <dbl> <int>  <dbl>
## 1     1  2015   1.88
## 2     2  2015   0.59
## 3     1  2016   0.92
## 4     2  2016   0.17
```


### 2 


```r
table4a
table4a %>% 
  gather(1999, 2000, key = "year", value = "cases")
#> Error in combine_vars(vars, ind_list): Position must be between 0 and n
```
This fails because the column names `1999` and `2000` need to be backticked.  R is interpreting them as numeric.


```r
table4a %>% 
  gather(`1999`, `2000`, key = "year", value = "cases")
```

```
## # A tibble: 6 × 3
##       country  year  cases
##         <chr> <chr>  <int>
## 1 Afghanistan  1999    745
## 2      Brazil  1999  37737
## 3       China  1999 212258
## 4 Afghanistan  2000   2666
## 5      Brazil  2000  80488
## 6       China  2000 213766
```

```r
#> Error in combine_vars(vars, ind_list): Position must be between 0 and n
```

### 3
_Why does spreading this tibble fail? How could you add a new column to fix the problem?_


```r
people <- tribble(
  ~name,             ~key,    ~value,
  #-----------------|--------|------
  "Phillip Woods",   "age",       45,
  "Phillip Woods",   "height",   186,
  "Phillip Woods",   "age",       50,
  "Jessica Cordero", "age",       37,
  "Jessica Cordero", "height",   156
)
people
```

```
## # A tibble: 5 × 3
##              name    key value
##             <chr>  <chr> <dbl>
## 1   Phillip Woods    age    45
## 2   Phillip Woods height   186
## 3   Phillip Woods    age    50
## 4 Jessica Cordero    age    37
## 5 Jessica Cordero height   156
```


```r
people %>% spread(key = key,value = value)
```

Fails because "Phillip Woods" is represented twice


```r
people  %>% group_by(name,key) %>% 
  mutate(observation=row_number()) %>% 
  spread(key = key,value = value)
```

```
## Source: local data frame [3 x 4]
## Groups: name [2]
## 
##              name observation   age height
## *           <chr>       <int> <dbl>  <dbl>
## 1 Jessica Cordero           1    37    156
## 2   Phillip Woods           1    45    186
## 3   Phillip Woods           2    50     NA
```

Not perfect because we don't know at which age the height was measured...

### 4 

_Tidy the simple tibble below. Do you need to spread or gather it? What are the variables?_


```r
preg <- tribble(
  ~pregnant, ~male, ~female,
  "yes",     NA,    10,
  "no",      20,    12
)
preg
```

```
## # A tibble: 2 × 3
##   pregnant  male female
##      <chr> <dbl>  <dbl>
## 1      yes    NA     10
## 2       no    20     12
```

Male and female need to be gathered 


```r
preg %>% gather(male, female, key="gender", value=cases)
```

```
## # A tibble: 4 × 3
##   pregnant gender cases
##      <chr>  <chr> <dbl>
## 1      yes   male    NA
## 2       no   male    20
## 3      yes female    10
## 4       no female    12
```

Potentially gathered and spread..


```r
preg %>% gather(male, female, key="gender", value=cases) %>%
  spread(key = pregnant, value=cases) %>%
  rename(not_pregnant=no, pregnant=yes)
```

```
## # A tibble: 2 × 3
##   gender not_pregnant pregnant
## *  <chr>        <dbl>    <dbl>
## 1 female           12       10
## 2   male           20       NA
```
```

## 12.4.3 Exercises

### 1.


```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
separate(x, c("one", "two", "three")) # g gets lost
```

```
## Warning: Too many values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
separate(x, c("one", "two", "three"),extra="drop") # g gets lost, no warning
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
separate(x, c("one", "two", "three"), extra="merge") # f,g remain unseparated
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e   f,g
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
separate(x, c("one", "two", "three")) #NA for row 2, column 3 + warning
```

```
## Warning: Too few values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e  <NA>
## 3     f     g     i
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
separate(x, c("one", "two", "three"),fill="right") #NA for row 2, column 3 no warning 
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e  <NA>
## 3     f     g     i
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
separate(x, c("one", "two", "three"),fill="left") #NA for row 2, column 1 no warning 
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2  <NA>     d     e
## 3     f     g     i
```


### 2.

Keep the original data column intact

### 3

Separate separates by definine the separator, extract separates by regular rexpression groups.

Well because unite you always want to define the separator...

