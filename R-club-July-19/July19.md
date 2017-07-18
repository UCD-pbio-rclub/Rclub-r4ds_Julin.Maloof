# July 19
Julin N Maloof  
7/17/2017  




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

```r
library(stringr)
```



## 14.4.2 Exercises

### 1
_Find all words that start or end with x_

```r
mywords <- c(words,"xylophone","xeriscape","xanax")
str_subset(mywords,"^x|x$")
```

```
## [1] "box"       "sex"       "six"       "tax"       "xylophone" "xeriscape"
## [7] "xanax"
```

```r
mywords[str_detect(mywords,"^x") | str_detect(mywords,"x$")]
```

```
## [1] "box"       "sex"       "six"       "tax"       "xylophone" "xeriscape"
## [7] "xanax"
```

_Find all words that start with a vowel and end with a consonant._


```r
str_subset(mywords,"^[aeiou].*[^aeiou]$")
```

```
##   [1] "about"       "accept"      "account"     "across"      "act"        
##   [6] "actual"      "add"         "address"     "admit"       "affect"     
##  [11] "afford"      "after"       "afternoon"   "again"       "against"    
##  [16] "agent"       "air"         "all"         "allow"       "almost"     
##  [21] "along"       "already"     "alright"     "although"    "always"     
##  [26] "amount"      "and"         "another"     "answer"      "any"        
##  [31] "apart"       "apparent"    "appear"      "apply"       "appoint"    
##  [36] "approach"    "arm"         "around"      "art"         "as"         
##  [41] "ask"         "at"          "attend"      "authority"   "away"       
##  [46] "awful"       "each"        "early"       "east"        "easy"       
##  [51] "eat"         "economy"     "effect"      "egg"         "eight"      
##  [56] "either"      "elect"       "electric"    "eleven"      "employ"     
##  [61] "end"         "english"     "enjoy"       "enough"      "enter"      
##  [66] "environment" "equal"       "especial"    "even"        "evening"    
##  [71] "ever"        "every"       "exact"       "except"      "exist"      
##  [76] "expect"      "explain"     "express"     "identify"    "if"         
##  [81] "important"   "in"          "indeed"      "individual"  "industry"   
##  [86] "inform"      "instead"     "interest"    "invest"      "it"         
##  [91] "item"        "obvious"     "occasion"    "odd"         "of"         
##  [96] "off"         "offer"       "often"       "okay"        "old"        
## [101] "on"          "only"        "open"        "opportunity" "or"         
## [106] "order"       "original"    "other"       "ought"       "out"        
## [111] "over"        "own"         "under"       "understand"  "union"      
## [116] "unit"        "university"  "unless"      "until"       "up"         
## [121] "upon"        "usual"
```

```r
mywords %>% .[str_detect(.,"^[aeiou]") & ! str_detect(.,"[aeiou]$")]
```

```
##   [1] "about"       "accept"      "account"     "across"      "act"        
##   [6] "actual"      "add"         "address"     "admit"       "affect"     
##  [11] "afford"      "after"       "afternoon"   "again"       "against"    
##  [16] "agent"       "air"         "all"         "allow"       "almost"     
##  [21] "along"       "already"     "alright"     "although"    "always"     
##  [26] "amount"      "and"         "another"     "answer"      "any"        
##  [31] "apart"       "apparent"    "appear"      "apply"       "appoint"    
##  [36] "approach"    "arm"         "around"      "art"         "as"         
##  [41] "ask"         "at"          "attend"      "authority"   "away"       
##  [46] "awful"       "each"        "early"       "east"        "easy"       
##  [51] "eat"         "economy"     "effect"      "egg"         "eight"      
##  [56] "either"      "elect"       "electric"    "eleven"      "employ"     
##  [61] "end"         "english"     "enjoy"       "enough"      "enter"      
##  [66] "environment" "equal"       "especial"    "even"        "evening"    
##  [71] "ever"        "every"       "exact"       "except"      "exist"      
##  [76] "expect"      "explain"     "express"     "identify"    "if"         
##  [81] "important"   "in"          "indeed"      "individual"  "industry"   
##  [86] "inform"      "instead"     "interest"    "invest"      "it"         
##  [91] "item"        "obvious"     "occasion"    "odd"         "of"         
##  [96] "off"         "offer"       "often"       "okay"        "old"        
## [101] "on"          "only"        "open"        "opportunity" "or"         
## [106] "order"       "original"    "other"       "ought"       "out"        
## [111] "over"        "own"         "under"       "understand"  "union"      
## [116] "unit"        "university"  "unless"      "until"       "up"         
## [121] "upon"        "usual"
```

