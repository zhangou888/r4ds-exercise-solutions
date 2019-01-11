
# Iteration

## Introduction


```r
library("tidyverse")
library("stringr")
```
The package **microbenchmark** is used for timing code

```r
library("microbenchmark")
```

## For Loops

### Exercise <span class="exercise-number">21.2.1</span> {.unnumbered .exercise}

<div class="question">

Write for-loops to:

1.  Compute the mean of every column in `mtcars`.
1.  Determine the type of each column in `nycflights13::flights`.
1.  Compute the number of unique values in each column of `iris`.
1.  Generate 10 random normals for each of $\mu = -10$, 0, 10, and 100.

</div>

<div class="answer">
The answers for each part are below.

1.  To compute the mean of every column in `mtcars`.
    
    ```r
    output <- vector("double", ncol(mtcars))
    names(output) <- names(mtcars)
    for (i in names(mtcars)) {
      output[i] <- mean(mtcars[[i]])
    }
    output
    #>     mpg     cyl    disp      hp    drat      wt    qsec      vs      am 
    #>  20.091   6.188 230.722 146.688   3.597   3.217  17.849   0.438   0.406 
    #>    gear    carb 
    #>   3.688   2.812
    ```

1.  Determine the type of each column in `nycflights13::flights`.
    Note that we need to use a `list`, not a character vector, since the class can have multiple values.
    
    ```r
    data("flights", package = "nycflights13")
    output <- vector("list", ncol(flights))
    names(output) <- names(flights)
    for (i in names(flights)) {
      output[[i]] <- class(flights[[i]])
    }
    output
    #> $year
    #> [1] "integer"
    #> 
    #> $month
    #> [1] "integer"
    #> 
    #> $day
    #> [1] "integer"
    #> 
    #> $dep_time
    #> [1] "integer"
    #> 
    #> $sched_dep_time
    #> [1] "integer"
    #> 
    #> $dep_delay
    #> [1] "numeric"
    #> 
    #> $arr_time
    #> [1] "integer"
    #> 
    #> $sched_arr_time
    #> [1] "integer"
    #> 
    #> $arr_delay
    #> [1] "numeric"
    #> 
    #> $carrier
    #> [1] "character"
    #> 
    #> $flight
    #> [1] "integer"
    #> 
    #> $tailnum
    #> [1] "character"
    #> 
    #> $origin
    #> [1] "character"
    #> 
    #> $dest
    #> [1] "character"
    #> 
    #> $air_time
    #> [1] "numeric"
    #> 
    #> $distance
    #> [1] "numeric"
    #> 
    #> $hour
    #> [1] "numeric"
    #> 
    #> $minute
    #> [1] "numeric"
    #> 
    #> $time_hour
    #> [1] "POSIXct" "POSIXt"
    ```

1.  To compute the number of unique values in each column of the `iris` dataset.
    
    ```r
    data("iris")
    iris_uniq <- vector("double", ncol(iris))
    names(iris_uniq) <- names(iris)
    for (i in names(iris)) {
      iris_uniq[i] <- length(unique(iris[[i]]))
    }
    iris_uniq
    #> Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
    #>           35           23           43           22            3
    ```

1.  To generate 10 random normals for each of $\mu = -10$, 0, 10, and 100.
    
    ```r
    # number to draw
    n <- 10
    # values of the mean
    mu <- c(-10, 0, 10, 100)
    normals <- vector("list", length(mu))
    for (i in seq_along(normals)) {
      normals[[i]] <- rnorm(n, mean = mu[i])
    }
    normals
    #> [[1]]
    #>  [1] -11.40  -9.74 -12.44 -10.01  -9.38  -8.85 -11.82 -10.25 -10.24 -10.28
    #> 
    #> [[2]]
    #>  [1] -0.5537  0.6290  2.0650 -1.6310  0.5124 -1.8630 -0.5220 -0.0526
    #>  [9]  0.5430 -0.9141
    #> 
    #> [[3]]
    #>  [1] 10.47 10.36  8.70 10.74 11.89  9.90  9.06  9.98  9.17  8.49
    #> 
    #> [[4]]
    #>  [1] 100.9 100.2 100.2 101.6 100.1  99.9  98.1  99.7  99.7 101.1
    ```

