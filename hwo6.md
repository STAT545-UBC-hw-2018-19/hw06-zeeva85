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
   
   







```r
install_load_packages <- function() {
  
x <- scan(what = "character")  

if (length(setdiff(x, rownames(installed.packages()))) > 0) {
  install.packages(setdiff(x, rownames(installed.packages())), dependencies = TRUE)  
}

sapply(x, library, character.only = TRUE)
sessionInfo(package = x)

}
```


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

 `\` - is a special character in R, when used singly leads to an incomplete expression error, `Error: Incomplete expression: "\"` in `R` when is evaluated in code chunk or results in the continuation `+` character when its run on the console or 
 

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

<!--html_preserve--><div id="htmlwidget-b087797e1dd7d21b892c" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-b087797e1dd7d21b892c">{"x":{"html":"<ul>\n  <li><span class='match'>\"'\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
str_view(Q14.3.1.1.2, pattern = "\"\'\\\\") # Match Q14.3.1.1.2
```

<!--html_preserve--><div id="htmlwidget-3e96c0f7f71b4537ace2" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-3e96c0f7f71b4537ace2">{"x":{"html":"<ul>\n  <li>hello<span class='match'>\"'\\<\/span>world<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


### Question 3

**What patterns will the regular expression \..\..\.. match? How would you represent it as a string?**

The `\..\..\..` will match a 6 characters string, a fullstop followed by any character in three sucessions in a row such as `.a.n.y`. 


```r
Q14.3.1.1.3 <- ".a.n.y"