_Are there any words that contain at least one of each different vowel?_

Not sure how to do this in single regex


```r
mywords %>% .[
  str_detect(.,"a") & 
    str_detect(.,"e") & 
    str_detect(.,"i") & 
    str_detect(.,"o") & 
    str_detect(.,"u") 
  ]
```

```
## character(0)
```


### 2

_What word has the highest number of vowels? What word has the highest proportion of vowels? (Hint: what is the denominator?)_


```r
words.df <- tibble(
  word=mywords)

#total vowels
words.df %>% mutate(vowels=str_count(word,"[aeiou]")) %>% arrange(desc(vowels))
```

```
## # A tibble: 983 × 2
##           word vowels
##          <chr>  <int>
## 1  appropriate      5
## 2    associate      5
## 3    available      5
## 4    colleague      5
## 5    encourage      5
## 6   experience      5
## 7   individual      5
## 8   television      5
## 9     absolute      4
## 10     achieve      4
## # ... with 973 more rows
```

```r
#average vowels
words.df %>% 
  mutate(vowels=str_count(word,"[aeiou]"),
         letters=str_length(word),
         proportion=vowels/letters) %>% 
  arrange(desc(proportion))
```

```
## # A tibble: 983 × 4
##      word vowels letters proportion
##     <chr>  <int>   <int>      <dbl>
## 1       a      1       1  1.0000000
## 2    area      3       4  0.7500000
## 3    idea      3       4  0.7500000
## 4     age      2       3  0.6666667
## 5     ago      2       3  0.6666667
## 6     air      2       3  0.6666667
## 7     die      2       3  0.6666667
## 8     due      2       3  0.6666667
## 9     eat      2       3  0.6666667
## 10 europe      4       6  0.6666667
## # ... with 973 more rows
```

## 14.4.3.1 Exercises

### 1 
_In the previous example, you might have noticed that the regular expression matched “flickered”, which is not a colour. Modify the regex to fix the problem._

```r
colors <- c("red", "orange", "yellow", "green", "blue", "purple")
color_match <- str_c("\\b",colors,"\\b", collapse = "|")
color_match
```

```
## [1] "\\bred\\b|\\borange\\b|\\byellow\\b|\\bgreen\\b|\\bblue\\b|\\bpurple\\b"
```


```r
more <- sentences[str_count(sentences, color_match) > 1]
str_view_all(more, color_match)
```

<!--html_preserve--><div id="htmlwidget-0347cb557efc1329d8e7" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0347cb557efc1329d8e7">{"x":{"html":"<ul>\n  <li>It is hard to erase <span class='match'>blue<\/span> or <span class='match'>red<\/span> ink.<\/li>\n  <li>The sky in the west is tinged with <span class='match'>orange<\/span> <span class='match'>red<\/span>.<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### 2
_From the Harvard sentences data, extract:_

_The first word from each sentence._
_All words ending in ing._
_All plurals._


```r
str_extract(sentences,"^\\w+\\b")
```