However, we don't need a for loop for this since `rnorm()` recycle the `mean` argument.

```r
matrix(rnorm(n * length(mu), mean = mu), ncol = n)
#>        [,1]   [,2]  [,3]     [,4]   [,5]   [,6]    [,7]   [,8]    [,9]
#> [1,] -9.930  -9.56 -9.88 -10.2061 -12.27 -8.926 -11.178  -9.51  -8.663
#> [2,] -0.639   2.76 -1.91   0.0192   2.68 -0.665  -0.976  -1.70   0.237
#> [3,]  9.950  10.05 10.86  10.0296   9.64 11.114  11.065   8.53  11.318
#> [4,] 99.749 100.58 99.76 100.5498 100.21 99.754 100.132 100.28 100.524
#>      [,10]
#> [1,] -9.39
#> [2,] -0.11
#> [3,] 10.17
#> [4,] 99.91
```
</div>

### Exercise <span class="exercise-number">21.2.2</span> {.unnumbered .exercise}

<div class="question">
Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors:
</div>

<div class="answer">


```r
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}
out
#> [1] "abcdefghijklmnopqrstuvwxyz"
```

Since `str_c()` already works with vectors, use `str_c()` with the `collapse` argument to return a single string.

```r
stringr::str_c(letters, collapse = "")
#> [1] "abcdefghijklmnopqrstuvwxyz"
```

For this I'm going to rename the variable `sd` to something different because `sd` is the name of the function we want to use.


```r
x <- sample(100)
sd. <- 0
for (i in seq_along(x)) {
  sd. <- sd. + (x[i] - mean(x))^2
}
sd. <- sqrt(sd. / (length(x) - 1))
sd.
#> [1] 29
```

We could simply use the `sd` function.

```r
sd(x)
#> [1] 29
```
Or if there was a need to use the equation (e.g. for pedagogical reasons), then
the functions `mean()` and `sum()` already work with vectors:

```r
sqrt(sum((x - mean(x))^2) / (length(x) - 1))
#> [1] 29
```


```r
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}
out
#>   [1]  0.126  1.064  1.865  2.623  3.156  3.703  3.799  4.187  4.359  5.050
#>  [11]  5.725  6.672  6.868  7.836  8.224  8.874  9.688  9.759 10.286 11.050
#>  [21] 11.485 12.038 12.242 12.273 13.242 13.421 14.199 15.085 15.921 16.527
#>  [31] 17.434 17.470 17.601 17.695 18.392 18.797 18.863 18.989 19.927 20.143
#>  [41] 20.809 21.013 21.562 22.389 22.517 22.778 23.066 23.081 23.935 24.349
#>  [51] 25.100 25.819 26.334 27.309 27.670 27.840 28.623 28.654 29.444 29.610
#>  [61] 29.639 30.425 31.250 32.216 32.594 32.769 33.372 34.178 34.215 34.947
#>  [71] 35.163 35.179 35.307 35.993 36.635 36.963 37.350 38.058 38.755 39.681
#>  [81] 40.140 40.736 40.901 41.468 42.366 42.960 43.792 44.386 45.165 45.562
#>  [91] 46.412 47.154 47.472 47.583 47.685 48.485 48.865 48.917 49.904 50.508
```

The code above is calculating a cumulative sum. Use the function `cumsum()`

```r
all.equal(cumsum(x), out)
#> [1] TRUE
```

</div>

### Exercise <span class="exercise-number">21.2.3</span> {.unnumbered .exercise}

<div class="question">

Combine your function writing and for loop skills:

1.  Write a for loop that `prints()` the lyrics to the children's song  "Alice the camel".

1.  Convert the nursery rhyme "ten in the bed" to a function. Generalize it to any number of people in any sleeping structure.

1.  Convert the song "99 bottles of beer on the wall" to a function.
Generalize to any number of any vessel containing any liquid on  surface.

</div>

<div class="answer">

The answers to each part follow.

