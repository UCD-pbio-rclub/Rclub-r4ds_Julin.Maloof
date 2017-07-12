# July 12
Julin N Maloof  
7/11/2017  



## Exercises 14.2.5

### 1
_In code that doesn’t use stringr, you’ll often see paste() and paste0(). What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?_

`paste` has a default separator of " " whereas `paste0` has a default separator of "" (no separator).  

`paste0` equivalent to `str_c` and `paste` is equivalent to `str_c(...,sep =" ")`

test NA behavior

```r
test <- c("A",NA,"B")
paste("prefix",test)
```

```
## [1] "prefix A"  "prefix NA" "prefix B"
```

```r
paste0("prefix",test)
```

```
## [1] "prefixA"  "prefixNA" "prefixB"
```

```r
str_c("prefix",test)
```

```
## [1] "prefixA" NA        "prefixB"
```

`paste` and `paste0` convert NA to a character string whereas `str_c` does not.

### 2

_In your own words, describe the difference between the sep and collapse arguments to str_c()._

sep is the separator used to separate each different argument provided to `str_c`.  If any of those elements are non-atomic (i.e. have multiple elements themselves) then collapse will collapse them with its argument value as a separator


```r
str_c(LETTERS,"1")
```

```
##  [1] "A1" "B1" "C1" "D1" "E1" "F1" "G1" "H1" "I1" "J1" "K1" "L1" "M1" "N1"
## [15] "O1" "P1" "Q1" "R1" "S1" "T1" "U1" "V1" "W1" "X1" "Y1" "Z1"
```

```r
str_c(LETTERS,"1",sep="*")
```

```
##  [1] "A*1" "B*1" "C*1" "D*1" "E*1" "F*1" "G*1" "H*1" "I*1" "J*1" "K*1"
## [12] "L*1" "M*1" "N*1" "O*1" "P*1" "Q*1" "R*1" "S*1" "T*1" "U*1" "V*1"
## [23] "W*1" "X*1" "Y*1" "Z*1"
```

```r
str_c(LETTERS,"1",collapse="*")
```

```
## [1] "A1*B1*C1*D1*E1*F1*G1*H1*I1*J1*K1*L1*M1*N1*O1*P1*Q1*R1*S1*T1*U1*V1*W1*X1*Y1*Z1"
```

### 3

_Use str_length() and str_sub() to extract the middle character from a string. What will you do if the string has an even number of characters?_


```r
str_middle <- function(string) {
  middle <- str_length(string)/2
  start <- ceiling(middle)
  end <- ifelse(start==middle, # string is even 
                middle + 1,
                ceiling(middle))
  str_sub(string,start,end)
}
```


```r
str_middle("ABCDE")
```

```
## [1] "C"
```

```r
str_middle("ABCDEF")
```

```
## [1] "CD"
```

### 4

_What does str_wrap() do? When might you want to use it?_

formats character strings into paragraphs, adding a return every X charcters and also allowing indentation of the first and subsequent lines.


```r
Ishmael <- "Call me Ishmael. Some years ago - never mind how long precisely - having little or no money in my purse, and nothing particular to interest me on shore, I thought I would sail about a little and see the watery part of the world. It is a way I have of driving off the spleen and regulating the circulation. Whenever I find myself growing grim about the mouth; whenever it is a damp, drizzly November in my soul; whenever I find myself involuntarily pausing before coffin warehouses, and bringing up the rear of every funeral I meet; and especially whenever my hypos get such an upper hand of me, that it requires a strong moral principle to prevent me from deliberately stepping into the street, and methodically knocking people's hats off - then, I account it high time to get to sea as soon as I can. This is my substitute for pistol and ball. With a philosophical flourish Cato throws himself upon his sword; I quietly take to the ship. There is nothing surprising in this. If they but knew it, almost all men in their degree, some time or other, cherish very nearly the same feelings towards the ocean with me."
```



```r
Ishmael %>% writeLines()
```