```
##   [1] "The"        "Glue"       "It"         "These"      "Rice"      
##   [6] "The"        "The"        "The"        "Four"       "Large"     
##  [11] "The"        "A"          "The"        "Kick"       "Help"      
##  [16] "A"          "Smoky"      "The"        "The"        "The"       
##  [21] "The"        "The"        "Press"      "The"        "The"       
##  [26] "Two"        "Her"        "The"        "It"         "Read"      
##  [31] "Hoist"      "Take"       "Note"       "Wipe"       "Mend"      
##  [36] "The"        "The"        "The"        "The"        "What"      
##  [41] "A"          "The"        "Sickness"   "The"        "The"       
##  [46] "Lift"       "The"        "Hop"        "The"        "Mesh"      
##  [51] "The"        "The"        "Adding"     "The"        "A"         
##  [56] "The"        "March"      "A"          "Place"      "Both"      
##  [61] "We"         "Use"        "He"         "The"        "A"         
##  [66] "Cars"       "The"        "This"       "The"        "Those"     
##  [71] "A"          "The"        "The"        "The"        "The"       
##  [76] "A"          "The"        "The"        "The"        "The"       
##  [81] "The"        "See"        "There"      "The"        "The"       
##  [86] "The"        "Cut"        "Men"        "Always"     "He"        
##  [91] "The"        "A"          "A"          "The"        "The"       
##  [96] "Bail"       "The"        "A"          "Ten"        "The"       
## [101] "Oak"        "Cats"       "The"        "Open"       "Add"       
## [106] "Thieves"    "The"        "Act"        "The"        "Move"      
## [111] "The"        "Leaves"     "The"        "Split"      "Burn"      
## [116] "He"         "Weave"      "Hemp"       "A"          "We"        
## [121] "Type"       "The"        "The"        "The"        "Paste"     
## [126] "The"        "It"         "The"        "Feel"       "The"       
## [131] "A"          "He"         "Pluck"      "Two"        "The"       
## [136] "Bring"      "Write"      "Clothes"    "We"         "Port"      
## [141] "The"        "Guess"      "A"          "The"        "These"     
## [146] "Pure"       "The"        "The"        "Mud"        "The"       
## [151] "The"        "A"          "He"         "The"        "The"       
## [156] "The"        "The"        "We"         "She"        "The"       
## [161] "The"        "At"         "Drop"       "A"          "An"        
## [166] "Wood"       "The"        "He"         "A"          "A"         
## [171] "Steam"      "The"        "There"      "The"        "Torn"      
## [176] "Sunday"     "The"        "The"        "They"       "Add"       
## [181] "Acid"       "Fairy"      "Eight"      "The"        "A"         
## [186] "Add"        "We"         "There"      "He"         "She"       
## [191] "The"        "Corn"       "Where"      "The"        "Sell"      
## [196] "The"        "The"        "Bring"      "They"       "Farmers"   
## [201] "The"        "The"        "Float"      "A"          "A"         
## [206] "The"        "After"      "The"        "He"         "Even"      
## [211] "The"        "The"        "The"        "Do"         "Lire"      
## [216] "The"        "It"         "Write"      "The"        "The"       
## [221] "A"          "Coax"       "Schools"    "The"        "They"      
## [226] "The"        "The"        "Jazz"       "Rake"       "Slash"     
## [231] "Try"        "They"       "He"         "They"       "The"       
## [236] "Whitings"   "Some"       "Jerk"       "A"          "Madam"     
## [241] "On"         "The"        "This"       "Add"        "The"       
## [246] "The"        "The"        "To"         "The"        "Jump"      
## [251] "Yell"       "They"       "Both"       "In"         "The"       
## [256] "The"        "Ducks"      "Fruit"      "These"      "Canned"    
## [261] "The"        "Carry"      "The"        "We"         "Gray"      
## [266] "The"        "High"       "Tea"        "A"          "A"         
## [271] "The"        "Find"       "Cut"        "The"        "Look"      
## [276] "The"        "Nine"       "The"        "The"        "Soak"      
## [281] "The"        "A"          "All"        "ii"         "To"        
## [286] "Shape"      "The"        "Hedge"      "Quench"     "Tight"     
## [291] "The"        "The"        "The"        "Watch"      "The"       
## [296] "The"        "Write"      "His"        "The"        "Tin"       
## [301] "Slide"      "The"        "The"        "Pink"       "She"       
## [306] "The"        "It"         "Let"        "The"        "The"       
## [311] "The"        "The"        "The"        "Paper"      "The"       
## [316] "The"        "Screw"      "Time"       "The"        "Men"       
## [321] "Fill"       "He"         "We"         "Pack"       "The"       
## [326] "The"        "Boards"     "The"        "Glass"      "Bathe"     
## [331] "Nine"       "The"        "The"        "The"        "Pages"     
## [336] "Try"        "Women"      "The"        "A"          "Code"      
## [341] "Most"       "He"         "The"        "Mince"      "The"       
## [346] "Let"        "A"          "A"          "Tack"       "Next"      
## [351] "Pour"       "Each"       "The"        "The"        "The"       
## [356] "Just"       "A"          "Our"        "Brass"      "It"        
## [361] "Feed"       "The"        "He"         "The"        "Plead"     
## [366] "Better"     "This"       "The"        "He"         "Tend"      
## [371] "It"         "Mark"       "Take"       "The"        "North"     
## [376] "He"         "Go"         "A"          "Soap"       "That"      
## [381] "He"         "A"          "Grape"      "Roads"      "Fake"      
## [386] "The"        "Smoke"      "Serve"      "Much"       "The"       
## [391] "Heave"      "A"          "It"         "His"        "The"       
## [396] "The"        "It"         "Beef"       "Raise"      "The"       
## [401] "A"          "Jerk"       "No"         "We"         "The"       
## [406] "The"        "Three"      "The"        "No"         "Grace"     
## [411] "Nudge"      "The"        "Once"       "A"          "Fasten"    
## [416] "A"          "He"         "The"        "The"        "There"     
## [421] "Seed"       "Draw"       "The"        "The"        "Hats"      
## [426] "The"        "Beat"       "Say"        "The"        "Screen"    
## [431] "This"       "The"        "He"         "These"      "The"       
## [436] "Twist"      "The"        "The"        "Xew"        "The"       
## [441] "They"       "The"        "A"          "Breakfast"  "Bottles"   
## [446] "The"        "He"         "Drop"       "The"        "Throw"     
## [451] "A"          "The"        "The"        "The"        "The"       
## [456] "Turn"       "The"        "The"        "To"         "The"       
## [461] "The"        "Dispense"   "The"        "He"         "The"       
## [466] "The"        "Fly"        "Thick"      "Birth"      "The"       
## [471] "The"        "A"          "The"        "We"         "The"       
## [476] "The"        "We"         "The"        "Five"       "A"         
## [481] "The"        "Shut"       "The"        "Crack"      "He"        
## [486] "Send"       "A"          "They"       "The"        "In"        
## [491] "A"          "Oats"       "Their"      "The"        "There"     
## [496] "Tuck"       "A"          "We"         "The"        "Take"      
## [501] "Shake"      "She"        "The"        "The"        "We"        
## [506] "Smile"      "A"          "The"        "Take"       "That"      
## [511] "The"        "The"        "Ripe"       "A"          "The"       
## [516] "The"        "The"        "This"       "She"        "The"       
## [521] "Press"      "Neat"       "The"        "The"        "The"       
## [526] "Shake"      "The"        "A"          "His"        "Flax"      
## [531] "Hurdle"     "A"          "Even"       "Peep"       "The"       
## [536] "Cheap"      "A"          "Flood"      "A"          "The"       
## [541] "Those"      "He"         "Dill"       "Down"       "Either"    
## [546] "The"        "If"         "At"         "Read"       "Fill"      
## [551] "The"        "Clams"      "The"        "The"        "Breathe"   
## [556] "It"         "A"          "A"          "A"          "A"         
## [561] "Paint"      "The"        "Bribes"     "Trample"    "The"       
## [566] "A"          "Footprints" "She"        "A"          "Prod"      
## [571] "It"         "The"        "It"         "The"        "Wake"      
## [576] "The"        "The"        "The"        "Hold"       "Next"      
## [581] "Every"      "He"         "They"       "Drive"      "Keep"      
## [586] "Sever"      "Paper"      "Slide"      "Help"       "A"         
## [591] "Stop"       "Jerk"       "Slidc"      "The"        "Light"     
## [596] "Set"        "Dull"       "A"          "Get"        "Choose"    
## [601] "A"          "He"         "There"      "The"        "Greet"     
## [606] "When"       "Sweet"      "A"          "A"          "Lush"      
## [611] "The"        "The"        "The"        "Sit"        "A"         
## [616] "The"        "Green"      "Tea"        "Pitch"      "The"       
## [621] "The"        "The"        "A"          "The"        "She"       
## [626] "The"        "Loop"       "Plead"      "Calves"     "Post"      
## [631] "Tear"       "A"          "A"          "It"         "Crouch"    
## [636] "Pack"       "The"        "Fine"       "Poached"    "Bad"       
## [641] "Ship"       "Dimes"      "They"       "The"        "The"       
## [646] "The"        "The"        "Pile"       "The"        "The"       
## [651] "The"        "The"        "A"          "The"        "The"       
## [656] "To"         "There"      "Cod"        "The"        "Dunk"      
## [661] "Hang"       "Cap"        "The"        "Be"         "Pick"      
## [666] "A"          "The"        "The"        "The"        "You"       
## [671] "Dots"       "Put"        "The"        "The"        "See"       
## [676] "Slide"      "Many"       "We"         "No"         "Dig"       
## [681] "The"        "A"          "Green"      "A"          "The"       
## [686] "A"          "The"        "The"        "Seven"      "Our"       
## [691] "The"        "It"         "One"        "Take"       "The"       
## [696] "The"        "The"        "Stop"       "The"        "The"       
## [701] "Open"       "Fish"       "Dip"        "Will"       "The"       
## [706] "The"        "The"        "He"         "Leave"      "The"       
## [711] "A"          "The"        "She"        "A"          "Small"     
## [716] "The"        "The"        "A"          "She"        "When"
```