1.  The lyrics for [Alice the Camel](http://www.kididdles.com/lyrics/a012.html) are:

    > Alice the camel has five humps. \
    > Alice the camel has five humps. \
    > Alice the camel has five humps. \
    > So go, Alice, go.

    This verse is repeated, each time with one fewer hump,
    until there are no humps.
    The last verse, with no humps, is:

    > Alice the camel has no humps. \
    > Alice the camel has no humps. \
    > Alice the camel has no humps. \
    > Now Alice is a horse.

    We'll iterate from five to no humps, and print out a different last line if there are no humps.
    
    ```r
    humps <- c("five", "four", "three", "two", "one", "no")
    for (i in humps) {
      cat(str_c("Alice the camel has ", rep(i, 3), " humps.",
        collapse = "\n"
      ), "\n")
      if (i == "no") {
        cat("Now Alice is a horse.\n")
      } else {
        cat("So go, Alice, go.\n")
      }
      cat("\n")
    }
    #> Alice the camel has five humps.
    #> Alice the camel has five humps.
    #> Alice the camel has five humps. 
    #> So go, Alice, go.
    #> 
    #> Alice the camel has four humps.
    #> Alice the camel has four humps.
    #> Alice the camel has four humps. 
    #> So go, Alice, go.
    #> 
    #> Alice the camel has three humps.
    #> Alice the camel has three humps.
    #> Alice the camel has three humps. 
    #> So go, Alice, go.
    #> 
    #> Alice the camel has two humps.
    #> Alice the camel has two humps.
    #> Alice the camel has two humps. 
    #> So go, Alice, go.
    #> 
    #> Alice the camel has one humps.
    #> Alice the camel has one humps.
    #> Alice the camel has one humps. 
    #> So go, Alice, go.
    #> 
    #> Alice the camel has no humps.
    #> Alice the camel has no humps.
    #> Alice the camel has no humps. 
    #> Now Alice is a horse.
    ```

1.  The lyrics for [Ten in the Bed](https://www.kididdles.com/lyrics/t003.html) are:

    > Here we go! \
    > There were ten in the bed \
    > and the little one said, \
    > “Roll over, roll over.” \
    > So they all rolled over and one fell out.

    This verse is repeated, each time with one fewer in the bed, until there is one left. That last verse is:

    > One!
    > There was one in the bed \
    > and the little one said, \
    > “I’m lonely...”

    
    ```r
    numbers <- c(
      "ten", "nine", "eight", "seven", "six", "five",
      "four", "three", "two", "one"
    )
    for (i in numbers) {
      cat(str_c("There were ", i, " in the bed\n"))
      cat("and the little one said\n")
      if (i == "one") {
        cat("I'm lonely...")
      } else {
        cat("Roll over, roll over\n")
        cat("So they all rolled over and one fell out.\n")
      }
      cat("\n")
    }
    #> There were ten in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were nine in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were eight in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were seven in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were six in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were five in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were four in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were three in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were two in the bed
    #> and the little one said
    #> Roll over, roll over
    #> So they all rolled over and one fell out.
    #> 
    #> There were one in the bed
    #> and the little one said
    #> I'm lonely...
    ```

1.  The lyrics of [Ninety-Nine Bottles of Beer on the Wall](https://en.wikipedia.org/wiki/99_Bottles_of_Beer) are

    > 99 bottles of beer on the wall, 99 bottles of beer. \
    > Take one down, pass it around, 98 bottles of beer on the wall

    This verse is repeated, each time with one few bottle, until
    there are no more bottles of beer. The last verse is

    > No more bottles of beer on the wall, no more bottles of beer. \
    > We've taken them down and passed them around; now we're drunk and passed out!

    For the bottles of beer, I define a helper function to correctly print the number of bottles.

    
    ```r
    bottles <- function(i) {
      if (i > 2) {
        bottles <- str_c(i - 1, " bottles")
      } else if (i == 2) {
        bottles <- "1 bottle"
      } else {
        bottles <- "no more bottles"
      }
      bottles
    }
    
    beer_bottles <- function(n) {
      # should test whether n >= 1.
      for (i in seq(n, 1)) {
        cat(str_c(bottles(i), " of beer on the wall, ", bottles(i), " of beer.\n"))
        cat(str_c(
          "Take one down and pass it around, ", bottles(i - 1),
          " of beer on the wall.\n\n"
        ))
      }
      cat("No more bottles of beer on the wall, no more bottles of beer.\n")
      cat(str_c("Go to the store and buy some more, ", bottles(n), " of beer on the wall.\n"))
    }
    beer_bottles(3)
    #> 2 bottles of beer on the wall, 2 bottles of beer.
    #> Take one down and pass it around, 1 bottle of beer on the wall.
    #> 
    #> 1 bottle of beer on the wall, 1 bottle of beer.
    #> Take one down and pass it around, no more bottles of beer on the wall.
    #> 
    #> no more bottles of beer on the wall, no more bottles of beer.
    #> Take one down and pass it around, no more bottles of beer on the wall.
    #> 
    #> No more bottles of beer on the wall, no more bottles of beer.
    #> Go to the store and buy some more, 2 bottles of beer on the wall.
    ```

</div>

#### Exercise <span class="exercise-number">21.2.4</span> {.unnumbered .exercise}

<div class="question">
It's common to see for loops that don't preallocate the output and instead increase the length of a vector at each step:


```r
output <- vector("integer", 0)
for (i in seq_along(x)) {
  output <- c(output, lengths(x[[i]]))
}
output
```
How does this affect performance? Design and execute an experiment.
</div>

<div class="answer">
I'll use the package **microbenchmark** to time this.
The `microbenchmark()` function will run an R expression a number of times and time it.

Define a function that appends to an integer vector.

```r
add_to_vector <- function(n) {
  output <- vector("integer", 0)
  for (i in seq_len(n)) {
    output <- c(output, i)
  }
  output
}
microbenchmark(add_to_vector(10000), times = 3)
#> Unit: milliseconds
#>                  expr min  lq mean median  uq max neval
#>  add_to_vector(10000) 133 136  153    138 163 189     3
```

And one that pre-allocates it.

```r
add_to_vector_2 <- function(n) {
  output <- vector("integer", n)
  for (i in seq_len(n)) {
    output[[i]] <- i
  }
  output
}
microbenchmark(add_to_vector_2(10000), times = 3)
#> Unit: microseconds
#>                    expr min   lq mean median   uq  max neval
#>  add_to_vector_2(10000) 565 3009 4979   5452 7186 8919     3
```

The pre-allocated vector is about **10** times faster!
You may get different answers, but the longer the vector and the bigger the objects, the more that pre-allocation will outperform appending.

</div>

## For loop variations

### Exercise <span class="exercise-number">21.3.1</span> {.unnumbered .exercise}

<div class="question">
Imagine you have a directory full of CSV files that you want to read in.
You have their paths in a vector,
`files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)`, and now
want to read each one with `read_csv()`. Write the for loop that will
load them into a single data frame.

</div>

<div class="answer">

First, I pre-allocate a list. Then, I read each file into a data frame, and assign it to an element in that list.
The result is a list of data frames.
I then use `bind_rows()` to combine the list of data frames into a single data frame.

```r
df <- vector("list", length(files))
for (fname in seq_along(files)) {
  df[[i]] <- read_csv(files[[i]])
}
df <- bind_rows(df)
```

</div>

### Exercise <span class="exercise-number">21.3.2</span> {.unnumbered .exercise}

<div class="question">
What happens if you use `for (nm in names(x))` and `x` has no names?
What if only some of the elements are named?
What if the names are not unique?
</div>

<div class="answer">

Let's try it out and see what happens.

When there are no names for the vector, it does not run the code in the loop (it runs zero iterations of the loop):

```r
x <- 1:3
print(names(x))
#> NULL
for (nm in names(x)) {
  print(nm)
  print(x[[nm]])
}
```
Note that the length of `NULL` is zero:

```r
length(NULL)
#> [1] 0
```

If there only some names, then we get an error if we try to access an element without a name.
However, oddly, `nm == ""` when there is no name.

```r
x <- c(a = 1, 2, c = 3)
names(x)
#> [1] "a" ""  "c"
```

```r
for (nm in names(x)) {
  print(nm)
  print(x[[nm]])
}
#> [1] "a"
#> [1] 1
#> [1] ""
#> Error in x[[nm]]: subscript out of bounds
```

Finally, if there are duplicate names, then `x[[nm]]` will give the *first* element with that name.
There is no way to access elements with duplicate names.

```r
x <- c(a = 1, a = 2, c = 3)
names(x)
#> [1] "a" "a" "c"
```

```r
for (nm in names(x)) {
  print(nm)
  print(x[[nm]])
}
#> [1] "a"
#> [1] 1
#> [1] "a"
#> [1] 1
#> [1] "c"
#> [1] 3
```

</div>

### Exercise <span class="exercise-number">21.3.3</span> {.unnumbered .exercise}  

<div class="question">
Write a function that prints the mean of each numeric column in a data  frame, along with its name.
For example, `show_mean(iris)` would print:
</div>

<div class="answer">


```r
show_mean(iris)
# > Sepal.Length: 5.84
# > Sepal.Width:  3.06
# > Petal.Length: 3.76
# > Petal.Width:  1.20
```

> (Extra challenge: what function did I use to make sure that the numbers lined up nicely, even though the variable names had different lengths?)

There may be other functions to do this, but I'll use `str_pad()`, and `str_length()` to ensure that the space given to the variable names is the same.
I messed around with the options to `format()` until I got two digits .

```r
show_mean <- function(df, digits = 2) {
  # Get max length of all variable names in the dataset
  maxstr <- max(str_length(names(df)))
  for (nm in names(df)) {
    if (is.numeric(df[[nm]])) {
      cat(
        str_c(str_pad(str_c(nm, ":"), maxstr + 1L, side = "right"),
          format(mean(df[[nm]]), digits = digits, nsmall = digits),
          sep = " "
        ),
        "\n"
      )
    }
  }
}
show_mean(iris)
#> Sepal.Length: 5.84 
#> Sepal.Width:  3.06 
#> Petal.Length: 3.76 
#> Petal.Width:  1.20
```

</div>

### Exercise <span class="exercise-number">21.3.4</span> {.unnumbered .exercise}

<div class="question">
What does this code do? How does it work?
</div>

<div class="answer">


```r
trans <- list(
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
```

```r
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}
```

This code mutates the `disp` and `am` columns:

-   `disp` is  multiplied by 0.0163871
-   `am` is replaced by a factor variable.

The code works by looping over a named list of functions.
It calls the named function in the list on the column of `mtcars` with the same name, and replaces the values of that column.

E.g. this is a function:

```r
trans[["disp"]]
```
This applies the function to the column of `mtcars` with the same name

```r
trans[["disp"]](mtcars[["disp"]])
```

</div>

## For loops vs. functionals

### Exercise <span class="exercise-number">21.4.1</span> {.unnumbered .exercise}

<div class="question">
Read the documentation for `apply()`. In the 2d case, what two for loops does it generalize.
</div>

<div class="answer">

It generalizes looping over the rows or columns of a matrix or data-frame.

</div>

### Exercise <span class="exercise-number">21.4.2</span> {.unnumbered .exercise}

<div class="question">
Adapt `col_summary()` so that it only applies to numeric columns.
You might want to start with an `is_numeric()` function that returns a logical vector that has a `TRUE` corresponding to each numeric column.
</div>

<div class="answer">

The original `col_summary()` function is

```r
col_summary <- function(df, fun) {
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[i] <- fun(df[[i]])
  }
  out
}
```

The adapted version adds extra logic to only apply the function to numeric
columns.

```r
col_summary2 <- function(df, fun) {
  # create an empty vector which will store whether each
  # column is numeric
  numeric_cols <- vector("logical", length(df))
  # test whether each column is numeric
  for (i in seq_along(df)) {
    numeric_cols[[i]] <- is.numeric(df[[i]])
  }
  # find the indexes of the numeric columns
  idxs <- which(numeric_cols)
  # find the number of numeric columns
  n <- sum(numeric_cols)
  # create a vector to hold the results
  out <- vector("double", n)
  # apply the function only to numeric vectors
  for (i in seq_along(idxs)) {
    out[[i]] <- fun(df[[idxs[[i]]]])
  }
  # name the vector
  names(out) <- names(df)[idxs]
  out
}
```

Let's test that `col_summary2()` works by creating a small data frame with 
some numeric and non-numeric columns.

```r
df <- tibble(
  X1 = c(1, 2, 3),
  X2 = c("A", "B", "C"),
  X3 = c(0, -1, 5),
  X4 = c(TRUE, FALSE, TRUE)
)
col_summary2(df, mean)
#>   X1   X3 
#> 2.00 1.33
```
As expected, it only calculates the mean of the numeric columns, `X1` and `X3`.
Let's test that it works with another function.

```r
col_summary2(df, median)
#> X1 X3 
#>  2  0
```

</div>

## The map functions

### Exercise <span class="exercise-number">21.5.1</span> {.unnumbered .exercise}

<div class="question">
Write code that uses one of the map functions to:

1.  Compute the mean of every column in `mtcars`.
1.  Determine the type of each column in `nycflights13::flights`.
1.  Compute the number of unique values in each column of `iris`.
1.  Generate 10 random normals for each of $\mu = -10$, $0$, $10$, and $100$.

</div>

<div class="answer">

1.  To calculate the mean of every column in `mtcars`, apply the function
    `mean()` to each column, and use `map_dbl`, since the results are numeric.
    
    ```r
    map_dbl(mtcars, mean)
    #>     mpg     cyl    disp      hp    drat      wt    qsec      vs      am 
    #>  20.091   6.188 230.722 146.688   3.597   3.217  17.849   0.438   0.406 
    #>    gear    carb 
    #>   3.688   2.812
    ```

1.  To calculate the type of every column in `nycflights13::flights` apply
    the function `typeof()`, discussed in the section on [Vector basics](http://r4ds.had.co.nz/vectors.html#vector-basics), 
    and use `map_chr()`, since the results are character.
    
    ```r
    map_chr(nycflights13::flights, typeof)
    #>           year          month            day       dep_time sched_dep_time 
    #>      "integer"      "integer"      "integer"      "integer"      "integer" 
    #>      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
    #>       "double"      "integer"      "integer"       "double"    "character" 
    #>         flight        tailnum         origin           dest       air_time 
    #>      "integer"    "character"    "character"    "character"       "double" 
    #>       distance           hour         minute      time_hour 
    #>       "double"       "double"       "double"       "double"
    ```
    

1.  There is no function that directly calculates the number of unique values 
    in a vector. For a single column, the number of unique values of a vector
    can be calculated like so,
    
    ```r
    length(unique(iris$Species))
    #> [1] 3
    ```
    To apply this to all columns, we can provide the map an anonymous function.
    We can write anonymous function using the standard R syntax, like this,
    
    ```r
    map_int(iris, function(x) length(unique(x)))
    #> Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
    #>           35           23           43           22            3
    ```
    or using the more compact, one-sided formula shortcut that **purrr** provides,
    
    ```r
    map_int(iris, ~ length(unique(.)))
    #> Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
    #>           35           23           43           22            3
    ```
    The `map_int` function is used since `length()` returns an integer.
    However, `map_dbl` will also work,
    
    ```r
    map_dbl(iris, ~ length(unique(.)))
    ```

1.  To generate 10 random normals for each of $\mu = -10$, $0$, $10$, and $100$:
    The result is a list of numeric vectors.
    
    ```r
    map(c(-10, 0, 10, 100), ~ rnorm(n = 10, mean = .))
    #> [[1]]
    #>  [1]  -8.47  -9.45  -8.01 -10.15  -7.44  -8.94  -8.86  -8.88 -10.40 -10.82
    #> 
    #> [[2]]
    #>  [1] -0.5789  1.7638  0.1330  0.3765  1.1387  1.2413  0.6121 -0.4294
    #>  [9]  1.3605 -0.0709
    #> 
    #> [[3]]
    #>  [1]  9.73  7.55 10.07  8.90  9.37  7.94 12.65  8.85  9.66 10.79
    #> 
    #> [[4]]
    #>  [1]  98.7 100.5 100.1 100.6 100.4  98.5 100.9  99.7  99.9 100.0
    ```
    Since a single call of `rnorm()` returns a numeric vector with a length greater
    than one we cannot use `map_dbl`, which requires the function to return a numeric
    vector that is only length one (see [Exercise 21.5.3.4](#exercise-21.5.3.4)). 
    The map functions pass any additional arguments to the function being called.

</div>

### Exercise <span class="exercise-number">21.5.2</span> {.unnumbered .exercise}  

<div class="question">
How can you create a single vector that for each column in a data frame indicates whether or not it's a factor?
</div>

<div class="answer">

The function `is.factor` indicates whether a vector is a factor. For example,

```r
is.factor(iris$Species)
#> [1] TRUE
is.factor(iris$Sepal.Length)
#> [1] FALSE
```
To apply this to all columns in a data frame, use `map_lgl` since the result
of `is.factor` is logical. For example,

```r
map_lgl(iris, is.factor)
#> Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
#>        FALSE        FALSE        FALSE        FALSE         TRUE
```

</div>

### Exercise <span class="exercise-number">21.5.3</span> {.unnumbered .exercise}

<div class="question">
What happens when you use the map functions on vectors that aren't lists?
What does `map(1:5, runif)` do?
Why?
</div>

<div class="answer">

Map functions work with any vectors, not just lists. 
As with lists, the map functions will apply the function to each element of the vector.

The expression `map(1:5, runif)` is equivalent to running `runif(1)`, `runif(2)`,
..., `runif(5)`, and collecting the results in a list.
The result is a length five list with numeric vectors of sizes one through five.

```r
map(1:5, runif)
#> [[1]]
#> [1] 0.549
#> 
#> [[2]]
#> [1] 0.650 0.159
#> 
#> [[3]]
#> [1] 0.294 0.891 0.588
#> 
#> [[4]]
#> [1] 0.633 0.260 0.318 0.339
#> 
#> [[5]]
#> [1] 0.7608 0.0109 0.0631 0.2638 0.5938
```

</div>

### Exercise <span class="exercise-number">21.5.4</span> {.unnumbered .exercise}

<div class="question">
What does `map(-2:2, rnorm, n = 5)` do? Why?

What does `map_dbl(-2:2, rnorm, n = 5)` do? Why?
</div>

<div class="answer">

Consider the first expression.

```r
map(-2:2, rnorm, n = 5)
#> [[1]]
#> [1] -2.53 -1.29 -2.25 -1.32 -2.60
#> 
#> [[2]]
#> [1] -0.8899 -0.6280 -1.6099  0.0439 -1.3027
#> 
#> [[3]]
#> [1]  1.417 -0.867 -2.218 -0.275  0.377
#> 
#> [[4]]
#> [1] 1.972 1.259 2.207 0.731 0.302
#> 
#> [[5]]
#> [1] 0.512 4.463 2.569 0.889 3.249
```
This takes samples size five from five normal distributions, each with a different mean of (-2, -1, 0, 1, and 2) but all have the same standard deviation (1). It returns a list with each element a numeric vectors of length 5.

However, if we use `map_dbl` it throws an error. 

```r
map_dbl(-2:2, rnorm, n = 5)
#> Error: Result 1 is not a length 1 atomic vector
```
This is because `map_dbl` expects the function it applies to each element to 
return a numeric vector of length one.

If we wanted a numeric vector, we could use `map()` followed by `flatten_dbl()`,

```r
flatten_dbl(map(-2:2, rnorm, n = 5))
#>  [1] -0.94550 -2.82098 -2.63763 -2.15312 -3.41641 -0.39268 -0.91161
#>  [8] -2.57027 -0.68716 -0.34732 -0.00796  1.72703  2.08647 -0.35835
#> [15] -1.44212  1.38128  1.09070  1.16210  1.35595  0.64029  1.89137
#> [22]  3.82779  0.03808  2.94598  2.54896
```

</div>

### Exercise <span class="exercise-number">21.5.5</span> {.unnumbered .exercise}

<div class="question">
Rewrite `map(x, function(df) lm(mpg ~ wt, data = df))` to eliminate the anonymous function.
</div>

<div class="answer">


```r
map(list(mtcars), ~ lm(mpg ~ wt, data = .))
#> [[1]]
#> 
#> Call:
#> lm(formula = mpg ~ wt, data = .)
#> 
#> Coefficients:
#> (Intercept)           wt  
#>       37.29        -5.34
```

</div>

## Dealing with Failure

<!--html_preserve--><div class="alert alert-warning hints-alert">
<div class="hints-icon">
<i class="fa fa-exclamation-circle"></i>
</div>
<div class="hints-container">No exercises</div>
</div><!--/html_preserve-->

## Mapping over multiple arguments

<!--html_preserve--><div class="alert alert-warning hints-alert">
<div class="hints-icon">
<i class="fa fa-exclamation-circle"></i>
</div>
<div class="hints-container">No exercises</div>
</div><!--/html_preserve-->

## Walk

<!--html_preserve--><div class="alert alert-warning hints-alert">
<div class="hints-icon">
<i class="fa fa-exclamation-circle"></i>
</div>
<div class="hints-container">No exercises</div>
</div><!--/html_preserve-->

## Other patterns of for loops

### Exercise <span class="exercise-number">21.9.1</span> {.unnumbered .exercise}

<div class="question">

Implement your own version of `every()` using a for loop.
Compare it with `purrr::every()`.
What does purrr's version do that your version doesn't?

</div>

<div class="answer">


```r
# Use ... to pass arguments to the function
every2 <- function(.x, .p, ...) {
  for (i in .x) {
    if (!.p(i, ...)) {
      # If any is FALSE we know not all of then were TRUE
      return(FALSE)
    }
  }
  # if nothing was FALSE, then it is TRUE
  TRUE
}

every2(1:3, function(x) {
  x > 1
})
#> [1] FALSE
every2(1:3, function(x) {
  x > 0
})
#> [1] TRUE
```

The function `purrr::every()` does fancy things with `.p`, like taking a logical vector instead of a function, or being able to test part of a string if the elements of `.x` are lists.
</div>

### Exercise <span class="exercise-number">21.9.2</span> {.unnumbered .exercise}

<div class="question">
Create an enhanced `col_sum()` that applies a summary function to every numeric column in a data frame.
</div>

<div class="answer">

**Note:** this question has a typo. It is referring to `col_summary()`.

I will use `map` to apply the function to all the columns, and `keep` to only select numeric columns.

```r
col_sum2 <- function(df, f, ...) {
  map(keep(df, is.numeric), f, ...)
}
```


```r
col_sum2(iris, mean)
#> $Sepal.Length
#> [1] 5.84
#> 
#> $Sepal.Width
#> [1] 3.06
#> 
#> $Petal.Length
#> [1] 3.76
#> 
#> $Petal.Width
#> [1] 1.2
```

</div>

### Exercise <span class="exercise-number">21.9.3</span> {.unnumbered .exercise}

<div class="question">
A possible base R equivalent of `col_sum()` is:


```r
col_sum3 <- function(df, f) {
  is_num <- sapply(df, is.numeric)
  df_num <- df[, is_num]
  sapply(df_num, f)
}
```

But it has a number of bugs as illustrated with the following inputs:


```r
df <- tibble(
  x = 1:3,
  y = 3:1,
  z = c("a", "b", "c")
)

# OK
col_sum3(df, mean)
# Has problems: don't always return numeric vector
col_sum3(df[1:2], mean)
col_sum3(df[1], mean)
col_sum3(df[0], mean)
```

What causes these bugs?
</div>

<div class="answer">
The problem is that `sapply` does not always return numeric vectors.
If no columns are selected, instead of returning an empty numeric vector, it returns an empty list.
This causes an error since we can't use a list with `[`.

```r
sapply(df[0], is.numeric)
#> named list()
```

```r
sapply(df[1], is.numeric)
#>   X1 
#> TRUE
```

```r
sapply(df[1:2], is.numeric)
#>    X1    X2 
#>  TRUE FALSE
```
</div>

<!-- match unopened div --><div>
