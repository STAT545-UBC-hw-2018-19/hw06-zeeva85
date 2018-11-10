---
title: "hw06"
author: Seevasant Indran
date: "09 November, 2018"   
output:  
  html_document:
    keep_md: true
    toc: true
    toc_depth: 2
    theme: readable
---
   
<details open>
  <summary>
Packages required
  </summary>  
     
-   [tidyverse](http://tidyverse.tidyverse.org/) (includes [ggplot2](http://ggplot2.tidyverse.org/), [dplyr](http://dplyr.tidyverse.org/), [tidyr](http://tidyr.tidyverse.org/), [readr](http://readr.tidyverse.org/), [tibble](http://tibble.tidyverse.org/))  
- [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html)    
- [knitr](https://cran.r-project.org/web/packages/knitr/index.html)    
  
**Install by running**  
```
install.packages("packageName", dependencies = TRUE)
```
</details>  
   
   







# Task 1: Character data

## Exercises 14.2.5

### Question 1

**In code that doesn't use stringr, you'll often see `paste()` and `paste0()`. What's the difference between the two functions?**

**`paste (..., sep = " ", collapse = NULL)`**   
**`paste0(..., collapse = NULL)`**      

Arguments  

...   	  - one or more R objects, to be converted to character vectors.  
sep   	  - a character string to separate the terms. Not NA_character_.  
collapse  - an optional character string to separate the results. Not NA_character_.  


**`str_c(..., sep = "", collapse = NULL)`**   

Arguments  

...	- One or more character vectors. Zero length arguments are removed.    
Short arguments are recycled to the length of the longest.   
Like most other R functions, missing values are "infectious": whenever a missing value is combined with another string the result will always be missing. Use str_replace_na() to convert NA to "NA"   
sep	- String to insert between input vectors.   
collapse	- Optional string used to combine input vectors into single string.   

Value    

If collapse = NULL (the default) a character vector with length equal to the longest input string. If collapse is non-NULL, a character vector of length 1.   


```r
## this adds whitespaces (sep = " ") between the strings by default
paste("arm","pit")
```

```
## [1] "arm pit"
```

```r
## This does not add spaces between the strings
paste0("arm","pit") 
```

```
## [1] "armpit"
```

![armpit][armpit]

`paste()` adds a space (by default) via the `sep = " "` argument. `paste0()` does not have the `sep =` argument built it.   

**What stringr function are they equivalent to?**

The `str_c()` from `stringr::` is the equavalent if the `base::` `paste0()` and `paste(..., sep = " ")`. 



```r
## This is the same as paste0("light", "saber")
str_c("light", "saber")
```

```
## [1] "lightsaber"
```

```r
# paste("light", "saber", sep = " ")
str_c("half", "sister", sep = " ")
```

```
## [1] "half sister"
```


```r
# NA is coerced into character 
paste(c(NA, "BrO")) # Two vector example 
```

```
## [1] "NA"  "BrO"
```

```r
paste0("What kind of fish is made out of 2 sodium atoms - ", 2, NA) # One vector example
```

```
## [1] "What kind of fish is made out of 2 sodium atoms - 2NA"
```

```r
# The 'sep = "-" is ignored in the in paste0()
paste0("twenty", "two", sep = "-" ) 
```

```
## [1] "twentytwo-"
```

```r
# paste() uses the 'sep="-"` argument
paste("state", "of", "the", "art",  sep = "-" )
```

```
## [1] "state-of-the-art"
```

Example above shows that that when the argument is used in `paste0(..., sep ="")` it is ignored and the value of the argument is taken as an additional string to be concatenated, but not for the `paste()`. Both the `paste` and the `paste0` coerces `NA` into a character vector. The `str_c` differs from in the way where it does not coereces NA into strings and drops out the character vector to be concatenated.

[armpit]: https://i.chzbgr.com/full/5911451136/h60ACC25C/

### Question 2

**In your own words, describe the difference between the `sep` and `collapse` arguments to `str_c()`.**


```r
x <- c("moon")
y <- c("shine", "light")
z <- c("full", "moon")

# The 'sep=' does not work here, because a single argumnent 'c()' is passed
str_c ( z , sep = "____" ) 
```

```
## [1] "full" "moon"
```

```r
str_c ( c("full", "moon") , sep = "____") # output same as above, does not work 
```

```
## [1] "full" "moon"
```

```r
## For it to work use the 'collapse=' instead of 'sep='

str_c ( z , collapse = "_" )
```

```
## [1] "full_moon"
```

```r
str_c ( c("full", "moon" ), collapse = "_")
```

```
## [1] "full_moon"
```

```r
# Altenatively, this works because the strings are passed as individual arguments to the 'sep=' argument.
str_c ("full", "moon" , sep = "_" )
```

```
## [1] "full_moon"
```

```r
# However, not useful many arguments. would you do this 26 times for all the alphabets ?
str_c ("a", "b", "c", "e", "f", "h", "i", "j", "k", "l", "m" , sep = "_" )
```

```
## [1] "a_b_c_e_f_h_i_j_k_l_m"
```

```r
## This would not work in using the collapse, just concatenates
str_c ("full", "moon" , collapse = "_" )
```

```
## [1] "fullmoon"
```

```r
# More examples using vector recycling and switching sep and collapse separators
str_c(x ,y,  collapse = "_")
```

```
## [1] "moonshine_moonlight"
```

```r
str_c(x, y, sep = " ", collapse = "_") # notice the two " " (whitespaces) and one "_"
```

```
## [1] "moon shine_moon light"
```

```r
str_c(x, y, sep = "_", collapse = " ") # notice the two "_" (underscore) and one " " (whitespace)
```

```
## [1] "moon_shine moon_light"
```

```r
# This would not work 
# str_c (letters, sep = "___" )

# This is the best solution
# str_c (letters, collapse = "___" )
```

By default the `str_c()` concatenates separate vectors- it doesnt collapse elements of a vector. By using `c()` a multiple element vector is passed as a single argument to the `str_c()` function. In such casses using the `sep=` argument with a `c()` does not concatenates, it just returns the separe vector as it was originally. This is where the `collapse=` argument is useful.

The `collapse =` arguments works by concatenating elements of a **single** vector with multiple elements (ect.. that was passed using c() or assigned using the c(). The collapse arguments looks for multiple elemental vectors to be concatenated tupple wise.


### Question 3

**Use `str_length()` and `str_sub()` to extract the middle character from a string.**

`str_sub(string, start = 1L, end = -1L)`


```r
oddString <- "isOddString"  # assign odd character number to oddSring

str_length(oddString) # Counts the string length
```

```
## [1] 11
```

```r
# subset string start at the middle by diving length with 2, use ceeling to round off to celing value if there is a floating point value instead of interger value
str_sub(oddString, ceiling(str_length(oddString)/2), ceiling(str_length(oddString)/2))
```

```
## [1] "S"
```

```r
# let's define a function to get the middle index of a string with odd number of characters
str_middle_odd <- function(i) {
  ceiling(str_length(i) / 2)
}

# extract the middle character of string_odd, which is u

str_sub(oddString, str_middle_odd(oddString), str_middle_odd(oddString))
```

```
## [1] "S"
```


**What will you do if the string has an even number of characters?**

We try to get the middle two characters if the number of characters in a string is an even number.


```r
evenString <- "isEvenString" # assign even character number to evenString

str_length(evenString)
```

```
## [1] 12
```

```r
str_middle_even <- function(i) {
  str_length(i) / 2
}

str_sub(evenString, ceiling(str_length(evenString)/2), ceiling(str_length(evenString)/2)+1)
```

```
## [1] "nS"
```

```r
# extract the middle character of string_odd, which is "nS"

str_sub(evenString, str_middle_even(evenString), str_middle_even(evenString)+ 1)
```

```
## [1] "nS"
```

### Question 4

**What does `str_wrap()` do? When might you want to use it?**

According to `?str_wrap`, this function tries to wrap paragraph into multiple lines using an algorithm called "Knuth-Plass paragraph wrapping algorithm". An introduction of this algorithm can be found [here](https://www.ugrad.cs.ubc.ca/~cs490/2015W2/lectures/Knuth.pdf).


```r
# The Ultimate Hitchhiker's Guide by Douglas Adams


paragraphwrap <- "Far out in the uncharted backwaters of the unfashionable end of the western spiral arm of the Galaxy lies a small unregarded yellow sun.Orbiting this at a distance of roughly ninety-two million miles is an utterly insignificant little blue green planet whose ape-descended life forms are so amazingly primitive that they still think digital watches are a pretty neat idea. This planet has-or rather had-a problem, which was this: most of the people on it were unhappy for pretty much of the time. Many solutions were suggested for this problem, but most of these were largely concerned with the movements of small green pieces of paper, which is the movements of small green pieces of paper, which is odd because on the whole it wasn't the small green pieces of paper that were unhappy."


cat (paragraphwrap) # print it without wrapping
```

```
## Far out in the uncharted backwaters of the unfashionable end of the western spiral arm of the Galaxy lies a small unregarded yellow sun.Orbiting this at a distance of roughly ninety-two million miles is an utterly insignificant little blue green planet whose ape-descended life forms are so amazingly primitive that they still think digital watches are a pretty neat idea. This planet has-or rather had-a problem, which was this: most of the people on it were unhappy for pretty much of the time. Many solutions were suggested for this problem, but most of these were largely concerned with the movements of small green pieces of paper, which is the movements of small green pieces of paper, which is odd because on the whole it wasn't the small green pieces of paper that were unhappy.
```

```r
cat(str_wrap(paragraphwrap, width = 80, indent = 4)) # let's try to print it after wrapping, and seperate by new line
```

```
##     Far out in the uncharted backwaters of the unfashionable end of the western
## spiral arm of the Galaxy lies a small unregarded yellow sun.Orbiting this at a
## distance of roughly ninety-two million miles is an utterly insignificant little
## blue green planet whose ape-descended life forms are so amazingly primitive
## that they still think digital watches are a pretty neat idea. This planet has-
## or rather had-a problem, which was this: most of the people on it were unhappy
## for pretty much of the time. Many solutions were suggested for this problem,
## but most of these were largely concerned with the movements of small green
## pieces of paper, which is the movements of small green pieces of paper, which
## is odd because on the whole it wasn't the small green pieces of paper that were
## unhappy.
```

The length of characters in each line is kept at 80 and there is an `indent` and `exdent` argument.

### Question 5

**What does `str_trim()` do?**

`str_trim()` removes white spaces from starting and end of a starting.


```r
# create letters with 8 space indent 
spaces = str_wrap(letters, indent = 8)
spaces
```

```
##  [1] "        a" "        b" "        c" "        d" "        e"
##  [6] "        f" "        g" "        h" "        i" "        j"
## [11] "        k" "        l" "        m" "        n" "        o"
## [16] "        p" "        q" "        r" "        s" "        t"
## [21] "        u" "        v" "        w" "        x" "        y"
## [26] "        z"
```

```r
# use str_trim() to delete all spaces at the beginning and the end of the string
removeSpaces <- str_trim(spaces)
removeSpaces
```

```
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"
```


**What's the opposite of `str_trim()`?**

`str_pad()` is the opposite of `str_trim` to add whitespace or other characters under the `pad=` argument.

`str_pad(string, width, side = c("left", "right", "both"), pad = " ")`

Re-create letters with spaces using `str_pad()`

We try to pad `string_no_sapces` back to `string_with_spaces`. Notice that, we need to add two spaces both at the beginning and the end of the string, which means we need to make the width of the final string 4 characters more.


```r
# use white spaces as pad value with a width of 9 (character + 8 whitespaces to the left)
str_pad(letters, 9, side = c("left"), pad = " ")
```

```
##  [1] "        a" "        b" "        c" "        d" "        e"
##  [6] "        f" "        g" "        h" "        i" "        j"
## [11] "        k" "        l" "        m" "        n" "        o"
## [16] "        p" "        q" "        r" "        s" "        t"
## [21] "        u" "        v" "        w" "        x" "        y"
## [26] "        z"
```

```r
# can even bad both sides
str_pad(letters, 9, side = c("both"), pad = " ")
```

```
##  [1] "    a    " "    b    " "    c    " "    d    " "    e    "
##  [6] "    f    " "    g    " "    h    " "    i    " "    j    "
## [11] "    k    " "    l    " "    m    " "    n    " "    o    "
## [16] "    p    " "    q    " "    r    " "    s    " "    t    "
## [21] "    u    " "    v    " "    w    " "    x    " "    y    "
## [26] "    z    "
```

```r
# check if the pad both sides is trimmed back to its original state
all(str_pad(letters, 9, side = c("both"), pad = " ") %>% 
  str_trim() == letters) # equality check returns TRUE only if all 26 letters is evaluated TRUE
```

```
## [1] TRUE
```

### Question 6

**Write a function that turns (e.g.) a vector `c("a", "b", "c")` into the string `a`, `b`, and `c`. Think carefully about what it should do if given a vector of length 0, 1, or 2.**

Assuming the final string is "a, b, and c", we make the following rules for the function:

- If the length of vector is 0, we return an empty string (""); 
- If the length of vector is 1, we return the string inside the vector (e.g., "a");
- If the length of vector is 2, we follow the same format as the length is 3 or above, which means we return, for example, "a, and b".


```r
# vector into a string
str_vec2str <- function(x) { 
  packages <- c("tidyverse") # specify package required for function
if (length(setdiff(packages, rownames(installed.packages()))) > 0) {
  install.packages(setdiff(packages, rownames(installed.packages())))  
} # if tidyverse are not installed install then load, if installed, load tidyverse
library(tidyverse) 
  
  if (length(x) == 0) {
    return("")
  } else if (length(x) == 1) {
    return(x[length(x)])
  } else {
    # evaluate length of vector, exclude last element, then collapse to a single string
    singleVecNolast <- str_c(x[-length(x)], collapse = ", ")
    # concatenate vector without last element with last element using `sep = "and"`
    vec2string <- str_c(singleVecNolast, x[length(x)], sep = ", and ")
    # return the final string
    return(vec2string)
  }
}
```

Use `testthat` to test function.


```r
# create four vectors to test
zerostr <- c()
onestr <- c("a")
twostr  <- c("a", "b")
threestr <- c ("a", "b", "c")


  # zerostr : vector length of 0 returns ""
  expect_equal(str_vec2str(zerostr), "")
  
  # onestr: vector length of 1 returns "a"
  expect_equal(str_vec2str(onestr), "a")
  
  # twostr: vector length of 2 returns "a, and b"
  expect_equal(str_vec2str(twostr), "a, and b")
  
  # threestr: vector length of 3 returns (a,b, and c)
  expect_equal(str_vec2str(threestr), "a, b, and c")
```

Check if all returns TRUE, No error is good.


## Exercises 14.3.1.1

### Question 1

**Explain why each of these strings don't match a `\`: `"\"`, `"\\"`, `"\\\"`.**

The `\` (blackslash) is an escape character in both `R` and `regex`. Backslash is used to start an escape sequence inside character constants. Escaping a character not in the following table is an error.

`\n`	newline  
`\r`	carriage return  
`\t`	tab   
`\b`	backspace   
`\a`	alert (bell)   
`\f`	form feed    
`\v`	vertical tab   
`\\`	backslash \    
`\'`	ASCII apostrophe '    
`\"`	ASCII quotation mark "    
`\``	ASCII grave accent (backtick) `    
`\nnn`	character with given octal code (1, 2 or 3 digits)    
`\xnn`	character with given hex code (1 or 2 hex digits)    
`\unnnn`	Unicode character with given code (1--4 hex digits)     
`\Unnnnnnnn`	Unicode character with given code (1--8 hex digits)     
     

   
 When the `"\"` is used in `R`, `R` takes precedence and assumes its a special character for ussages such as `\n` or `\t` until and unless another special character is followed then it becomes a literal puncuation character. In `regex` the `\` is also a special character along with 12 other special characters and these needs to be escaped with a `\` before using them for literal special characters.  


    
 `\` - is a special character in R, when used singly leads to an incomplete expression error, `Error: Incomplete expression: "\"` in `R` when is evaluated in code chunk or results in the continuation `+` character when its run on the console.
 

```r
# viewing the output of `\` as an code chunk error or a continuation character when is run in console
cat("\") 
```
 
     
- `\\` is evaluated as an escape character followed by a backslash (`\`), this will be sent to the regular expression engine as a single back slash `\`. Remember that in regex the `\` is also a special character therefore the expression in incomplete in regex.    


```r
# viewing the output of `\\`
cat("\\") # prints "\"
```

     
- `\\\` is evaluated as an escape character followed by a backslash (`\`), and then an escape again this will be sent to the regular expression engine as a single back slash `\`    


```r
# viewing the output of `\` as an code chunk error or a continuation character when is run in console
cat("\\\")
```



### Question 2

**How would you match the sequence `"'\` ?**

To match the `"'\`, `R` function has to produce a string output of `"'\` and there are few ways to go about to match the string. As a notes single quotes need to be escaped by backslash in single-quoted strings `' ' '`, and double quotes in double-quoted strings `" " "`. In regex double quotes and single quotes are not special characters. To match the sequence `"'\`, we will use strings encaspsulated in double quotes, hence the double quotes need to be escaped in `R` like so `\"` but not in regex as mentioned. The single quotes are not affected so it can be passed as it is. The `\` are special characters in `R` and regex hence it needs to be passed to regex like this `\\`. To obtain that string `R` it needs to be escaped twice resulting in 4 backslashes `\\\\`. 

The string to match the sequence for the question is `"\"'\\\\"` or every character can be escaped as such `\"\'\\\\"`




```r
Q14.3.1.1.1 <- "\"'\\" # create and assign matching string
Q14.3.1.1.2 <- "hello\"'\\world" # create and assign matching string

cat (Q14.3.1.1.1) # check if the string prints the same as the question.
```

```
## "'\
```

```r
str_view(Q14.3.1.1.1, pattern = "\"'\\\\") # Match Q14.3.1.1.1
```

<!--html_preserve--><div id="htmlwidget-8fa256de8a053e7ab6c6" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-8fa256de8a053e7ab6c6">{"x":{"html":"<ul>\n  <li><span class='match'>\"'\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
str_view(Q14.3.1.1.2, pattern = "\"\'\\\\") # Match Q14.3.1.1.2
```

<!--html_preserve--><div id="htmlwidget-fc9f688c9e3504e1f761" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-fc9f688c9e3504e1f761">{"x":{"html":"<ul>\n  <li>hello<span class='match'>\"'\\<\/span>world<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


### Question 3

**What patterns will the regular expression \..\..\.. match? How would you represent it as a string?**

The `\..\..\..` will match a 6 characters string, a fullstop followed by any character in three sucessions in a row such as `.a.n.y`. 


```r
Q14.3.1.1.3 <- ".a.n.y"


str_view(Q14.3.1.1.3, pattern = "\\..\\..\\..")
```

<!--html_preserve--><div id="htmlwidget-52ad2d3ddcf7daf46315" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-52ad2d3ddcf7daf46315">{"x":{"html":"<ul>\n  <li><span class='match'>.a.n.y<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Exercises 14.3.2.1

### Question 1

**How would you match the literal string `"$^$"`?**

The `$` and `^` are special characters in regex therefore the woould need to be escaped, therfore the output of `R` should be `\$\^\$` to the regex engine. The string for that evaluate should escape the backslashes. To match the string in the question `\\$\\^\\$` would be the pattern.


```r
Q14.3.2.1.1 <- "$^$"

cat ("\\$\\^\\$") # Test string output for regex \$\^\$
```

```
## \$\^\$
```

```r
str_view(Q14.3.2.1.1, pattern = "\\$\\^\\$")
```

<!--html_preserve--><div id="htmlwidget-423692c29a8163a9465d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-423692c29a8163a9465d">{"x":{"html":"<ul>\n  <li><span class='match'>$^$<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Question 2

**Given the corpus of common words in `stringr::words`, create regular expressions that find all words that fulfilling the following requirements.**

**Words starting with "y".**


```r
# find words starting with "y" using the "^y"
str_view(words, pattern = "^y", match = TRUE) # Use match = TRUE to only show words that match
```

<!--html_preserve--><div id="htmlwidget-2510879afd91ec6c74d3" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-2510879afd91ec6c74d3">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**Words ending with "x".**


```r
# find words end with "x" using the "x$"
str_view(words, pattern = "x$", match = TRUE) # Use match = TRUE to only show words that match
```

<!--html_preserve--><div id="htmlwidget-bb1bee7760d356bc3c81" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-bb1bee7760d356bc3c81">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Are exactly three letters long. (Don't cheat by using `str_length()`!).**


```r
# use the random sampling where to keep markdown shorter
str_view(sample(words, 50), pattern = "\\b...\\b", match = TRUE) # use the boundry before and after three any characters
```

<!--html_preserve--><div id="htmlwidget-e8d4fefb27627ee825f6" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-e8d4fefb27627ee825f6">{"x":{"html":"<ul>\n  <li><span class='match'>hit<\/span><\/li>\n  <li><span class='match'>sit<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Requirement 4: have seven letters or more.**


```r
str_view(sample(words, 50),  pattern = "\\b.......", match = TRUE) # use one boundry to define seven letters and more
```

<!--html_preserve--><div id="htmlwidget-eef34bbc17d78b9087a0" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-eef34bbc17d78b9087a0">{"x":{"html":"<ul>\n  <li><span class='match'>differe<\/span>nce<\/li>\n  <li><span class='match'>evidenc<\/span>e<\/li>\n  <li><span class='match'>between<\/span><\/li>\n  <li><span class='match'>station<\/span><\/li>\n  <li><span class='match'>million<\/span><\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>importa<\/span>nt<\/li>\n  <li><span class='match'>educate<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


## Exercises 14.3.3.1

### Question 1

**Create regular expressions to find all words that start with a vowel.**


```r
# find words that start with a vowel
str_view(sample(words, 20), pattern = "^[aeiou]", match = TRUE) 
```

<!--html_preserve--><div id="htmlwidget-26d0014db7c587bb0adb" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-26d0014db7c587bb0adb">{"x":{"html":"<ul>\n  <li><span class='match'>i<\/span>n<\/li>\n  <li><span class='match'>e<\/span>ast<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions that only contain consonants. (Hint: thinking about matching "not"-vowels.)**


```r
# find words that without a vowel using the not "^" special character
str_view(words, pattern = "^aeiou", match = T)
```

<!--html_preserve--><div id="htmlwidget-d3285006b7b071aba546" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-d3285006b7b071aba546">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

There is no words without all the vowels

**Create regular expressions that only end with `ed`, but not with `eed`.**


```r
# find words that end with "ed" but not "eed"
str_view(words, pattern = "[^e]ed$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-2a778c146da7cc4d8656" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-2a778c146da7cc4d8656">{"x":{"html":"<ul>\n  <li><span class='match'>bed<\/span><\/li>\n  <li>hund<span class='match'>red<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions that only end with `ing` or `ise`.**


```r
# find words that end with "ing" or "ise"
str_view(words, pattern = "ing$|ise$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-8cbbf58f52a23aa64388" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-8cbbf58f52a23aa64388">{"x":{"html":"<ul>\n  <li>advert<span class='match'>ise<\/span><\/li>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>exerc<span class='match'>ise<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>morn<span class='match'>ing<\/span><\/li>\n  <li>otherw<span class='match'>ise<\/span><\/li>\n  <li>pract<span class='match'>ise<\/span><\/li>\n  <li>ra<span class='match'>ise<\/span><\/li>\n  <li>real<span class='match'>ise<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>surpr<span class='match'>ise<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
### Question 2

** Create regular expressions that the rule is empirically verified the rule "i" before "e" except after "c".**

The sentenece is rather confusing, this is what it means, check for words that have "ie", and without any "cie". When "c" present, instead look for "cei"


```r
str_subset(words, pattern = "[^c]ie|cei") # subset not "c", before ie", and "cei"
```

```
##  [1] "achieve"    "believe"    "brief"      "client"     "die"       
##  [6] "experience" "field"      "friend"     "lie"        "piece"     
## [11] "quiet"      "receive"    "tie"        "view"
```


### Question 3

**Create regular expressions that the rule is empirically verified is "q" always followed by a "u"?**

To check if "q" is always followed by "u", we can check if having "q" not (^q) followed by "u" results in an output. We will also use the test that package to do an expect identical test.


```r
# is "q followed by not u" non elegant manual way

str_view(words, pattern = "q[^u]", match = T)
```

<!--html_preserve--><div id="htmlwidget-5a92e32a0a50be2ba657" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5a92e32a0a50be2ba657">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# is "q followed by not u" more systematic elegant method, 

test_that("if 'q' is always followed by a 'u'", {
 expect_failure(expect_identical(str_view(words, pattern = "[^u]", match = T), 
                                 str_view(words, pattern = "qu", match = T)))
})

# could have been done like below, but would produce error when knitting, this code chunk would fail

# expect_identical(str_view(words, pattern = "[^u]", match = T), str_view(words, pattern = "qu", match = T))
```

No messages is good message, the verdict is q always is followed by a u.

### Question 4

**Write a regular expression that matches a word if it's probably written in British English, not American English.**

Some words taken from ()[https://www.spellzone.com/pages/british-american.cfm]


```r
# copy pasted from a a table from the website above and it is in a weird format
britUSAwords <- c("cancelled
counsellor
equalled
fuelling
fuelled
grovelling
canceled
counselor
equaled
fueling
fueled
groveling")

# use str _split to split words into individual words 
britUSAwords <- str_split(britUSAwords[1], pattern = "\n") 


# british spelling has often "ll" followed by any character.
str_view(britUSAwords, pattern = ".+l{2}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-7757e0e5606a49ae80ad" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-7757e0e5606a49ae80ad">{"x":{"html":"<ul>\n  <li><span class='match'>c(\"cancelled\", \"counsellor\", \"equalled\", \"fuelling\", \"fuelled\", \"grovell<\/span>ing\", \"canceled\", \"counselor\", \"equaled\", \"fueling\", \"fueled\", \"groveling\")<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Question 5

**Create a regular expression that will match telephone numbers as commonly written in your country.**

The telephone numbers in Malaysia typically is like so:- "+60 (12) 110 0101". 


```r
# create some malaysia, canada, and singapore numbers to test
telNum <- c("+60 12 110 0101", "+1 778 8464 273", "+65  1110 0000")

# use a regex to detect telephone numbers in Canada
str_view(telNum, pattern = "\\+60 [1-9][1-9] [:digit:]{3} [:digit:]{4}")
```

<!--html_preserve--><div id="htmlwidget-1f373a029586273ed992" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-1f373a029586273ed992">{"x":{"html":"<ul>\n  <li><span class='match'>+60 12 110 0101<\/span><\/li>\n  <li>+1 778 8464 273<\/li>\n  <li>+65  1110 0000<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Exercises 14.3.4.1

### Question 1

**Describe the equivalents of `?`, `+`, `*` in {m,n} form.**

- `?` is `{0,1}`
- `+` is `{1,}`
- `*` is `{0,}`

### Question 2

**Describe in words what these regular expressions match: (read carefully to see if I'm using a regular expression or a string that defines a regular expression.)**

- `^.*$` - is a regex expression that matches any character at the start with any number of character till the end. It matches any chracter except a new line.
- "\\{.+\\}" - is a string and is evaluated and sent to regex as `\{.+\}`. This matches 1 or more number of any character(s)/word which has to be inside a literal `{}` curly braces as such `{movember}`.
- `\d{4}-\d{2}-\d{2}` is a regular expresion that matches a string looks like a date, like so`2018-11-10`, d is digit in regular expression
- `"\\\\{4}"` is a string that is sent to regex as `\\{4}`. Regex escapes the first and evaluated the second blackslash as a litreal `\`, because it is immidietly followed by {4} it does not need to be escaped matching string `\\\\`.

### Question 3

**Create regular expressions to find all words that start with three consonants.**


```r
# words start with three consonants
str_view(words, pattern = "^[^aeoiu]{3}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-1e1881165437503a6a6a" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-1e1881165437503a6a6a">{"x":{"html":"<ul>\n  <li><span class='match'>Chr<\/span>ist<\/li>\n  <li><span class='match'>Chr<\/span>istmas<\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>sch<\/span>eme<\/li>\n  <li><span class='match'>sch<\/span>ool<\/li>\n  <li><span class='match'>str<\/span>aight<\/li>\n  <li><span class='match'>str<\/span>ategy<\/li>\n  <li><span class='match'>str<\/span>eet<\/li>\n  <li><span class='match'>str<\/span>ike<\/li>\n  <li><span class='match'>str<\/span>ong<\/li>\n  <li><span class='match'>str<\/span>ucture<\/li>\n  <li><span class='match'>sys<\/span>tem<\/li>\n  <li><span class='match'>thr<\/span>ee<\/li>\n  <li><span class='match'>thr<\/span>ough<\/li>\n  <li><span class='match'>thr<\/span>ow<\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>typ<\/span>e<\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions to find all words that have three or more vowels in a row.**


```r
# words that have three or more vowels in a row 
str_view(words, pattern = "[aeoiu]{3,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c1ad07fc349132a0ffa3" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c1ad07fc349132a0ffa3">{"x":{"html":"<ul>\n  <li>b<span class='match'>eau<\/span>ty<\/li>\n  <li>obv<span class='match'>iou<\/span>s<\/li>\n  <li>prev<span class='match'>iou<\/span>s<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>ser<span class='match'>iou<\/span>s<\/li>\n  <li>var<span class='match'>iou<\/span>s<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions to find all words that have two or more vowel-consonant pairs in a row.**


```r
# words that have two or more vowel-consonant pairs in a row
str_view(sample(words, 50), pattern = "([aeoui][^aeoiu]){2,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c9713598b69c4cfdcf50" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c9713598b69c4cfdcf50">{"x":{"html":"<ul>\n  <li>n<span class='match'>otic<\/span>e<\/li>\n  <li>tho<span class='match'>usan<\/span>d<\/li>\n  <li>g<span class='match'>over<\/span>n<\/li>\n  <li>e<span class='match'>urop<\/span>e<\/li>\n  <li>pr<span class='match'>otec<\/span>t<\/li>\n  <li>pr<span class='match'>oduc<\/span>e<\/li>\n  <li>d<span class='match'>ecis<\/span>ion<\/li>\n  <li>b<span class='match'>efor<\/span>e<\/li>\n  <li>f<span class='match'>igur<\/span>e<\/li>\n  <li>pr<span class='match'>epar<\/span>e<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Question 4

**Solve the beginner regexp crosswords [here](https://regexcrossword.com/challenges/beginner).**

The following images are the results for the crosswords.


![](figs/fig1.png)    
![](figs/fig2.png)  
![](figs/fig3.png)  
![](figs/fig4.png)  
![](figs/fig5.png)  

## Exercises 14.3.5.1

### Question 1

**Describe, in words, what these expressions will match:**

- `(.)\1\1` regex, matches single character repeated in three succesion such as  "g**ooo**gle".   
-  `"(.)(.)\\2\\1"` string expression, matches a two character direct palindromic repeat character such as "**noon**".   
-  `(..)\1` regex, matches bi-repeated (two characters repeats) such as "**coco**".   
-  `"(.).\\1.\\1"` string expression, matches five characters string with repeated group in 1st, 3rd and 5th position with any random character in 2nd and 4th place such as, "c**a**n**a**d**a**".   
- `"(.)(.)(.).*\\3\\2\\1"` string expression, matches a three character palidrome with the middle having any ammount of character (0 or more). Making it 3 three chracter direct and non direct palidromic string such as "reviver".   

### Question 2

**Construct regular expressions to match words that start and end with the same character.**


```r
# match words that start and end with the same character
str_view(words, pattern = "^(.).*\\1$", match = TRUE) 
```

<!--html_preserve--><div id="htmlwidget-11ec301940980671fc7c" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-11ec301940980671fc7c">{"x":{"html":"<ul>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>area<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>dead<\/span><\/li>\n  <li><span class='match'>depend<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>else<\/span><\/li>\n  <li><span class='match'>encourage<\/span><\/li>\n  <li><span class='match'>engine<\/span><\/li>\n  <li><span class='match'>europe<\/span><\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>excuse<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>health<\/span><\/li>\n  <li><span class='match'>high<\/span><\/li>\n  <li><span class='match'>knock<\/span><\/li>\n  <li><span class='match'>level<\/span><\/li>\n  <li><span class='match'>local<\/span><\/li>\n  <li><span class='match'>nation<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>rather<\/span><\/li>\n  <li><span class='match'>refer<\/span><\/li>\n  <li><span class='match'>remember<\/span><\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>stairs<\/span><\/li>\n  <li><span class='match'>test<\/span><\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>transport<\/span><\/li>\n  <li><span class='match'>treat<\/span><\/li>\n  <li><span class='match'>trust<\/span><\/li>\n  <li><span class='match'>window<\/span><\/li>\n  <li><span class='match'>yesterday<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Construct regular expressions to match words that contain a repeated pair of letters (e.g. "church" contains "ch" repeated twice.)**


```r
# find words that contain a repeated pair of letters
str_view(words, pattern = "(..).*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-9102565c9941fcee3431" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-9102565c9941fcee3431">{"x":{"html":"<ul>\n  <li>ap<span class='match'>propr<\/span>iate<\/li>\n  <li><span class='match'>church<\/span><\/li>\n  <li>c<span class='match'>ondition<\/span><\/li>\n  <li><span class='match'>decide<\/span><\/li>\n  <li><span class='match'>environmen<\/span>t<\/li>\n  <li>l<span class='match'>ondon<\/span><\/li>\n  <li>pa<span class='match'>ragra<\/span>ph<\/li>\n  <li>p<span class='match'>articular<\/span><\/li>\n  <li><span class='match'>photograph<\/span><\/li>\n  <li>p<span class='match'>repare<\/span><\/li>\n  <li>p<span class='match'>ressure<\/span><\/li>\n  <li>r<span class='match'>emem<\/span>ber<\/li>\n  <li><span class='match'>repre<\/span>sent<\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>sense<\/span><\/li>\n  <li>the<span class='match'>refore<\/span><\/li>\n  <li>u<span class='match'>nderstand<\/span><\/li>\n  <li>w<span class='match'>hethe<\/span>r<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Construct regular expressions to match words that contain one letter repeated in at least three places (e.g. "eleven" contains three "e"s.)**


```r
# match words that contain one letter repeated in at least three places
str_view(words, pattern = "(.).*\\1.*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-a8e4df5e9b40ef1f47cd" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-a8e4df5e9b40ef1f47cd">{"x":{"html":"<ul>\n  <li>a<span class='match'>pprop<\/span>riate<\/li>\n  <li><span class='match'>availa<\/span>ble<\/li>\n  <li>b<span class='match'>elieve<\/span><\/li>\n  <li>b<span class='match'>etwee<\/span>n<\/li>\n  <li>bu<span class='match'>siness<\/span><\/li>\n  <li>d<span class='match'>egree<\/span><\/li>\n  <li>diff<span class='match'>erence<\/span><\/li>\n  <li>di<span class='match'>scuss<\/span><\/li>\n  <li><span class='match'>eleve<\/span>n<\/li>\n  <li>e<span class='match'>nvironmen<\/span>t<\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>indivi<\/span>dual<\/li>\n  <li>p<span class='match'>aragra<\/span>ph<\/li>\n  <li>r<span class='match'>eceive<\/span><\/li>\n  <li>r<span class='match'>emembe<\/span>r<\/li>\n  <li>r<span class='match'>eprese<\/span>nt<\/li>\n  <li>t<span class='match'>elephone<\/span><\/li>\n  <li>th<span class='match'>erefore<\/span><\/li>\n  <li>t<span class='match'>omorro<\/span>w<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Exercises 14.4.2

### Question 1

**For each of the following challenges, try solving it by using both a single regular expression, and a combination of multiple `str_detect()` calls.**

**Find all words that start or end with x.**


```r
# single regex find words that start or end with x
str_subset(words, pattern = "^x|x$")
```

```
## [1] "box" "sex" "six" "tax"
```

```r
#  multiple str_detect() calls

# positions start with x
startxde <- str_detect(words, pattern = "^x") # logical vector of words starting with x
# find positions end with x
endxde <- str_detect(words, pattern = "x$") # logical vector of words ending with x

# subset words using pipe
words %>% 
  `[`(startxde | endxde)
```

```
## [1] "box" "sex" "six" "tax"
```

```r
# altenatively 

words[endxde | startxde] # subsets the word vector with a logical vector for x at the start or end of a word.
```

```
## [1] "box" "sex" "six" "tax"
```

```r
all(str_subset(words, pattern = "^x|x$") == words[endxde | startxde]) # check if they are the same
```

```
## [1] TRUE
```


**Find all words that start with a vowel and end with a consonant.**


```r
# single regex
str_subset(words, pattern = "^[aeiou].*[^aeiou]$")
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
# multiple str_detect() calls

startvowel <- str_detect(words, pattern = "^[aeiou]") # logical vector of start with consonants

endconsde <- str_detect(words, pattern = "[^aeiou]$")  # logical vector of end with consonants

# find subset that start or end with x
words %>% 
  `[`(startvowel & endconsde)
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
**Are there any words that contain at least one of each different vowel?**

using `str_detect()` multiple approach there seems no word with all the vowels.


```r
# multiple str_detect() calls

avowel <- str_detect(words, pattern = "[a]") # logical vector of start with vowel a
evowel <- str_detect(words, pattern = "[e]") # logical vector of start with vowel e
ivowel <- str_detect(words, pattern = "[i]") # logical vector of start with vowel i
ovowel <- str_detect(words, pattern = "[o]") # logical vector of start with vowel o
uvowel <- str_detect(words, pattern = "[u]") # logical vector of start with vowel u

words %>% 
  `[`(avowel & evowel & ivowel & ovowel & uvowel)
```

```
## character(0)
```

```r
# fucntion to test vowel

# vowels_test <- function(v) {
#   vowel <- c("a", "e", "i", "o", "u") #  vowels in vector vowel
#   for (vowel in vowels) { # loop over each vowels
#     check <- as.logical(seq_along(v)) & str_detect(v, pattern = vowel)
#   }
#   # return whether s contain at least one of each different vowel
#   return(v[as.logical(seq_along(v))])
# }
# 
# 
# 
# 
# # create a test case
# test_that("function contain_each_vowel() not works", {
#   test_string <- "aberixowu"
#   
#   expect_equal(test_string, vowels_test(test_string))
# })
```

### Question 2

**What word has the highest number of vowels?**


```r
# find the words with the highest number of vowels
words %>% 
  `[`(which(str_count(words, pattern = "[aeiou]")  == # count the number of vowels
              max(str_count(words, pattern = "[aeiou]"))))  # eval expression max and subset using which
```

```
## [1] "appropriate" "associate"   "available"   "colleague"   "encourage"  
## [6] "experience"  "individual"  "television"
```


**What word has the highest proportion of vowels? (Hint: what is the denominator?)**


```r
# find the words with the highest number of vowels
stringr::words %>% 
  `[`(which(str_count(words, pattern = "[aeiou]") # proportion of vowels in words
            / str_length(words) == max(str_count(words, pattern = "[aeiou]") / str_length(words))))
```

```
## [1] "a"
```

`a` should have the highest proportion considering it is purely a vowel word.

## Exercises 14.4.3.1

### Question 1

**In the previous example, you might have noticed that the regular expression matched "flickered", which is not a colour. Modify the regex to fix the problem.**



```r
colours <- c("red", "orange", "yellow", "green", "blue", "purple") # character vector of colors


(colour_match <- str_c(colours, collapse = "|")) # colapse into single vector, so regex "|" or operator
```

```
## [1] "red|orange|yellow|green|blue|purple"
```

```r
more <- sentences[str_count(sentences, colour_match) > 1]
str_view_all(more, colour_match)
```

<!--html_preserve--><div id="htmlwidget-5a4533dc7416ca20552f" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5a4533dc7416ca20552f">{"x":{"html":"<ul>\n  <li>It is hard to erase <span class='match'>blue<\/span> or <span class='match'>red<\/span> ink.<\/li>\n  <li>The <span class='match'>green<\/span> light in the brown box flicke<span class='match'>red<\/span>.<\/li>\n  <li>The sky in the west is tinged with <span class='match'>orange<\/span> <span class='match'>red<\/span>.<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
(flickred <- str_c("\\b", colour_match, "\\b")) # boundry word matches \b to regex
```

```
## [1] "\\bred|orange|yellow|green|blue|purple\\b"
```

```r
# view sentences with more than 1 match
more <- sentences[str_count(sentences, flickred) > 1]
str_view_all(more, flickred)
```

<!--html_preserve--><div id="htmlwidget-430166b4ba2e54be18a5" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-430166b4ba2e54be18a5">{"x":{"html":"<ul>\n  <li>It is hard to erase <span class='match'>blue<\/span> or <span class='match'>red<\/span> ink.<\/li>\n  <li>The sky in the west is tinged with <span class='match'>orange<\/span> <span class='match'>red<\/span>.<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

se "\\b" in regex. "\\b" works at looking for whole words on searches. 


### Question 2

**From the Harvard sentences data, extract the following words.**

**The first word from each sentence.**

`str_extract()` extracts the first match in a string.


```r
sample <-sample(sentences, 20) # Set a random 20 sample from the sentences 

first_words <- str_extract(sample, "[a-zA-Z]+") # str_extract for first word

tibble(sample, first_words) %>% 
  knitr::kable(col.names = c("Sentences", "1st-word"))
```



Sentences                                            1st-word   
---------------------------------------------------  -----------
Footprints showed the path he took up the beach.     Footprints 
Fill the ink jar with sticky glue.                   Fill       
Mud was spattered on the front of his white shirt.   Mud        
Beat the dust from the rug onto the lawn.            Beat       
The door was barred, locked, and bolted as well.     The        
Xew pants lack cuffs and pockets.                    Xew        
The fly made its way along the wall.                 The        
The young prince became heir to the throne.          The        
Hold the hammer near the end to drive the nail.      Hold       
Schools for ladies teach charm and grace.            Schools    
The friendly gang left the drug store.               The        
To send it. now in large amounts is bad.             To         
Two plus seven is less than ten.                     Two        
Those words were the cue for the actor to leave.     Those      
The doctor cured him with these pills.               The        
There is a lag between thought and act.              There      
The soft cushion broke the man's fall.               The        
The peace league met to discuss their plans.         The        
It was a bad error on the part of the new judge.     It         
He used the lathe to make brass objects.             He         

**All words ending in `ing`.**

`str_extract_all()` tries to extract all matches from a string, and put them into a vector.


```r
end_ing <- str_extract_all(sentences, "\\b[a-zA-Z]+ing\\b", simplify = TRUE) # str_extract_all to get words in 'ing'


end_ing2 <- end_ing[end_ing != ""] # every sentence only contains one words ending in "ing", remove all empty element

head(tibble(sentences[end_ing != ""], end_ing2)) 
```

```
## # A tibble: 6 x 2
##   `sentences[end_ing != ""]`                        end_ing2
##   <chr>                                             <chr>   
## 1 The source of the huge river is the clear spring. spring  
## 2 A pot of tea helps to pass the evening.           evening 
## 3 It snowed, rained, and hailed the same morning.   morning 
## 4 Take the winding path to reach the lake.          winding 
## 5 What joy there is in living.                      living  
## 6 A king ruled the state in the early days.         king
```

**All plurals.**

Use multiple `str_detect` as it is too complicated to get all plurals in regex.



```r
regex <- "\\b[a-zA-Z]{3,}(ses|sses|shes|ches|xes|zes|sses|zzes|ves|ies|oes|s)\\b" # regex pattern

sing <- sentences[str_detect(sentences, regex)]

pluralSen <- str_extract_all(sing, pattern = regex, simplify = TRUE)

data.frame(sing, pluralSen)[25:35,] 
```

```
##                                                sing      X1     X2     X3
## 25           Place a rosebush near the porch steps.   steps              
## 26       Both lost their lives in the raging storm.   lives              
## 27       We talked of the slide show in the circus.  circus              
## 28              A small creek cut across the field.  across              
## 29          Cars and busses stalled in snow drifts.    Cars busses drifts
## 30    This is a grand season for hikes on the road.    This  hikes       
## 31 Those words were the cue for the actor to leave.   words              
## 32              The lease ran out in sixteen weeks.   weeks              
## 33                A tame squirrel makes a nice pet.   makes              
## 34   The heart beat strongly and with firm strokes. strokes              
## 35          The fruit peel was cut in thick slices.  slices
```


## Exercises 14.4.4.1

### Question 1

**Find all words that come after a "number" like "one", "two", "three" etc. Pull out both the number and the word.**




```r
numbers = c("zero","one","two","three","four","five","six","seven","eight","nine",
         "ten","eleven","twelve","thirteen","fourteen","fifteen") # assign chracter to numbers

pattern <- str_c(numbers, collapse = "|")
pattern <- str_c("(", pattern, ") ([^ \\.]+)")

filter_sentences <- sentences[str_detect(sentences, pattern)]

number_word <- str_extract_all(filter_sentences, pattern = pattern, simplify = TRUE)

data.frame(filter_sentences, number_word)
```

```
##                                filter_sentences            X1         X2
## 1          Rice is often served in round bowls.    ten served           
## 2         Lift the square stone over the fence.      one over           
## 3   The rope will bind the seven books at once.   seven books           
## 4        The two met while playing on the sand.       two met           
## 5         There are more than two factors here.   two factors           
## 6         He lay prone and hardly moved a limb.       one and           
## 7               Type out three lists of orders.   three lists           
## 8              Two plus seven is less than ten.      seven is           
## 9        Drop the two when you add the figures.      two when           
## 10        Torn scraps littered the stone floor.     one floor           
## 11          There the flood mark is ten inches.    ten inches           
## 12    The lamp shone with a steady green flame.      one with           
## 13          We are sure that one war is enough.       one war           
## 14 His shirt was clean but one button was gone.    one button           
## 15      The fight will end in just six minutes.   six minutes           
## 16            The purple tie was ten years old.     ten years           
## 17     The copper bowl shone in the sun's rays.        one in           
## 18   The kitten chased the dog down the street.    ten chased           
## 19                   Each penny shone like new.      one like           
## 20            Take two shares as a fair profit.    two shares           
## 21     It's a dense crowd in two distinct ways.  two distinct           
## 22        The cone costs five cents on Mondays.     one costs five cents
## 23                Fasten two pins on each side.       ten two           
## 24           The straw nest housed five robins.   five robins           
## 25              Bottles hold four kinds of rum.    four kinds           
## 26          The wall phone rang loud and often.      one rang           
## 27        They told wild tales to frighten him.       ten him           
## 28    The three story house was built of stone.   three story           
## 29      Oats are a food eaten by horse and man.        ten by           
## 30     The dusty bench stood by the stone wall.      one wall           
## 31     It is a band of steel three inches wide.  three inches           
## 32    A stiff cord will do to fasten your shoe.      ten your           
## 33        A six comes up more often than a ten.     six comes   ten than
## 34     It was done before the boy could see it.    one before           
## 35     The mail comes in three batches per day. three batches           
## 36       Slide the bill between the two leaves.    two leaves
```

### Question 2

**Find all contractions. Separate out the pieces before and after the apostrophe.**


```r
pattern <- "([A-Za-z]+)'([A-Za-z]+)" # regex pattern

filter_sentences <- sentences[str_detect(sentences, pattern)] # use str_detect to filter first

contractions <- str_extract_all(filter_sentences, pattern, simplify = T) #str_extract_all to get matches


pieces <- str_split(contractions, pattern = "'", simplify = T) 

# show results as a table
data.frame(filter_sentences, pieces)
```

```
##                                filter_sentences       X1 X2
## 1        It's easy to tell the depth of a well.       It  s
## 2        The soft cushion broke the man's fall.      man  s
## 3     Open the crate but don't break the glass.      don  t
## 4     Add the store's account to the last cent.    store  s
## 5  The beam dropped down on the workmen's head.  workmen  s
## 6    Let's all join as we sing the last chorus.      Let  s
## 7      The copper bowl shone in the sun's rays.      sun  s
## 8           A child's wit saved the day for us.    child  s
## 9       A ripe plum is fit for a king's palate.     king  s
## 10     It's a dense crowd in two distinct ways.       It  s
## 11     We don't get much money but we have fun.      don  t
## 12      Ripe pears are fit for a queen's table.    queen  s
## 13     We don't like to admit our small faults.      don  t
## 14     Dig deep in the earth for pirate's gold.   pirate  s
## 15       She saw a cat in the neighbor's house. neighbor  s
```

## Exercises 14.4.5.1

### Question 1

**Replace all forward slashes in a string with backslashes.**


```r
replace_str <- c("https/www", "github/participation") # assign string to replace_str


str_replace(replace_str, "/", "\\\\") # forward slashed with backslashes
```

```
## [1] "https\\www"            "github\\participation"
```


### Question 2

**Implement a simple version of `str_to_lower()` using `replace_all()`.**


```r
# create some strings to test
letterscaps <- c(LETTERS)

# let's see the results of str_to_lower()
str_to_lower(letterscaps)
```

```
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"
```

```r
# use replace_all() 
str_replace_all(letterscaps, c('A'='a', 'B'='b', 'C'='c', 'D'='d', 'E'='e', 'F'='f', 'G'='g', 'H'='h', 'I'='i', 'J'='j', 'K'='k', 'L'='l', 'M'='m', 'N'='n', 'O'='o', 'P'='p', 'Q'='q', 'R'='r', 'S'='s', 'T'='t', 'U'='u', 'V'='v', 'W'='w', 'X'='x', 'Y'='y', 'Z'='z'))
```

```
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"
```

### Question 3

**Switch the first and last letters in words.**

We use `str_replace()` to switch the letters. We split a word into three groups, the first letter, the middle part, and the third letts, then switch the other of the groups.


```r
switchQ3 <- str_replace(words, pattern = "(^[a-zA-Z])([a-zA-Z]*)([a-zA-Z]$)", replacement = "\\3\\2\\1") # first and last letter switch

head(switchQ3)
```

```
## [1] "a"        "ebla"     "tboua"    "ebsoluta" "tccepa"   "tccouna"
```

**Which of those strings are still words?**


```r
# intersect(), to show words still intact
head(intersect(words, switchQ3), 10) 
```

```
##  [1] "a"       "america" "area"    "dad"     "dead"    "deal"    "dear"   
##  [8] "depend"  "dog"     "educate"
```

## Exercises 14.4.6.1

### Question 1

**Split up a string like "apples, pears, and bananas" into individual components.**

`boundary(word)` to control `str_split()`.


```r
string <- c("apples, pears, and bananas")

# use str_split() with boundary(word)
str_split(string, boundary("word")) %>% 
  `[[`(1) 
```

```
## [1] "apples"  "pears"   "and"     "bananas"
```

### Question 2

**Why is it better to split up by `boundary("word")` than " "?**



```r
# use str_split() with " "
str_split(string, " ") %>% 
  `[[`(1) 
```

```
## [1] "apples," "pears,"  "and"     "bananas"
```

 `boundary("word")` only allows words that are selected out, instead of having punctuations and extra whitespaces by employing the " ".

### Question 3

**What does splitting with an empty string ("") do? Experiment, and then read the documentation.**


```r
# use str_split() with " "
str_split(string, "") %>% 
  `[[`(1) 
```

```
##  [1] "a" "p" "p" "l" "e" "s" "," " " "p" "e" "a" "r" "s" "," " " "a" "n"
## [18] "d" " " "b" "a" "n" "a" "n" "a" "s"
```

empty pattern, '', is equivalent to `boundary('character')`'"), empty string tries to split a strings into single characters.

## Exercises 14.5.1

### Question 1

**How would you find all strings containing `\` with `regex()` vs. with `fixed()`?**

we need to add escape fdor `R` and regex, if fixed is used only the escape character for regex is needed



```r
# create string to test
string2 <- c("https\\www", "github\\participation")

# using regex to find \
str_view(string2, pattern = "\\\\")
```

<!--html_preserve--><div id="htmlwidget-72309ee6d6f46109477b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-72309ee6d6f46109477b">{"x":{"html":"<ul>\n  <li>https<span class='match'>\\<\/span>www<\/li>\n  <li>github<span class='match'>\\<\/span>participation<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# using fixed() to find \
str_view(string2, pattern = fixed("\\"))
```

<!--html_preserve--><div id="htmlwidget-a95af755d676dc30e541" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-a95af755d676dc30e541">{"x":{"html":"<ul>\n  <li>https<span class='match'>\\<\/span>www<\/li>\n  <li>github<span class='match'>\\<\/span>participation<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


### Question 2

**What are the five most common words in `sentences`?**

We first try to get all words in `sentences` using `str_extract_all()` with `boundary("word")`. Then we can convert the results into a tibble and use techniques discribes in STAT 545A to tidy up the data and find the most common words.


```r
word <- sentences %>%   # extract words from sentences
  str_extract_all(boundary("word"), simplify = TRUE) %>%    # change to lower case for further analysis
  str_to_lower()

# convert results to tibble
tibble(words = word) %>%   # n by words
  count(words, sort = TRUE) %>%  
  slice(2:6) 
```

```
## # A tibble: 5 x 2
##   words     n
##   <chr> <int>
## 1 the     751
## 2 a       202
## 3 of      132
## 4 to      123
## 5 and     118
```


## Exercises 14.7.1

### Question 1

**Find the stringi functions that fullfill the following requirements.**

**Requirement 1: count the number of words.**

We can use `stri_count_words()` for this requirement.


```r
# create string to test
test_string <- "This sentence has five words."

# create a test case to test stri_count_words()
test_that("The number of words is not correct", {
  expect_equal(stri_count_words(test_string), 5)
})
```


**Requirement 2: find duplicated strings.**

We can use `stri_duplicated()` or `stri_duplicated_any()` for this requirement:

- `stri_duplicated()` determines which strings in a character vector are duplicates of other elements.
- `stri_duplicated_any()` determines if there are any duplicated strings in a character vector.

We write a function using these two functions, so it can output duplicated strings if they exist.


```r
# write a function to output duplicated strings if they exist
output_duplicated <- function(v) {
  if (any(stri_duplicated(v))) {
    return(v[stri_duplicated_any(v)])
  } else {
    return(NA)
  }
}

# create two vectors to test
vector_duplicated <- c("unique", "duplicated", "duplicated")
vector_unique <- c("all", "are", "unique")

# crate a test suite to test our function
test_that("output_duplicated() is not correct", {
  expect_equal(output_duplicated(vector_duplicated), "duplicated")
  expect_equal(output_duplicated(vector_unique), NA)
})
```

**Requirement 3: generate random text.**

We can use the following two functions for this requirement:

- `stri_rand_strings()`: generate a (pseudo) random strings of desired lengths. The usage of it is as followed, which means that we can use `n` to control how many texts we want to generate, use `length` to control length, and use `pattern` to even decide what we want to generate.

`stri_rand_strings(n, length, pattern = "[A-Za-z0-9]")`

- `stri_rand_lipsum()`: generate a (pseudo) random *lorem ipsum* text consisting of a given number of text paragraphs.


```r
# use stri_rand_strings() to generate random text (for example, password)
stri_rand_strings(n = 5, length = 16, pattern = "[A-Za-z0-9!@$%\\^\\&*]") %>% 
  knitr::kable(col.names = c("Random Text (Password)"))

# use stri_rand_ipsum() to generate random text
stri_rand_lipsum(1)
```

### Question 2

**How do you control the language that stri_sort() uses for sorting?**

According to the document shown using `?stri_sort`, we can use argument `opts_collator` to control the language uses for sorting. For example, we can try to sort french words.


```r
# create vector to test
french_words <- c("Bonjour", "Amour", "Bonheur", "Chat", "Chien")

# use stri_sort() to sort french words, especially for Canadian French
stri_sort(french_words, french = TRUE)
```

## 2. Writing functions

I went with writting a utility function which is a general all package function called `install_load_packages`.

The reason for writting this fucntion is, I always seem to spend some time loading packages when peer reviewing (yes I do load each of them and re-run them and evaluate them individually). As people have different approcahes in the way they approach a problems we are each used to a certain way we do certain things. I have tried simple appraches of loading multiple library, such as `library (tidyverse, gapminder)` which doesnt seem to work, it normally just loads one packages not the second ones. It would be a nice to have a standard were pacakages used are specified at the top of the `md` so one could just install them as needed like [here](https://github.com/STAT545-UBC-students/hw05-zeeva85/blob/master/hw05_gapminder.md). 

As such, I have chosen to make this function for this assignment. I have attempted pass along the `seesioninfo()` as well after using this function as a means to be aware of what was loaded and what is not attached, which is useful to any user.

This function prompts the required package in the console. Simply just type the names WITHOUT the `""` and enter. Following that the next package prompt would be required and the same step is required. When all the required packages have been key'ed in the console. One merely enters an empty entry once for it to close the prompt, look for the package availability and load it if its available. If it is not available it installs it and then loads them and finally outputting the session info in the console with the loaded packages including base packages and the loaded packages along with other system information useful for trouble shooting and reproducibility.

It currently works only for CRAN, based packages. 

**Ussage** 

In cosole or in code chunk:-


```r
install_load_packages() # enter package name for each entry. Enter empty entry once when done.
```


The fucntion itself:-


```r
install_load_packages <- function() {
  
x <- scan(what = "character")  #  The prompt that request the package names

# retrieving the names and length of all the installed packages. Then the input character length vector packages "x" is compared to the installed packages via setdiff(), where rows that appear on x but not y (rownames of packages). 


if (length(setdiff(x, rownames(installed.packages()))) > 0) { # This differences differs if the package in not installed locally and would be evaluated "> 0" which satisfy the if loops.
  install.packages(setdiff(x, rownames(installed.packages())), dependencies = TRUE)  
} # Prompts the install.package now based on the character vectors not the length anymore and retrives dependencies.

sapply(x, library, character.only = TRUE) # loads installed packages using library from the provided list
sessionInfo(package = x) # loads session info of all running packages including system info 

}
```