```r
ing <- "\\b\\w*ing\\b"
sentences %>%
  str_subset(ing) %>%
  str_extract_all(ing) %>% 
  unlist() %>%
  unique()
```

```
##  [1] "spring"    "evening"   "morning"   "winding"   "living"   
##  [6] "king"      "Adding"    "making"    "raging"    "playing"  
## [11] "sleeping"  "ring"      "glaring"   "sinking"   "dying"    
## [16] "Bring"     "lodging"   "filing"    "wearing"   "wading"   
## [21] "swing"     "nothing"   "sing"      "painting"  "walking"  
## [26] "bring"     "shipping"  "puzzling"  "landing"   "thing"    
## [31] "waiting"   "whistling" "timing"    "changing"  "drenching"
## [36] "moving"    "working"
```


```r
plurals <- "\\b\\w*[^aeiousy' ]s\\b"
sentences %>% 
  str_subset(plurals) %>% #only those sentences with a plural
  str_extract_all(plurals,simplify=FALSE) %>%
  unlist() %>%
  unique() %>%
  .[!str_detect(.,"help|its")]
```

```
##   [1] "planks"     "bowls"      "lemons"     "hogs"       "hours"     
##   [6] "stockings"  "bonds"      "pants"      "kittens"    "books"     
##  [11] "keeps"      "chicks"     "leads"      "sums"       "boards"    
##  [16] "wheels"     "soldiers"   "steps"      "Cars"       "drifts"    
##  [21] "words"      "weeks"      "factors"    "parts"      "costs"     
##  [26] "eggs"       "seems"      "gifts"      "pins"       "Cats"      
##  [31] "dogs"       "friends"    "orders"     "logs"       "tropics"   
##  [36] "things"     "lists"      "contents"   "problems"   "events"    
##  [41] "results"    "curls"      "pencils"    "blocks"     "scraps"    
##  [46] "pills"      "burns"      "players"    "cobs"       "tacks"     
##  [51] "tongs"      "petals"     "Farmers"    "wonders"    "taps"      
##  [56] "Schools"    "fans"       "ribbons"    "groups"     "backs"     
##  [61] "Whitings"   "nets"       "ads"        "buyers"     "rings"     
##  [66] "islands"    "funds"      "gets"       "brothers"   "bricks"    
##  [71] "Ducks"      "flavors"    "drinks"     "others"     "pears"     
##  [76] "seats"      "spoils"     "binds"      "ruins"      "crackers"  
##  [81] "needs"      "hands"      "hops"       "cans"       "clouds"    
##  [86] "walls"      "frocks"     "plans"      "brings"     "years"     
##  [91] "records"    "Boards"     "rows"       "secrets"    "objects"   
##  [96] "fails"      "woods"      "sticks"     "seeds"      "wanders"   
## [101] "cats"       "winds"      "colds"      "fevers"     "asks"      
## [106] "means"      "items"      "Roads"      "cuts"       "trims"     
## [111] "lingers"    "cents"      "news"       "minds"      "Hats"      
## [116] "robins"     "mats"       "protects"   "coins"      "rags"      
## [121] "cuffs"      "pockets"    "lasts"      "buns"       "kinds"     
## [126] "chairs"     "stems"      "bills"      "figs"       "sparks"    
## [131] "lanterns"   "hearts"     "Oats"       "eyelids"    "clowns"    
## [136] "mails"      "requests"   "ends"       "clips"      "trinkets"  
## [141] "Clams"      "matters"    "reads"      "sockets"    "designs"   
## [146] "Footprints" "shrubs"     "outdoors"   "ears"       "speaks"    
## [151] "kids"       "guests"     "runs"       "rocks"      "looks"     
## [156] "grows"      "facts"      "flaps"      "waters"     "maps"      
## [161] "pods"       "fields"     "amounts"    "informs"    "Dots"      
## [166] "faults"     "blows"      "seals"      "sheets"     "troops"    
## [171] "puts"       "bombs"      "streets"    "turns"
```