```
## Call me Ishmael. Some years ago - never mind how long precisely - having little or no money in my purse, and nothing particular to interest me on shore, I thought I would sail about a little and see the watery part of the world. It is a way I have of driving off the spleen and regulating the circulation. Whenever I find myself growing grim about the mouth; whenever it is a damp, drizzly November in my soul; whenever I find myself involuntarily pausing before coffin warehouses, and bringing up the rear of every funeral I meet; and especially whenever my hypos get such an upper hand of me, that it requires a strong moral principle to prevent me from deliberately stepping into the street, and methodically knocking people's hats off - then, I account it high time to get to sea as soon as I can. This is my substitute for pistol and ball. With a philosophical flourish Cato throws himself upon his sword; I quietly take to the ship. There is nothing surprising in this. If they but knew it, almost all men in their degree, some time or other, cherish very nearly the same feelings towards the ocean with me.
```


```r
Ishmael %>% str_wrap(width=60,indent=5) %>% writeLines()
```

```
##      Call me Ishmael. Some years ago - never mind how long
## precisely - having little or no money in my purse, and
## nothing particular to interest me on shore, I thought I
## would sail about a little and see the watery part of the
## world. It is a way I have of driving off the spleen and
## regulating the circulation. Whenever I find myself growing
## grim about the mouth; whenever it is a damp, drizzly
## November in my soul; whenever I find myself involuntarily
## pausing before coffin warehouses, and bringing up the rear
## of every funeral I meet; and especially whenever my hypos
## get such an upper hand of me, that it requires a strong
## moral principle to prevent me from deliberately stepping
## into the street, and methodically knocking people's hats
## off - then, I account it high time to get to sea as soon
## as I can. This is my substitute for pistol and ball. With a
## philosophical flourish Cato throws himself upon his sword;
## I quietly take to the ship. There is nothing surprising in
## this. If they but knew it, almost all men in their degree,
## some time or other, cherish very nearly the same feelings
## towards the ocean with me.
```

### 5
_What does str_trim() do? What’s the opposite of str_trim()?_

str_trim() removes white space from beginning and/or end of strings.  The opposite is str_pad

### 6
_Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2._


```r
str_format <- function(vec) {
  if(length(vec) < 2) result <- vec
  
  if(length(vec) == 2 ) {
    result <- vec %>% 
      str_c(collapse = " and ")
  }
  
  if(length(vec) > 2) {
    end <- last(vec) #get the last element
    result <- vec %>% 
      setdiff(end) %>% #get all but the last element
      str_c(collapse=", ") %>% # collapse all but the last element with commas
      str_c(end, sep=", and ") # use ", and " to separate the last element from the rest
  }
  
  return(result)
}
```


```r
str_format(babynames::babynames$name[0])
```

```
## character(0)
```

```r
str_format(babynames::babynames$name[1])
```

```
## [1] "Mary"
```

```r
str_format(babynames::babynames$name[1:2])
```

```
## [1] "Mary and Anna"
```

```r
str_format(babynames::babynames$name[1:5])
```

```
## [1] "Mary, Anna, Emma, Elizabeth, and Minnie"
```

## Regexone Tutorial

### 1

`abc`

OR

`abc(de)?(fg)?`

### 1.5

`123`

OR

`\d\d\d`

OR 

`\d{3}`

### 2

`...\.`

### 3

`[cmf]an`

### 4

`[^b]og`

### 5

`[A-C][n-p][a-c]`

### 6

`waz{2,}up`

### 7

`a+b*c+`

### 8

`[0-9]+ files? found\?`

### 9

`[0-9]\.\s+abc`

### 10

`^Mission: successful$`

### 11

`^(file.*)\.pdf`

### 12

`(^[A-Z]+[a-z]{2} ([0-9]{4}))`

### 13

`(\d+)x(\d+)`

### 14

`I love (cats|dogs)`

### 15

`.*`

## Regexone problems

### 1

`^(-|\+)?(\d+(\.|,|e)?)+\d+$`

### 2