str_view(Q14.3.1.1.3, pattern = "\\..\\..\\..")
```

<!--html_preserve--><div id="htmlwidget-f5e5889dbb732b305e7b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-f5e5889dbb732b305e7b">{"x":{"html":"<ul>\n  <li><span class='match'>.a.n.y<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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

<!--html_preserve--><div id="htmlwidget-19be86208466fdaca72d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-19be86208466fdaca72d">{"x":{"html":"<ul>\n  <li><span class='match'>$^$<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Question 2

**Given the corpus of common words in `stringr::words`, create regular expressions that find all words that fulfilling the following requirements.**

**Words starting with "y".**


```r
# find words starting with "y" using the "^y"
str_view(words, pattern = "^y", match = TRUE) # Use match = TRUE to only show words that match
```

<!--html_preserve--><div id="htmlwidget-6be08bf183b210cd029c" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-6be08bf183b210cd029c">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**Words ending with "x".**


```r
# find words end with "x" using the "x$"
str_view(words, pattern = "x$", match = TRUE) # Use match = TRUE to only show words that match
```

<!--html_preserve--><div id="htmlwidget-21609d0140d49656188d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-21609d0140d49656188d">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Are exactly three letters long. (Don't cheat by using `str_length()`!).**


```r
str_view(words, pattern = "\\b...\\b", match = TRUE) # use the boundry before and after three any characters
```

<!--html_preserve--><div id="htmlwidget-2cc94d496dcb0e189426" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-2cc94d496dcb0e189426">{"x":{"html":"<ul>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>age<\/span><\/li>\n  <li><span class='match'>ago<\/span><\/li>\n  <li><span class='match'>air<\/span><\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li><span class='match'>any<\/span><\/li>\n  <li><span class='match'>arm<\/span><\/li>\n  <li><span class='match'>art<\/span><\/li>\n  <li><span class='match'>ask<\/span><\/li>\n  <li><span class='match'>bad<\/span><\/li>\n  <li><span class='match'>bag<\/span><\/li>\n  <li><span class='match'>bar<\/span><\/li>\n  <li><span class='match'>bed<\/span><\/li>\n  <li><span class='match'>bet<\/span><\/li>\n  <li><span class='match'>big<\/span><\/li>\n  <li><span class='match'>bit<\/span><\/li>\n  <li><span class='match'>box<\/span><\/li>\n  <li><span class='match'>boy<\/span><\/li>\n  <li><span class='match'>bus<\/span><\/li>\n  <li><span class='match'>but<\/span><\/li>\n  <li><span class='match'>buy<\/span><\/li>\n  <li><span class='match'>can<\/span><\/li>\n  <li><span class='match'>car<\/span><\/li>\n  <li><span class='match'>cat<\/span><\/li>\n  <li><span class='match'>cup<\/span><\/li>\n  <li><span class='match'>cut<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>day<\/span><\/li>\n  <li><span class='match'>die<\/span><\/li>\n  <li><span class='match'>dog<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>due<\/span><\/li>\n  <li><span class='match'>eat<\/span><\/li>\n  <li><span class='match'>egg<\/span><\/li>\n  <li><span class='match'>end<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>far<\/span><\/li>\n  <li><span class='match'>few<\/span><\/li>\n  <li><span class='match'>fit<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>for<\/span><\/li>\n  <li><span class='match'>fun<\/span><\/li>\n  <li><span class='match'>gas<\/span><\/li>\n  <li><span class='match'>get<\/span><\/li>\n  <li><span class='match'>god<\/span><\/li>\n  <li><span class='match'>guy<\/span><\/li>\n  <li><span class='match'>hit<\/span><\/li>\n  <li><span class='match'>hot<\/span><\/li>\n  <li><span class='match'>how<\/span><\/li>\n  <li><span class='match'>job<\/span><\/li>\n  <li><span class='match'>key<\/span><\/li>\n  <li><span class='match'>kid<\/span><\/li>\n  <li><span class='match'>lad<\/span><\/li>\n  <li><span class='match'>law<\/span><\/li>\n  <li><span class='match'>lay<\/span><\/li>\n  <li><span class='match'>leg<\/span><\/li>\n  <li><span class='match'>let<\/span><\/li>\n  <li><span class='match'>lie<\/span><\/li>\n  <li><span class='match'>lot<\/span><\/li>\n  <li><span class='match'>low<\/span><\/li>\n  <li><span class='match'>man<\/span><\/li>\n  <li><span class='match'>may<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>new<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>not<\/span><\/li>\n  <li><span class='match'>now<\/span><\/li>\n  <li><span class='match'>odd<\/span><\/li>\n  <li><span class='match'>off<\/span><\/li>\n  <li><span class='match'>old<\/span><\/li>\n  <li><span class='match'>one<\/span><\/li>\n  <li><span class='match'>out<\/span><\/li>\n  <li><span class='match'>own<\/span><\/li>\n  <li><span class='match'>pay<\/span><\/li>\n  <li><span class='match'>per<\/span><\/li>\n  <li><span class='match'>put<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n  <li><span class='match'>rid<\/span><\/li>\n  <li><span class='match'>run<\/span><\/li>\n  <li><span class='match'>say<\/span><\/li>\n  <li><span class='match'>see<\/span><\/li>\n  <li><span class='match'>set<\/span><\/li>\n  <li><span class='match'>sex<\/span><\/li>\n  <li><span class='match'>she<\/span><\/li>\n  <li><span class='match'>sir<\/span><\/li>\n  <li><span class='match'>sit<\/span><\/li>\n  <li><span class='match'>six<\/span><\/li>\n  <li><span class='match'>son<\/span><\/li>\n  <li><span class='match'>sun<\/span><\/li>\n  <li><span class='match'>tax<\/span><\/li>\n  <li><span class='match'>tea<\/span><\/li>\n  <li><span class='match'>ten<\/span><\/li>\n  <li><span class='match'>the<\/span><\/li>\n  <li><span class='match'>tie<\/span><\/li>\n  <li><span class='match'>too<\/span><\/li>\n  <li><span class='match'>top<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>two<\/span><\/li>\n  <li><span class='match'>use<\/span><\/li>\n  <li><span class='match'>war<\/span><\/li>\n  <li><span class='match'>way<\/span><\/li>\n  <li><span class='match'>wee<\/span><\/li>\n  <li><span class='match'>who<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n  <li><span class='match'>win<\/span><\/li>\n  <li><span class='match'>yes<\/span><\/li>\n  <li><span class='match'>yet<\/span><\/li>\n  <li><span class='match'>you<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Requirement 4: have seven letters or more.**


```r
str_view(words, pattern = "\\b.......", match = TRUE) # use one boundry to define seven letters and more
```

<!--html_preserve--><div id="htmlwidget-8ec98854ed3fa117ddc3" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-8ec98854ed3fa117ddc3">{"x":{"html":"<ul>\n  <li><span class='match'>absolut<\/span>e<\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>adverti<\/span>se<\/li>\n  <li><span class='match'>afterno<\/span>on<\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>althoug<\/span>h<\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>apparen<\/span>t<\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>approac<\/span>h<\/li>\n  <li><span class='match'>appropr<\/span>iate<\/li>\n  <li><span class='match'>arrange<\/span><\/li>\n  <li><span class='match'>associa<\/span>te<\/li>\n  <li><span class='match'>authori<\/span>ty<\/li>\n  <li><span class='match'>availab<\/span>le<\/li>\n  <li><span class='match'>balance<\/span><\/li>\n  <li><span class='match'>because<\/span><\/li>\n  <li><span class='match'>believe<\/span><\/li>\n  <li><span class='match'>benefit<\/span><\/li>\n  <li><span class='match'>between<\/span><\/li>\n  <li><span class='match'>brillia<\/span>nt<\/li>\n  <li><span class='match'>britain<\/span><\/li>\n  <li><span class='match'>brother<\/span><\/li>\n  <li><span class='match'>busines<\/span>s<\/li>\n  <li><span class='match'>certain<\/span><\/li>\n  <li><span class='match'>chairma<\/span>n<\/li>\n  <li><span class='match'>charact<\/span>er<\/li>\n  <li><span class='match'>Christm<\/span>as<\/li>\n  <li><span class='match'>colleag<\/span>ue<\/li>\n  <li><span class='match'>collect<\/span><\/li>\n  <li><span class='match'>college<\/span><\/li>\n  <li><span class='match'>comment<\/span><\/li>\n  <li><span class='match'>committ<\/span>ee<\/li>\n  <li><span class='match'>communi<\/span>ty<\/li>\n  <li><span class='match'>company<\/span><\/li>\n  <li><span class='match'>compare<\/span><\/li>\n  <li><span class='match'>complet<\/span>e<\/li>\n  <li><span class='match'>compute<\/span><\/li>\n  <li><span class='match'>concern<\/span><\/li>\n  <li><span class='match'>conditi<\/span>on<\/li>\n  <li><span class='match'>conside<\/span>r<\/li>\n  <li><span class='match'>consult<\/span><\/li>\n  <li><span class='match'>contact<\/span><\/li>\n  <li><span class='match'>continu<\/span>e<\/li>\n  <li><span class='match'>contrac<\/span>t<\/li>\n  <li><span class='match'>control<\/span><\/li>\n  <li><span class='match'>convers<\/span>e<\/li>\n  <li><span class='match'>correct<\/span><\/li>\n  <li><span class='match'>council<\/span><\/li>\n  <li><span class='match'>country<\/span><\/li>\n  <li><span class='match'>current<\/span><\/li>\n  <li><span class='match'>decisio<\/span>n<\/li>\n  <li><span class='match'>definit<\/span>e<\/li>\n  <li><span class='match'>departm<\/span>ent<\/li>\n  <li><span class='match'>describ<\/span>e<\/li>\n  <li><span class='match'>develop<\/span><\/li>\n  <li><span class='match'>differe<\/span>nce<\/li>\n  <li><span class='match'>difficu<\/span>lt<\/li>\n  <li><span class='match'>discuss<\/span><\/li>\n  <li><span class='match'>distric<\/span>t<\/li>\n  <li><span class='match'>documen<\/span>t<\/li>\n  <li><span class='match'>economy<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>electri<\/span>c<\/li>\n  <li><span class='match'>encoura<\/span>ge<\/li>\n  <li><span class='match'>english<\/span><\/li>\n  <li><span class='match'>environ<\/span>ment<\/li>\n  <li><span class='match'>especia<\/span>l<\/li>\n  <li><span class='match'>evening<\/span><\/li>\n  <li><span class='match'>evidenc<\/span>e<\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>exercis<\/span>e<\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experie<\/span>nce<\/li>\n  <li><span class='match'>explain<\/span><\/li>\n  <li><span class='match'>express<\/span><\/li>\n  <li><span class='match'>finance<\/span><\/li>\n  <li><span class='match'>fortune<\/span><\/li>\n  <li><span class='match'>forward<\/span><\/li>\n  <li><span class='match'>functio<\/span>n<\/li>\n  <li><span class='match'>further<\/span><\/li>\n  <li><span class='match'>general<\/span><\/li>\n  <li><span class='match'>germany<\/span><\/li>\n  <li><span class='match'>goodbye<\/span><\/li>\n  <li><span class='match'>history<\/span><\/li>\n  <li><span class='match'>holiday<\/span><\/li>\n  <li><span class='match'>hospita<\/span>l<\/li>\n  <li><span class='match'>however<\/span><\/li>\n  <li><span class='match'>hundred<\/span><\/li>\n  <li><span class='match'>husband<\/span><\/li>\n  <li><span class='match'>identif<\/span>y<\/li>\n  <li><span class='match'>imagine<\/span><\/li>\n  <li><span class='match'>importa<\/span>nt<\/li>\n  <li><span class='match'>improve<\/span><\/li>\n  <li><span class='match'>include<\/span><\/li>\n  <li><span class='match'>increas<\/span>e<\/li>\n  <li><span class='match'>individ<\/span>ual<\/li>\n  <li><span class='match'>industr<\/span>y<\/li>\n  <li><span class='match'>instead<\/span><\/li>\n  <li><span class='match'>interes<\/span>t<\/li>\n  <li><span class='match'>introdu<\/span>ce<\/li>\n  <li><span class='match'>involve<\/span><\/li>\n  <li><span class='match'>kitchen<\/span><\/li>\n  <li><span class='match'>languag<\/span>e<\/li>\n  <li><span class='match'>machine<\/span><\/li>\n  <li><span class='match'>meaning<\/span><\/li>\n  <li><span class='match'>measure<\/span><\/li>\n  <li><span class='match'>mention<\/span><\/li>\n  <li><span class='match'>million<\/span><\/li>\n  <li><span class='match'>ministe<\/span>r<\/li>\n  <li><span class='match'>morning<\/span><\/li>\n  <li><span class='match'>necessa<\/span>ry<\/li>\n  <li><span class='match'>obvious<\/span><\/li>\n  <li><span class='match'>occasio<\/span>n<\/li>\n  <li><span class='match'>operate<\/span><\/li>\n  <li><span class='match'>opportu<\/span>nity<\/li>\n  <li><span class='match'>organiz<\/span>e<\/li>\n  <li><span class='match'>origina<\/span>l<\/li>\n  <li><span class='match'>otherwi<\/span>se<\/li>\n  <li><span class='match'>paragra<\/span>ph<\/li>\n  <li><span class='match'>particu<\/span>lar<\/li>\n  <li><span class='match'>pension<\/span><\/li>\n  <li><span class='match'>percent<\/span><\/li>\n  <li><span class='match'>perfect<\/span><\/li>\n  <li><span class='match'>perhaps<\/span><\/li>\n  <li><span class='match'>photogr<\/span>aph<\/li>\n  <li><span class='match'>picture<\/span><\/li>\n  <li><span class='match'>politic<\/span><\/li>\n  <li><span class='match'>positio<\/span>n<\/li>\n  <li><span class='match'>positiv<\/span>e<\/li>\n  <li><span class='match'>possibl<\/span>e<\/li>\n  <li><span class='match'>practis<\/span>e<\/li>\n  <li><span class='match'>prepare<\/span><\/li>\n  <li><span class='match'>present<\/span><\/li>\n  <li><span class='match'>pressur<\/span>e<\/li>\n  <li><span class='match'>presume<\/span><\/li>\n  <li><span class='match'>previou<\/span>s<\/li>\n  <li><span class='match'>private<\/span><\/li>\n  <li><span class='match'>probabl<\/span>e<\/li>\n  <li><span class='match'>problem<\/span><\/li>\n  <li><span class='match'>proceed<\/span><\/li>\n  <li><span class='match'>process<\/span><\/li>\n  <li><span class='match'>produce<\/span><\/li>\n  <li><span class='match'>product<\/span><\/li>\n  <li><span class='match'>program<\/span>me<\/li>\n  <li><span class='match'>project<\/span><\/li>\n  <li><span class='match'>propose<\/span><\/li>\n  <li><span class='match'>protect<\/span><\/li>\n  <li><span class='match'>provide<\/span><\/li>\n  <li><span class='match'>purpose<\/span><\/li>\n  <li><span class='match'>quality<\/span><\/li>\n  <li><span class='match'>quarter<\/span><\/li>\n  <li><span class='match'>questio<\/span>n<\/li>\n  <li><span class='match'>realise<\/span><\/li>\n  <li><span class='match'>receive<\/span><\/li>\n  <li><span class='match'>recogni<\/span>ze<\/li>\n  <li><span class='match'>recomme<\/span>nd<\/li>\n  <li><span class='match'>relatio<\/span>n<\/li>\n  <li><span class='match'>remembe<\/span>r<\/li>\n  <li><span class='match'>represe<\/span>nt<\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>researc<\/span>h<\/li>\n  <li><span class='match'>resourc<\/span>e<\/li>\n  <li><span class='match'>respect<\/span><\/li>\n  <li><span class='match'>respons<\/span>ible<\/li>\n  <li><span class='match'>saturda<\/span>y<\/li>\n  <li><span class='match'>science<\/span><\/li>\n  <li><span class='match'>scotlan<\/span>d<\/li>\n  <li><span class='match'>secreta<\/span>ry<\/li>\n  <li><span class='match'>section<\/span><\/li>\n  <li><span class='match'>separat<\/span>e<\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>service<\/span><\/li>\n  <li><span class='match'>similar<\/span><\/li>\n  <li><span class='match'>situate<\/span><\/li>\n  <li><span class='match'>society<\/span><\/li>\n  <li><span class='match'>special<\/span><\/li>\n  <li><span class='match'>specifi<\/span>c<\/li>\n  <li><span class='match'>standar<\/span>d<\/li>\n  <li><span class='match'>station<\/span><\/li>\n  <li><span class='match'>straigh<\/span>t<\/li>\n  <li><span class='match'>strateg<\/span>y<\/li>\n  <li><span class='match'>structu<\/span>re<\/li>\n  <li><span class='match'>student<\/span><\/li>\n  <li><span class='match'>subject<\/span><\/li>\n  <li><span class='match'>succeed<\/span><\/li>\n  <li><span class='match'>suggest<\/span><\/li>\n  <li><span class='match'>support<\/span><\/li>\n  <li><span class='match'>suppose<\/span><\/li>\n  <li><span class='match'>surpris<\/span>e<\/li>\n  <li><span class='match'>telepho<\/span>ne<\/li>\n  <li><span class='match'>televis<\/span>ion<\/li>\n  <li><span class='match'>terribl<\/span>e<\/li>\n  <li><span class='match'>therefo<\/span>re<\/li>\n  <li><span class='match'>thirtee<\/span>n<\/li>\n  <li><span class='match'>thousan<\/span>d<\/li>\n  <li><span class='match'>through<\/span><\/li>\n  <li><span class='match'>thursda<\/span>y<\/li>\n  <li><span class='match'>togethe<\/span>r<\/li>\n  <li><span class='match'>tomorro<\/span>w<\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>traffic<\/span><\/li>\n  <li><span class='match'>transpo<\/span>rt<\/li>\n  <li><span class='match'>trouble<\/span><\/li>\n  <li><span class='match'>tuesday<\/span><\/li>\n  <li><span class='match'>underst<\/span>and<\/li>\n  <li><span class='match'>univers<\/span>ity<\/li>\n  <li><span class='match'>various<\/span><\/li>\n  <li><span class='match'>village<\/span><\/li>\n  <li><span class='match'>wednesd<\/span>ay<\/li>\n  <li><span class='match'>welcome<\/span><\/li>\n  <li><span class='match'>whether<\/span><\/li>\n  <li><span class='match'>without<\/span><\/li>\n  <li><span class='match'>yesterd<\/span>ay<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


## Exercises 14.3.3.1

### Question 1

**Create regular expressions to find all words that start with a vowel.**


```r
# find words that start with a vowel
str_view(words, pattern = "^[aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-28bef166c9859ce2b75b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-28bef166c9859ce2b75b">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span><\/li>\n  <li><span class='match'>a<\/span>ble<\/li>\n  <li><span class='match'>a<\/span>bout<\/li>\n  <li><span class='match'>a<\/span>bsolute<\/li>\n  <li><span class='match'>a<\/span>ccept<\/li>\n  <li><span class='match'>a<\/span>ccount<\/li>\n  <li><span class='match'>a<\/span>chieve<\/li>\n  <li><span class='match'>a<\/span>cross<\/li>\n  <li><span class='match'>a<\/span>ct<\/li>\n  <li><span class='match'>a<\/span>ctive<\/li>\n  <li><span class='match'>a<\/span>ctual<\/li>\n  <li><span class='match'>a<\/span>dd<\/li>\n  <li><span class='match'>a<\/span>ddress<\/li>\n  <li><span class='match'>a<\/span>dmit<\/li>\n  <li><span class='match'>a<\/span>dvertise<\/li>\n  <li><span class='match'>a<\/span>ffect<\/li>\n  <li><span class='match'>a<\/span>fford<\/li>\n  <li><span class='match'>a<\/span>fter<\/li>\n  <li><span class='match'>a<\/span>fternoon<\/li>\n  <li><span class='match'>a<\/span>gain<\/li>\n  <li><span class='match'>a<\/span>gainst<\/li>\n  <li><span class='match'>a<\/span>ge<\/li>\n  <li><span class='match'>a<\/span>gent<\/li>\n  <li><span class='match'>a<\/span>go<\/li>\n  <li><span class='match'>a<\/span>gree<\/li>\n  <li><span class='match'>a<\/span>ir<\/li>\n  <li><span class='match'>a<\/span>ll<\/li>\n  <li><span class='match'>a<\/span>llow<\/li>\n  <li><span class='match'>a<\/span>lmost<\/li>\n  <li><span class='match'>a<\/span>long<\/li>\n  <li><span class='match'>a<\/span>lready<\/li>\n  <li><span class='match'>a<\/span>lright<\/li>\n  <li><span class='match'>a<\/span>lso<\/li>\n  <li><span class='match'>a<\/span>lthough<\/li>\n  <li><span class='match'>a<\/span>lways<\/li>\n  <li><span class='match'>a<\/span>merica<\/li>\n  <li><span class='match'>a<\/span>mount<\/li>\n  <li><span class='match'>a<\/span>nd<\/li>\n  <li><span class='match'>a<\/span>nother<\/li>\n  <li><span class='match'>a<\/span>nswer<\/li>\n  <li><span class='match'>a<\/span>ny<\/li>\n  <li><span class='match'>a<\/span>part<\/li>\n  <li><span class='match'>a<\/span>pparent<\/li>\n  <li><span class='match'>a<\/span>ppear<\/li>\n  <li><span class='match'>a<\/span>pply<\/li>\n  <li><span class='match'>a<\/span>ppoint<\/li>\n  <li><span class='match'>a<\/span>pproach<\/li>\n  <li><span class='match'>a<\/span>ppropriate<\/li>\n  <li><span class='match'>a<\/span>rea<\/li>\n  <li><span class='match'>a<\/span>rgue<\/li>\n  <li><span class='match'>a<\/span>rm<\/li>\n  <li><span class='match'>a<\/span>round<\/li>\n  <li><span class='match'>a<\/span>rrange<\/li>\n  <li><span class='match'>a<\/span>rt<\/li>\n  <li><span class='match'>a<\/span>s<\/li>\n  <li><span class='match'>a<\/span>sk<\/li>\n  <li><span class='match'>a<\/span>ssociate<\/li>\n  <li><span class='match'>a<\/span>ssume<\/li>\n  <li><span class='match'>a<\/span>t<\/li>\n  <li><span class='match'>a<\/span>ttend<\/li>\n  <li><span class='match'>a<\/span>uthority<\/li>\n  <li><span class='match'>a<\/span>vailable<\/li>\n  <li><span class='match'>a<\/span>ware<\/li>\n  <li><span class='match'>a<\/span>way<\/li>\n  <li><span class='match'>a<\/span>wful<\/li>\n  <li><span class='match'>e<\/span>ach<\/li>\n  <li><span class='match'>e<\/span>arly<\/li>\n  <li><span class='match'>e<\/span>ast<\/li>\n  <li><span class='match'>e<\/span>asy<\/li>\n  <li><span class='match'>e<\/span>at<\/li>\n  <li><span class='match'>e<\/span>conomy<\/li>\n  <li><span class='match'>e<\/span>ducate<\/li>\n  <li><span class='match'>e<\/span>ffect<\/li>\n  <li><span class='match'>e<\/span>gg<\/li>\n  <li><span class='match'>e<\/span>ight<\/li>\n  <li><span class='match'>e<\/span>ither<\/li>\n  <li><span class='match'>e<\/span>lect<\/li>\n  <li><span class='match'>e<\/span>lectric<\/li>\n  <li><span class='match'>e<\/span>leven<\/li>\n  <li><span class='match'>e<\/span>lse<\/li>\n  <li><span class='match'>e<\/span>mploy<\/li>\n  <li><span class='match'>e<\/span>ncourage<\/li>\n  <li><span class='match'>e<\/span>nd<\/li>\n  <li><span class='match'>e<\/span>ngine<\/li>\n  <li><span class='match'>e<\/span>nglish<\/li>\n  <li><span class='match'>e<\/span>njoy<\/li>\n  <li><span class='match'>e<\/span>nough<\/li>\n  <li><span class='match'>e<\/span>nter<\/li>\n  <li><span class='match'>e<\/span>nvironment<\/li>\n  <li><span class='match'>e<\/span>qual<\/li>\n  <li><span class='match'>e<\/span>special<\/li>\n  <li><span class='match'>e<\/span>urope<\/li>\n  <li><span class='match'>e<\/span>ven<\/li>\n  <li><span class='match'>e<\/span>vening<\/li>\n  <li><span class='match'>e<\/span>ver<\/li>\n  <li><span class='match'>e<\/span>very<\/li>\n  <li><span class='match'>e<\/span>vidence<\/li>\n  <li><span class='match'>e<\/span>xact<\/li>\n  <li><span class='match'>e<\/span>xample<\/li>\n  <li><span class='match'>e<\/span>xcept<\/li>\n  <li><span class='match'>e<\/span>xcuse<\/li>\n  <li><span class='match'>e<\/span>xercise<\/li>\n  <li><span class='match'>e<\/span>xist<\/li>\n  <li><span class='match'>e<\/span>xpect<\/li>\n  <li><span class='match'>e<\/span>xpense<\/li>\n  <li><span class='match'>e<\/span>xperience<\/li>\n  <li><span class='match'>e<\/span>xplain<\/li>\n  <li><span class='match'>e<\/span>xpress<\/li>\n  <li><span class='match'>e<\/span>xtra<\/li>\n  <li><span class='match'>e<\/span>ye<\/li>\n  <li><span class='match'>i<\/span>dea<\/li>\n  <li><span class='match'>i<\/span>dentify<\/li>\n  <li><span class='match'>i<\/span>f<\/li>\n  <li><span class='match'>i<\/span>magine<\/li>\n  <li><span class='match'>i<\/span>mportant<\/li>\n  <li><span class='match'>i<\/span>mprove<\/li>\n  <li><span class='match'>i<\/span>n<\/li>\n  <li><span class='match'>i<\/span>nclude<\/li>\n  <li><span class='match'>i<\/span>ncome<\/li>\n  <li><span class='match'>i<\/span>ncrease<\/li>\n  <li><span class='match'>i<\/span>ndeed<\/li>\n  <li><span class='match'>i<\/span>ndividual<\/li>\n  <li><span class='match'>i<\/span>ndustry<\/li>\n  <li><span class='match'>i<\/span>nform<\/li>\n  <li><span class='match'>i<\/span>nside<\/li>\n  <li><span class='match'>i<\/span>nstead<\/li>\n  <li><span class='match'>i<\/span>nsure<\/li>\n  <li><span class='match'>i<\/span>nterest<\/li>\n  <li><span class='match'>i<\/span>nto<\/li>\n  <li><span class='match'>i<\/span>ntroduce<\/li>\n  <li><span class='match'>i<\/span>nvest<\/li>\n  <li><span class='match'>i<\/span>nvolve<\/li>\n  <li><span class='match'>i<\/span>ssue<\/li>\n  <li><span class='match'>i<\/span>t<\/li>\n  <li><span class='match'>i<\/span>tem<\/li>\n  <li><span class='match'>o<\/span>bvious<\/li>\n  <li><span class='match'>o<\/span>ccasion<\/li>\n  <li><span class='match'>o<\/span>dd<\/li>\n  <li><span class='match'>o<\/span>f<\/li>\n  <li><span class='match'>o<\/span>ff<\/li>\n  <li><span class='match'>o<\/span>ffer<\/li>\n  <li><span class='match'>o<\/span>ffice<\/li>\n  <li><span class='match'>o<\/span>ften<\/li>\n  <li><span class='match'>o<\/span>kay<\/li>\n  <li><span class='match'>o<\/span>ld<\/li>\n  <li><span class='match'>o<\/span>n<\/li>\n  <li><span class='match'>o<\/span>nce<\/li>\n  <li><span class='match'>o<\/span>ne<\/li>\n  <li><span class='match'>o<\/span>nly<\/li>\n  <li><span class='match'>o<\/span>pen<\/li>\n  <li><span class='match'>o<\/span>perate<\/li>\n  <li><span class='match'>o<\/span>pportunity<\/li>\n  <li><span class='match'>o<\/span>ppose<\/li>\n  <li><span class='match'>o<\/span>r<\/li>\n  <li><span class='match'>o<\/span>rder<\/li>\n  <li><span class='match'>o<\/span>rganize<\/li>\n  <li><span class='match'>o<\/span>riginal<\/li>\n  <li><span class='match'>o<\/span>ther<\/li>\n  <li><span class='match'>o<\/span>therwise<\/li>\n  <li><span class='match'>o<\/span>ught<\/li>\n  <li><span class='match'>o<\/span>ut<\/li>\n  <li><span class='match'>o<\/span>ver<\/li>\n  <li><span class='match'>o<\/span>wn<\/li>\n  <li><span class='match'>u<\/span>nder<\/li>\n  <li><span class='match'>u<\/span>nderstand<\/li>\n  <li><span class='match'>u<\/span>nion<\/li>\n  <li><span class='match'>u<\/span>nit<\/li>\n  <li><span class='match'>u<\/span>nite<\/li>\n  <li><span class='match'>u<\/span>niversity<\/li>\n  <li><span class='match'>u<\/span>nless<\/li>\n  <li><span class='match'>u<\/span>ntil<\/li>\n  <li><span class='match'>u<\/span>p<\/li>\n  <li><span class='match'>u<\/span>pon<\/li>\n  <li><span class='match'>u<\/span>se<\/li>\n  <li><span class='match'>u<\/span>sual<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions that only contain consonants. (Hint: thinking about matching "not"-vowels.)**


```r
# find words that without a vowel using the not "^" special character
str_view(stringr::words, pattern = "^aeiou", match = T)
```

<!--html_preserve--><div id="htmlwidget-20323385df30c2f13862" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-20323385df30c2f13862">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

There is no words without all the vowels

**Create regular expressions that only end with `ed`, but not with `eed`.**


```r
# find words that end with "ed" but not "eed"
str_view(words, pattern = "[^e]ed$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-60ad8944f8f3d52412a6" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-60ad8944f8f3d52412a6">{"x":{"html":"<ul>\n  <li><span class='match'>bed<\/span><\/li>\n  <li>hund<span class='match'>red<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions that only end with `ing` or `ise`.**


```r
# find words that end with "ing" or "ise"
str_view(words, pattern = "ing$|ise$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-928ae07541af0937a36f" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-928ae07541af0937a36f">{"x":{"html":"<ul>\n  <li>advert<span class='match'>ise<\/span><\/li>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>exerc<span class='match'>ise<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>morn<span class='match'>ing<\/span><\/li>\n  <li>otherw<span class='match'>ise<\/span><\/li>\n  <li>pract<span class='match'>ise<\/span><\/li>\n  <li>ra<span class='match'>ise<\/span><\/li>\n  <li>real<span class='match'>ise<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>surpr<span class='match'>ise<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
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

<!--html_preserve--><div id="htmlwidget-af4eb38c50d35af2bafb" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-af4eb38c50d35af2bafb">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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

<!--html_preserve--><div id="htmlwidget-fc81eddd7141725924be" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-fc81eddd7141725924be">{"x":{"html":"<ul>\n  <li><span class='match'>c(\"cancelled\", \"counsellor\", \"equalled\", \"fuelling\", \"fuelled\", \"grovell<\/span>ing\", \"canceled\", \"counselor\", \"equaled\", \"fueling\", \"fueled\", \"groveling\")<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Question 5

**Create a regular expression that will match telephone numbers as commonly written in your country.**

The telephone numbers in Malaysia typically is like so:- "+60 (12) 110 0101". 


```r
# create some malaysia, canada, and singapore numbers to test
telNum <- c("+60 12 110 0101", "+1 778 8464 273", "+65  1110 0000")

# use a regex to detect telephone numbers in Canada
str_view(telNum, pattern = "\\+60 [1-9][1-9] [:digit:]{3} [:digit:]{4}")
```

<!--html_preserve--><div id="htmlwidget-83ff45677a62a359af7d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-83ff45677a62a359af7d">{"x":{"html":"<ul>\n  <li><span class='match'>+60 12 110 0101<\/span><\/li>\n  <li>+1 778 8464 273<\/li>\n  <li>+65  1110 0000<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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

<!--html_preserve--><div id="htmlwidget-afb392c68cbd0dcd9796" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-afb392c68cbd0dcd9796">{"x":{"html":"<ul>\n  <li><span class='match'>Chr<\/span>ist<\/li>\n  <li><span class='match'>Chr<\/span>istmas<\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>sch<\/span>eme<\/li>\n  <li><span class='match'>sch<\/span>ool<\/li>\n  <li><span class='match'>str<\/span>aight<\/li>\n  <li><span class='match'>str<\/span>ategy<\/li>\n  <li><span class='match'>str<\/span>eet<\/li>\n  <li><span class='match'>str<\/span>ike<\/li>\n  <li><span class='match'>str<\/span>ong<\/li>\n  <li><span class='match'>str<\/span>ucture<\/li>\n  <li><span class='match'>sys<\/span>tem<\/li>\n  <li><span class='match'>thr<\/span>ee<\/li>\n  <li><span class='match'>thr<\/span>ough<\/li>\n  <li><span class='match'>thr<\/span>ow<\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>typ<\/span>e<\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions to find all words that have three or more vowels in a row.**


```r
# words that have three or more vowels in a row 
str_view(stringr::words, pattern = "[aeoiu]{3,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-d16cfba1fea6bdab352e" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-d16cfba1fea6bdab352e">{"x":{"html":"<ul>\n  <li>b<span class='match'>eau<\/span>ty<\/li>\n  <li>obv<span class='match'>iou<\/span>s<\/li>\n  <li>prev<span class='match'>iou<\/span>s<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>ser<span class='match'>iou<\/span>s<\/li>\n  <li>var<span class='match'>iou<\/span>s<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**Create regular expressions to find all words that have two or more vowel-consonant pairs in a row.**


```r
# words that have two or more vowel-consonant pairs in a row
str_view(words, pattern = "([aeoui][^aeoiu]){2,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-0eed5a265ca7a0b62d6f" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0eed5a265ca7a0b62d6f">{"x":{"html":"<ul>\n  <li>abs<span class='match'>olut<\/span>e<\/li>\n  <li><span class='match'>agen<\/span>t<\/li>\n  <li><span class='match'>alon<\/span>g<\/li>\n  <li><span class='match'>americ<\/span>a<\/li>\n  <li><span class='match'>anot<\/span>her<\/li>\n  <li><span class='match'>apar<\/span>t<\/li>\n  <li>app<span class='match'>aren<\/span>t<\/li>\n  <li>auth<span class='match'>orit<\/span>y<\/li>\n  <li>ava<span class='match'>ilab<\/span>le<\/li>\n  <li><span class='match'>awar<\/span>e<\/li>\n  <li><span class='match'>away<\/span><\/li>\n  <li>b<span class='match'>alan<\/span>ce<\/li>\n  <li>b<span class='match'>asis<\/span><\/li>\n  <li>b<span class='match'>ecom<\/span>e<\/li>\n  <li>b<span class='match'>efor<\/span>e<\/li>\n  <li>b<span class='match'>egin<\/span><\/li>\n  <li>b<span class='match'>ehin<\/span>d<\/li>\n  <li>b<span class='match'>enefit<\/span><\/li>\n  <li>b<span class='match'>usines<\/span>s<\/li>\n  <li>ch<span class='match'>arac<\/span>ter<\/li>\n  <li>cl<span class='match'>oses<\/span><\/li>\n  <li>comm<span class='match'>unit<\/span>y<\/li>\n  <li>cons<span class='match'>ider<\/span><\/li>\n  <li>c<span class='match'>over<\/span><\/li>\n  <li>d<span class='match'>ebat<\/span>e<\/li>\n  <li>d<span class='match'>ecid<\/span>e<\/li>\n  <li>d<span class='match'>ecis<\/span>ion<\/li>\n  <li>d<span class='match'>efinit<\/span>e<\/li>\n  <li>d<span class='match'>epar<\/span>tment<\/li>\n  <li>d<span class='match'>epen<\/span>d<\/li>\n  <li>d<span class='match'>esig<\/span>n<\/li>\n  <li>d<span class='match'>evelop<\/span><\/li>\n  <li>diff<span class='match'>eren<\/span>ce<\/li>\n  <li>diff<span class='match'>icul<\/span>t<\/li>\n  <li>d<span class='match'>irec<\/span>t<\/li>\n  <li>d<span class='match'>ivid<\/span>e<\/li>\n  <li>d<span class='match'>ocumen<\/span>t<\/li>\n  <li>d<span class='match'>urin<\/span>g<\/li>\n  <li><span class='match'>econom<\/span>y<\/li>\n  <li><span class='match'>educat<\/span>e<\/li>\n  <li><span class='match'>elec<\/span>t<\/li>\n  <li><span class='match'>elec<\/span>tric<\/li>\n  <li><span class='match'>eleven<\/span><\/li>\n  <li>enco<span class='match'>urag<\/span>e<\/li>\n  <li>env<span class='match'>iron<\/span>ment<\/li>\n  <li>e<span class='match'>urop<\/span>e<\/li>\n  <li><span class='match'>even<\/span><\/li>\n  <li><span class='match'>evenin<\/span>g<\/li>\n  <li><span class='match'>ever<\/span><\/li>\n  <li><span class='match'>ever<\/span>y<\/li>\n  <li><span class='match'>eviden<\/span>ce<\/li>\n  <li><span class='match'>exac<\/span>t<\/li>\n  <li><span class='match'>exam<\/span>ple<\/li>\n  <li><span class='match'>exer<\/span>cise<\/li>\n  <li><span class='match'>exis<\/span>t<\/li>\n  <li>f<span class='match'>amil<\/span>y<\/li>\n  <li>f<span class='match'>igur<\/span>e<\/li>\n  <li>f<span class='match'>inal<\/span><\/li>\n  <li>f<span class='match'>inan<\/span>ce<\/li>\n  <li>f<span class='match'>inis<\/span>h<\/li>\n  <li>fr<span class='match'>iday<\/span><\/li>\n  <li>f<span class='match'>utur<\/span>e<\/li>\n  <li>g<span class='match'>eneral<\/span><\/li>\n  <li>g<span class='match'>over<\/span>n<\/li>\n  <li>h<span class='match'>oliday<\/span><\/li>\n  <li>h<span class='match'>ones<\/span>t<\/li>\n  <li>hosp<span class='match'>ital<\/span><\/li>\n  <li>h<span class='match'>owever<\/span><\/li>\n  <li><span class='match'>iden<\/span>tify<\/li>\n  <li><span class='match'>imagin<\/span>e<\/li>\n  <li>ind<span class='match'>ivid<\/span>ual<\/li>\n  <li>int<span class='match'>eres<\/span>t<\/li>\n  <li>intr<span class='match'>oduc<\/span>e<\/li>\n  <li><span class='match'>item<\/span><\/li>\n  <li>j<span class='match'>esus<\/span><\/li>\n  <li>l<span class='match'>evel<\/span><\/li>\n  <li>l<span class='match'>ikel<\/span>y<\/li>\n  <li>l<span class='match'>imit<\/span><\/li>\n  <li>l<span class='match'>ocal<\/span><\/li>\n  <li>m<span class='match'>ajor<\/span><\/li>\n  <li>m<span class='match'>anag<\/span>e<\/li>\n  <li>me<span class='match'>anin<\/span>g<\/li>\n  <li>me<span class='match'>asur<\/span>e<\/li>\n  <li>m<span class='match'>inis<\/span>ter<\/li>\n  <li>m<span class='match'>inus<\/span><\/li>\n  <li>m<span class='match'>inut<\/span>e<\/li>\n  <li>m<span class='match'>omen<\/span>t<\/li>\n  <li>m<span class='match'>oney<\/span><\/li>\n  <li>m<span class='match'>usic<\/span><\/li>\n  <li>n<span class='match'>atur<\/span>e<\/li>\n  <li>n<span class='match'>eces<\/span>sary<\/li>\n  <li>n<span class='match'>ever<\/span><\/li>\n  <li>n<span class='match'>otic<\/span>e<\/li>\n  <li><span class='match'>okay<\/span><\/li>\n  <li><span class='match'>open<\/span><\/li>\n  <li><span class='match'>operat<\/span>e<\/li>\n  <li>opport<span class='match'>unit<\/span>y<\/li>\n  <li>org<span class='match'>aniz<\/span>e<\/li>\n  <li><span class='match'>original<\/span><\/li>\n  <li><span class='match'>over<\/span><\/li>\n  <li>p<span class='match'>aper<\/span><\/li>\n  <li>p<span class='match'>arag<\/span>raph<\/li>\n  <li>p<span class='match'>aren<\/span>t<\/li>\n  <li>part<span class='match'>icular<\/span><\/li>\n  <li>ph<span class='match'>otog<\/span>raph<\/li>\n  <li>p<span class='match'>olic<\/span>e<\/li>\n  <li>p<span class='match'>olic<\/span>y<\/li>\n  <li>p<span class='match'>olitic<\/span><\/li>\n  <li>p<span class='match'>osit<\/span>ion<\/li>\n  <li>p<span class='match'>ositiv<\/span>e<\/li>\n  <li>p<span class='match'>ower<\/span><\/li>\n  <li>pr<span class='match'>epar<\/span>e<\/li>\n  <li>pr<span class='match'>esen<\/span>t<\/li>\n  <li>pr<span class='match'>esum<\/span>e<\/li>\n  <li>pr<span class='match'>ivat<\/span>e<\/li>\n  <li>pr<span class='match'>obab<\/span>le<\/li>\n  <li>pr<span class='match'>oces<\/span>s<\/li>\n  <li>pr<span class='match'>oduc<\/span>e<\/li>\n  <li>pr<span class='match'>oduc<\/span>t<\/li>\n  <li>pr<span class='match'>ojec<\/span>t<\/li>\n  <li>pr<span class='match'>oper<\/span><\/li>\n  <li>pr<span class='match'>opos<\/span>e<\/li>\n  <li>pr<span class='match'>otec<\/span>t<\/li>\n  <li>pr<span class='match'>ovid<\/span>e<\/li>\n  <li>qu<span class='match'>alit<\/span>y<\/li>\n  <li>re<span class='match'>alis<\/span>e<\/li>\n  <li>re<span class='match'>ason<\/span><\/li>\n  <li>r<span class='match'>ecen<\/span>t<\/li>\n  <li>r<span class='match'>ecog<\/span>nize<\/li>\n  <li>r<span class='match'>ecom<\/span>mend<\/li>\n  <li>r<span class='match'>ecor<\/span>d<\/li>\n  <li>r<span class='match'>educ<\/span>e<\/li>\n  <li>r<span class='match'>efer<\/span><\/li>\n  <li>r<span class='match'>egar<\/span>d<\/li>\n  <li>r<span class='match'>elat<\/span>ion<\/li>\n  <li>r<span class='match'>emem<\/span>ber<\/li>\n  <li>r<span class='match'>epor<\/span>t<\/li>\n  <li>repr<span class='match'>esen<\/span>t<\/li>\n  <li>r<span class='match'>esul<\/span>t<\/li>\n  <li>r<span class='match'>etur<\/span>n<\/li>\n  <li>s<span class='match'>atur<\/span>day<\/li>\n  <li>s<span class='match'>econ<\/span>d<\/li>\n  <li>secr<span class='match'>etar<\/span>y<\/li>\n  <li>s<span class='match'>ecur<\/span>e<\/li>\n  <li>s<span class='match'>eparat<\/span>e<\/li>\n  <li>s<span class='match'>even<\/span><\/li>\n  <li>s<span class='match'>imilar<\/span><\/li>\n  <li>sp<span class='match'>ecific<\/span><\/li>\n  <li>str<span class='match'>ateg<\/span>y<\/li>\n  <li>st<span class='match'>uden<\/span>t<\/li>\n  <li>st<span class='match'>upid<\/span><\/li>\n  <li>t<span class='match'>elep<\/span>hone<\/li>\n  <li>t<span class='match'>elevis<\/span>ion<\/li>\n  <li>th<span class='match'>erefor<\/span>e<\/li>\n  <li>tho<span class='match'>usan<\/span>d<\/li>\n  <li>t<span class='match'>oday<\/span><\/li>\n  <li>t<span class='match'>oget<\/span>her<\/li>\n  <li>t<span class='match'>omor<\/span>row<\/li>\n  <li>t<span class='match'>onig<\/span>ht<\/li>\n  <li>t<span class='match'>otal<\/span><\/li>\n  <li>t<span class='match'>owar<\/span>d<\/li>\n  <li>tr<span class='match'>avel<\/span><\/li>\n  <li><span class='match'>unit<\/span><\/li>\n  <li><span class='match'>unit<\/span>e<\/li>\n  <li><span class='match'>univer<\/span>sity<\/li>\n  <li><span class='match'>upon<\/span><\/li>\n  <li>v<span class='match'>isit<\/span><\/li>\n  <li>w<span class='match'>ater<\/span><\/li>\n  <li>w<span class='match'>oman<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Question 4

**Solve the beginner regexp crosswords [here](https://regexcrossword.com/challenges/beginner).**

The following images are the results for the crosswords.


![](figs/fig1.png)    
![](figs/fig2.png)  
![](figs/fig3.png)  
![](figs/fig4.png)  
![](figs/fig5.png)  