## 14.4.1 Exercises

### 1 
_Find all words that come after a “number” like “one”, “two”, “three” etc. Pull out both the number and the word._


```r
numbers <- str_replace_all("(one two three four five six seven eight nine ten eleven twelve thirteen fourteen fifteen sixteen seventeen eighteen nineteen twenty)"," ","|")
numbers
```

```
## [1] "(one|two|three|four|five|six|seven|eight|nine|ten|eleven|twelve|thirteen|fourteen|fifteen|sixteen|seventeen|eighteen|nineteen|twenty)"
```

```r
pattern <- str_c(numbers,"\\s(\\w+)")
pattern
```

```
## [1] "(one|two|three|four|five|six|seven|eight|nine|ten|eleven|twelve|thirteen|fourteen|fifteen|sixteen|seventeen|eighteen|nineteen|twenty)\\s(\\w+)"
```

```r
sentences %>% 
  str_subset(pattern) %>%
  str_match(pattern)
```

```
##       [,1]            [,2]      [,3]      
##  [1,] "ten served"    "ten"     "served"  
##  [2,] "one over"      "one"     "over"    
##  [3,] "seven books"   "seven"   "books"   
##  [4,] "two met"       "two"     "met"     
##  [5,] "sixteen weeks" "sixteen" "weeks"   
##  [6,] "two factors"   "two"     "factors" 
##  [7,] "one and"       "one"     "and"     
##  [8,] "three lists"   "three"   "lists"   
##  [9,] "seven is"      "seven"   "is"      
## [10,] "two when"      "two"     "when"    
## [11,] "one floor"     "one"     "floor"   
## [12,] "ten inches"    "ten"     "inches"  
## [13,] "one with"      "one"     "with"    
## [14,] "one war"       "one"     "war"     
## [15,] "one button"    "one"     "button"  
## [16,] "six minutes"   "six"     "minutes" 
## [17,] "ten years"     "ten"     "years"   
## [18,] "one in"        "one"     "in"      
## [19,] "ten chased"    "ten"     "chased"  
## [20,] "one like"      "one"     "like"    
## [21,] "two shares"    "two"     "shares"  
## [22,] "two distinct"  "two"     "distinct"
## [23,] "one costs"     "one"     "costs"   
## [24,] "ten two"       "ten"     "two"     
## [25,] "five robins"   "five"    "robins"  
## [26,] "four kinds"    "four"    "kinds"   
## [27,] "one rang"      "one"     "rang"    
## [28,] "ten him"       "ten"     "him"     
## [29,] "three story"   "three"   "story"   
## [30,] "ten by"        "ten"     "by"      
## [31,] "one wall"      "one"     "wall"    
## [32,] "three inches"  "three"   "inches"  
## [33,] "ten your"      "ten"     "your"    
## [34,] "six comes"     "six"     "comes"   
## [35,] "one before"    "one"     "before"  
## [36,] "three batches" "three"   "batches" 
## [37,] "two leaves"    "two"     "leaves"
```

