---
title: "Task 23"
subtitle: "Should we do it?"
author: "Kyle Tolliver"
date: "March 19, 2020"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    code_folding: hide
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---






```r
# Use this R-Chunk to import all your datasets!
let <- readr::read_lines("https://byuistats.github.io/M335/data/randomletters.txt") 
letnum <- readr::read_lines("https://byuistats.github.io/M335/data/randomletters_wnumbers.txt")
```

## Background

This week we will learn new coding techniques and visualization principles. Go back to task 11 and build your code into two or three functions that performs the same work for the tasks.

1. Your functions can allow the input of the spacing between characters or maybe a vector that picks of specific characters.
2. Your functions could remove letters after periods and put the sentence into one string.
3. Your function can allow the input of specific grep commands.

## Reading

* Chapter 29: R for Data Science - R Markdown format
* Chapter 19: R for Data Science - Functions

## Tasks

*Functions*

1. Your functions can allow the input of the spacing between characters or maybe a vector that picks of specific characters.
2. Your functions could remove letters after periods and put the sentence into one string.
3. Your function can allow the input of specific grep commands.

*Repeat the task 11 tasks using three functions that you built*

* [X] Use the readr::read_lines() function to read in each string
  + randomletters.txt 
  + randomletters_wnumbers.txt
* [X] With the randomletters.txt file
  + Pull out every 1700 letter (e.g. 1, 1700, 3400, …) 
  + Find the quote that is hidden - the quote ends with a period
* [X] With the randomletters_wnumbers.txt file
  + Find all the numbers hidden 
  + Convert those numbers to letters using the letters order in the alphabet to decipher the message
* [X] With the randomletters.txt file
  + Remove all the spaces and periods from the string then find the longest sequence of vowels

## Task 11

### Data Wrangling 


```r
letter1700 <- let %>% 
  str_split("") %>% 
  unlist() %>% 
  .[c(1, seq(0, str_count(let), 1700))] %>% 
  str_flatten() %>% 
  str_remove_all('\\..*$')

letter1700
```

```
## [1] "the plural of anecdote is not data"
```

```r
numlet <- letnum %>% 
  str_extract_all('[:digit:]+') %>% 
  unlist() %>% 
  as.integer() %>% 
  letters[.] %>% 
  paste(collapse = '')
  
numlet
```

```
## [1] "expertsoftenpossessmoredatathanjudgment"
```

```r
spacelet <- let %>% 
  str_remove_all('\\.') %>% 
  str_remove_all('[:space:]') %>% 
  str_locate('[aeiou]')
  
spacelet
```

```
##      start end
## [1,]     7   7
```

## Task 23

### R Functions


```r
pullout <- function(df, n){
  df %>% 
  str_split("") %>% 
  unlist() %>% 
  .[c(1, seq(0, str_count(df), n))] %>% 
  str_flatten() %>% 
  str_remove_all('\\..*$')
}

numberconvert <- function(df){
  df %>% 
  str_extract_all('[:digit:]+') %>% 
  unlist() %>% 
  as.integer() %>% 
  letters[.] %>% 
  paste(collapse = '')
}

spacevowels <- function(df){
  df %>% 
  str_remove_all('\\.') %>% 
  str_remove_all('[:space:]') %>% 
  str_locate('[aeiou]')

}
```

### Data Wrangling & Visualization 


```r
pullout(let, 1700)
```

```
## [1] "the plural of anecdote is not data"
```

```r
numberconvert(letnum)
```

```
## [1] "expertsoftenpossessmoredatathanjudgment"
```

```r
spacevowels(let)
```

```
##      start end
## [1,]     7   7
```