`^1? ?\(?(\d{3})\)?( |-)?\d{3}( |-)?\d{4}`

### 3

`^([a-z.]+)`

### 4

`^<([a-z]+)`

### 5

`^\.{0}(.*)\.(jpg|png|gif)$`

### 6

`^\s*(.*)\s*`

### 7

`at \w+\.+\w+\.+(\w+)\((.*):(\d+)\)`

### 8

couldn't get my solution to work, had to look it up

`(\w+)://([\w\-\.]+)(:(\d+))?`

## 14.3.5.1 exercises

### 1

_Describe, in words, what these expressions will match:_

`(.)\1\1`

A triple repeated character.  To obtain that regex we have to escape the \


```r
writeLines("(.)\\1\\1")
```

```
## (.)\1\1
```

```r
str_view("appple","(.)\\1\\1")
```

<!--html_preserve--><div id="htmlwidget-e1e91086a3214b3cc3d3" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-e1e91086a3214b3cc3d3">{"x":{"html":"<ul>\n  <li>a<span class='match'>ppp<\/span>le<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


`"(.)(.)\\2\\1"`

A two character palindrome, like "abba"


```r
str_view("abba is my favorite band", "(.)(.)\\2\\1")
```

<!--html_preserve--><div id="htmlwidget-4e91ac692c21f38d8c64" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-4e91ac692c21f38d8c64">{"x":{"html":"<ul>\n  <li><span class='match'>abba<\/span> is my favorite band<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



`(..)\1`

A two character string repeated , ie "abab"


`"(.).\\1.\\1"`

A character repeated twice, but with any other characters inbetween


```r
str_view("bananas are good", "(.).\\1.\\1")
```

<!--html_preserve--><div id="htmlwidget-cc817199e3622c5ea5c7" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-cc817199e3622c5ea5c7">{"x":{"html":"<ul>\n  <li>b<span class='match'>anana<\/span>s are good<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


`"(.)(.)(.).*\\3\\2\\1"`

A three character palindrome separated by any number of characters in the middle


```r
str_view("geese from canda see many sights","(.)(.)(.).*\\3\\2\\1")
```

<!--html_preserve--><div id="htmlwidget-5c0e4eb9cc3efc36ab9a" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5c0e4eb9cc3efc36ab9a">{"x":{"html":"<ul>\n  <li>g<span class='match'>eese from canda see<\/span> many sights<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### 2

_Construct regular expressions to match words that:_

_1. Start and end with the same character._


```r
pattern <- "\\W([A-Za-z])[a-z]*\\1\\W"
str_view_all("what silly sentence am I going to write for this exercise?",pattern)
```

<!--html_preserve--><div id="htmlwidget-0f19dcd16e02f0f1c874" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0f19dcd16e02f0f1c874">{"x":{"html":"<ul>\n  <li>what silly sentence am I<span class='match'> going <\/span>to write for this<span class='match'> exercise?<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# not perfect: single character words.  Also caps need to be dealt with.  And match includes spaces
```

Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)


```r
pattern <- "[A-Za-z]*([A-Za-z]{2})[a-z]*\\1[a-z]*"
str_view_all("what silly sentence am I going to write for this exercise?",pattern)
```

<!--html_preserve--><div id="htmlwidget-d271a9da9135ec13794b" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-d271a9da9135ec13794b">{"x":{"html":"<ul>\n  <li>what silly <span class='match'>sentence<\/span> am I going to write for this exercise?<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)


```r
pattern <- "[A-Za-z]*([A-Za-z])[a-z]*\\1[a-z]*\\1[a-z]*"
str_view_all("what silly sentence am I going to write for this exercise?  Aiyeee",pattern)
```

<!--html_preserve--><div id="htmlwidget-b16cb9026c48b920b5f5" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-b16cb9026c48b920b5f5">{"x":{"html":"<ul>\n  <li>what silly <span class='match'>sentence<\/span> am I going to write for this <span class='match'>exercise<\/span>?  <span class='match'>Aiyeee<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