### 2
_Find all contractions. Separate out the pieces before and after the apostrophe._

```r
pattern0 <- "([Ii]t's|[Ll]et's)|((\\w+)'([^s ]+))" #match all words with a "'" except for possesives
pattern <- "(\\w+)'(\\w+)"

sentences %>% 
  str_subset(pattern0) %>%
  str_match(pattern)
```

```
##      [,1]    [,2]  [,3]
## [1,] "It's"  "It"  "s" 
## [2,] "don't" "don" "t" 
## [3,] "Let's" "Let" "s" 
## [4,] "It's"  "It"  "s" 
## [5,] "don't" "don" "t" 
## [6,] "don't" "don" "t"
```


## 14.4.5.1 Exercises

### 1
_Replace all forward slashes in a string with backslashes._


```r
unixpath <- getwd()
unixpath
```

```
## [1] "/Users/jmaloof/git/r_club_members/Rclub-r4ds_Julin.Maloof/R-club-July-19"
```

```r
unixpath %>% str_replace_all("/","\\\\")
```

```
## [1] "\\Users\\jmaloof\\git\\r_club_members\\Rclub-r4ds_Julin.Maloof\\R-club-July-19"
```

Can't figure out how to do it and get single backslashes

_Implement a simple version of str_to_lower() using replace_all()._

