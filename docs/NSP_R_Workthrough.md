---
title: "R Workthrough"
author: "Avinash Iyer"
permalink: /NSP_R_Workthrough
output: 
  html_document:
    keep_md: true
---

## Getting Started

To store a vector to a variable, we do the following:


```r
x <- c(1,2,4)
```

We can also store this variable into a new variable:


```r
q <- c(x,x,8)
```

Then, we print `q`:


```r
q
```

```
## [1] 1 2 4 1 2 4 8
```

To pull out the individual element, we do the following:


```r
x[3]
```

```
## [1] 4
```

Elements of vectors in R are indexed at 1, **not** 0. For example, we would get an error if we tried to find `x[0]`. Importantly, we can also do subsets (or slicing, if we're talking about Python syntax).


```r
x[2:3]
```

```
## [1] 2 4
```

The syntax `x[2:3]` means we start at index 2 (or the 2nd element) and go until (and including) index 3 (or the 3rd element). For example, with our vector `q`:


```r
q[2:4]
```

```
## [1] 2 4 1
```

We can also find some values:


```r
mean(x)
```

```
## [1] 2.333333
```

```r
sd(x)
```

```
## [1] 1.527525
```

R also has a bunch of internal data sets that we can use for our own edification by typing in the following:


```r
data()
```

For example, we can use the dataset `Nile` for some fun:


```r
mean(Nile)
```

```
## [1] 919.35
```

```r
sd(Nile)
```

```
## [1] 169.2275
```

```r
hist(Nile)
```

![](NSP_R_Workthrough_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

To make a function, we can define as follows:


```r
oddcount <- function(x){
  k<- 0 # assign 0 to k
  for (n in x){
    if(n%% 2 == 1) k<-k+1 # %% is modulus
  }
  return(k)
}
```

We can test our `oddcount` function on various vectors:


```r
oddcount(c(1,3,5))
```

```
## [1] 3
```

```r
oddcount(c(1,3,7,9,2,4))
```

```
## [1] 4
```

In function `oddcount`, the values of `k` and `n` are local variables, and so don't really exist until the function is called. This is different from Python. Additionally, functions are passed by value rather than by reference. Let's create a function called `addone`, and see what happens to our vectors.


```r
addone <- function(x){
  for(i in 1:length(x)){
    x[i] <- x[i]+1
  }
  return(x)
}
z <- c(1,3,5)
addone(z)
```

```
## [1] 2 4 6
```

```r
z
```

```
## [1] 1 3 5
```

## Vectors
In R, there is no separate thing as a "scalar," since scalars are considered to be 1-dimensional vectors. We define vectors as follows:

```r
x <- c(1,3,4)
x
```

```
## [1] 1 3 4
```

```r
length(x)
```

```
## [1] 3
```
R automatically recycles (or basically copypastes) the shorter vector in addition until it reaches the length of the longer vector, as follows:

```r
y<- c(1,3,4,5,6)
y
```

```
## [1] 1 3 4 5 6
```

```r
z <- x+y
```

```
## Warning in x + y: longer object length is not a multiple of shorter object
## length
```

```r
z
```

```
## [1] 2 6 8 6 9
```
This is equivalent to adding `c(1,3,4,1,3)` to `c(1,3,4,5,6)`.

Multiplication is performed elementwise:

```r
w <- c(1,3,4) * c(6,7,8)
w
```

```
## [1]  6 21 32
```
This is true of other numeric operators:

```r
k <- w %% c(3,4,2)
k
```

```
## [1] 0 1 0
```

Alternatively, we can do the following:

```r
w %% 2
```

```
## [1] 0 1 0
```

We can also generate vectors with slicing operations:

```r
z <- 5:8
z
```

```
## [1] 5 6 7 8
```

If we wanted to extract all but the last element, we can do either of the following:

```r
z[1:(length(z)-1)]
```

```
## [1] 5 6 7
```

```r
z[-length(z)]
```

```
## [1] 5 6 7
```

Slicing is precedent over plus/minus, so be wary (which is why we put the `length(z)-1` in parentheses up there).

We can also make vectors from sequences as follows:

```r
seq(from=1.1,to=2,length=10)
```

```
##  [1] 1.1 1.2 1.3 1.4 1.5 1.6 1.7 1.8 1.9 2.0
```
Sequences help deal with the null vector problem:

```r
x <- c(5,12,13)
seq(x)
```

```
## [1] 1 2 3
```

```r
x <- NULL
seq(x)
```

```
## integer(0)
```

We can also use the `rep(x,times)` function to repeat vectors a number of times.

```r
x <- rep(8,4)
x
```

```
## [1] 8 8 8 8
```

We can also use the argument `each` within the `rep` function to change how numbers are repeated.

```r
rep(c(5,12,13),3)
```

```
## [1]  5 12 13  5 12 13  5 12 13
```

```r
rep(c(5,12,13),each=3)
```

```
## [1]  5  5  5 12 12 12 13 13 13
```

We can also use the functions `any` and `all` as "or" or "and" for vector elements:

```r
x <- 1:10
any(x>8)
```

```
## [1] TRUE
```

```r
all(x>3)
```

```
## [1] FALSE
```

```r
any(x>88)
```

```
## [1] FALSE
```

We can use the `any` and `all` functions to find the number of runs of 1s in a vector of 0s and 1s, as follows:

```r
findruns <- function(x,k){
  n <- length(x)
  runs <- NULL
  for(i in 1:(n-k+1)){
    if(all(x[i:(i+k-1)] == 1)){
      runs <- c(runs,i)
    }
  }
  return(runs)
}
findruns(c(1,1,0,0,1,1,1,0,1,1,1),2) # this function should return (1,5,6,9,10)
```

```
## [1]  1  5  6  9 10
```

Considering that this function creates a new vector every time it finds a run, this is very memory inefficient. We can solve this by initializing a new vector in `runs` and then cutting it towards the end.

```r
findruns1 <- function(x,k){
  n <- length(x)
  runs <- vector(length=n) # create a vector of length n at the beginning
  count <- 0
  for(i in 1:(n-k+1)){
    if(all(x[i:(i+k-1)] == 1)){
      count <- count + 1
      runs[count] <- i
    }
  }
  # this loop will change the vector elements in runs to the point where the runs start
  if(count > 0){
    runs <- runs[1:count]
  } else{
    runs <- NULL
  }
  return(runs)
}
findruns1(c(1,1,0,0,1,1,1,0,1,1,1),2)
```

```
## [1]  1  5  6  9 10
```