```r
pattern <- letters
names(pattern) <- LETTERS
pattern
```

```
##   A   B   C   D   E   F   G   H   I   J   K   L   M   N   O   P   Q   R 
## "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q" "r" 
##   S   T   U   V   W   X   Y   Z 
## "s" "t" "u" "v" "w" "x" "y" "z"
```

```r
sentences %>% str_replace_all(pattern) %>% head()
```

```
## [1] "the birch canoe slid on the smooth planks." 
## [2] "glue the sheet to the dark blue background."
## [3] "it's easy to tell the depth of a well."     
## [4] "these days a chicken leg is a rare dish."   
## [5] "rice is often served in round bowls."       
## [6] "the juice of lemons makes fine punch."
```


_Switch the first and last letters in words. Which of those strings are still words?_

```r
scramble <- words %>% str_replace_all("\\b(\\w){1}(\\w*)(\\w{1})\\b","\\3\\2\\1")
head(scramble)
```

```
## [1] "a"        "ebla"     "tboua"    "ebsoluta" "tccepa"   "tccouna"
```


```r
intersect(scramble,words)
```

```
##  [1] "a"          "america"    "area"       "dad"        "dead"      
##  [6] "lead"       "read"       "depend"     "god"        "educate"   
## [11] "else"       "encourage"  "engine"     "europe"     "evidence"  
## [16] "example"    "excuse"     "exercise"   "expense"    "experience"
## [21] "eye"        "dog"        "health"     "high"       "knock"     
## [26] "deal"       "level"      "local"      "nation"     "on"        
## [31] "non"        "no"         "rather"     "dear"       "refer"     
## [36] "remember"   "serious"    "stairs"     "test"       "tonight"   
## [41] "transport"  "treat"      "trust"      "window"     "yesterday"
```


## 14.4.6.1 Exercises

### 1
_Split up a string like "apples, pears, and bananas" into individual components._

```r
str_split("apples, pears, and bananas",pattern = "(, (and )?)")
```

```
## [[1]]
## [1] "apples"  "pears"   "bananas"
```


### 2
_Why is it better to split up by boundary("word") than " "?_

words could be split by other characters than " "

### 3
_What does splitting with an empty string ("") do? Experiment, and then read the documentation._

splits on every character

```r
str_split("apples, pears, and bananas",pattern = "")
```

```
## [[1]]
##  [1] "a" "p" "p" "l" "e" "s" "," " " "p" "e" "a" "r" "s" "," " " "a" "n"
## [18] "d" " " "b" "a" "n" "a" "n" "a" "s"
```


## 14.5.1 Exercises

### 1
How would you find all strings containing \ with regex() vs. with fixed()?

```r
test <- c("has\\a\\backslash","no/backslash")

str_subset(test,"\\\\") %>% writeLines
```

```
## has\a\backslash
```

```r
str_subset(test,fixed("\\")) %>% writeLines
```

```
## has\a\backslash
```


### 2
What are the five most common words in sentences?


```r
sentences %>% 
  str_split(boundary("word")) %>%
  unlist() %>%
  str_to_lower() %>%
  tibble(word=.) %>%
  count(word) %>%
  arrange(desc(n)) %>%
  filter(min_rank(desc(n)) < 6)
```

```
## # A tibble: 5 × 2
##    word     n
##   <chr> <int>
## 1   the   751
## 2     a   202
## 3    of   132
## 4    to   123
## 5   and   118
```

## 14.7.1 Exercises

Find the stringi functions that:

_Count the number of words._


```r
library(stringi)
sentences %>% head()
```

```
## [1] "The birch canoe slid on the smooth planks." 
## [2] "Glue the sheet to the dark blue background."
## [3] "It's easy to tell the depth of a well."     
## [4] "These days a chicken leg is a rare dish."   
## [5] "Rice is often served in round bowls."       
## [6] "The juice of lemons makes fine punch."
```

```r
sentences %>% head() %>% stri_count_words()
```

```
## [1] 8 8 9 9 7 7
```


_Find duplicated strings._

```r
stri_duplicated(c("one","two","three","one"))
```

```
## [1] FALSE FALSE FALSE  TRUE
```

Generate random text.


```r
stri_rand_strings(5,10)
```

```
## [1] "0HnKUz49qt" "n2gsmKQVoh" "8MsaPlYKgQ" "3AvFZZUsUu" "YnvR6LR9qG"
```

How do you control the language that stri_sort() uses for sorting?


```r
stri_sort(words) %>% head()
```

```
## [1] "a"        "able"     "about"    "absolute" "accept"   "account"
```

```r
stri_sort(words,opts_collator = list(locale="fr")) %>% head()
```

```
## [1] "a"        "able"     "about"    "absolute" "accept"   "account"
```

