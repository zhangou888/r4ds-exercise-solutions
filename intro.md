
# Introduction



## Acknowledgments {-}



All the credit should go to Garrett Grolemund and Hadley Wickham for writing the truly fantastic *R for Data Science* book,
without which these solutions would not exist---literally.

Special thanks to [\@dongzhuoer](https://github.com/dongzhuoer) for a careful reading of the book
and noticing numerous issues and proposing fixes.

These solutions have benefited from those who fixed problems and contributed 
solutions. 
Thank you to all of those who contributed issues or pull-requests on
[GitHub](https://github.com/jrnold/r4ds-exercise-solutions/graphs/contributors) 
(in alphabetical order): [\@adamblake](https://github.com/adamblake), [\@benherbertson](https://github.com/benherbertson), [\@carajoos](https://github.com/carajoos), [\@dcgreaves](https://github.com/dcgreaves), [\@decoursin](https://github.com/decoursin), [\@dongzhuoer](https://github.com/dongzhuoer), [\@FluvialEDavis](https://github.com/FluvialEDavis), [\@GoldbergData](https://github.com/GoldbergData), [\@gvwilson](https://github.com/gvwilson), [\@JamesCuster](https://github.com/JamesCuster), [\@jmclawson](https://github.com/jmclawson), [\@KleinGeard](https://github.com/KleinGeard), [\@mjones01](https://github.com/mjones01), [\@mvhone](https://github.com/mvhone), [\@nickcorona](https://github.com/nickcorona), [\@nzxwang](https://github.com/nzxwang), [\@RandallEW](https://github.com/RandallEW), and[\@tinhb92](https://github.com/tinhb92)
Thank you to all of you who contributed annotations on [hypothes.is](https://hypothes.is/search?q=url%3Ajrnold.github.io%2Fr4ds-exercise-solutions%2F*) (in alphabetical order): [\@MajorosMask](https://hypothes.is/users/MajorosMask), and[\@sunxm03](https://hypothes.is/users/sunxm03).

## How this book is organized {-}

The book is divided into sections in with the same numbers and titles as those in *R for Data Science*. 
Not all sections have exercises.
Those sections without exercises have placeholder text indicating that there are no exercises.
The text for each exercise is followed by the solution. 

Like *R for Data Science*, packages used in each chapter are loaded in a code chunk at the start of the chapter in a section titled "Prerequisites".
If exercises depend on code in a section of *R for Data Science* it is either provided before the exercises or within the exercise solution.

If a package is used infrequently in solutions it may not be loaded, and functions using it will be called using the package name followed by two colons, as in `dplyr::mutate()` (see the *R for Data Science* [Introduction](http://r4ds.had.co.nz/introduction.html#running-r-code)).
The double colon may also be used to be explicit about the package from which a function comes.

## Prerequisites

This book is a complement to, not a substitute of, [R for Data Science]().
It only provides the exercise solutions for it. 
See the [R for Data Science](https://r4ds.had.co.nz/introduction.html#prerequisites) prerequisites.

Additional, the solutions use several packages that are not used in *R4DS*.
You can install all packages required to run the code in this book with the following line of code.

```r
devtools::install_github("jrnold/r4ds-exercise-solutions")
```

## Bugs/Contributing {-}

If you find any typos, errors in the solutions, have an alternative solution,
or think the solution could be improved, I would love your contributions.
The best way to contribute is through GitHub.
Please open an issue at <https://github.com/jrnold/r4ds-exercise-solutions/issues> or a pull request at
<https://github.com/jrnold/r4ds-exercise-solutions/pulls>.

## Colophon {-}



HTML and PDF versions of this book are available at <http://jrnold.github.io/r4ds-exercise-solutions>.
The book is powered by [bookdown](https://bookdown.org) which makes it easy to turn R markdown files into HTML, PDF, and EPUB.

The source of this book is available on GitHub at <https://github.com/jrnold/r4ds-exercise-solutions>.
This book was built from commit [be0c011](https://github.com/jrnold/r4ds-exercise-solutions/tree/be0c01116bf1601a381dec9f1867fd3fbe6fb8b6).

This book was built with these R packages.

```r
devtools::session_info("r4ds.exercise.solutions")
#> ─ Session info ──────────────────────────────────────────────────────────
#>  setting  value                       
#>  version  R version 3.5.2 (2017-01-27)
#>  os       Ubuntu 14.04.5 LTS          
#>  system   x86_64, linux-gnu           
#>  ui       X11                         
#>  language (EN)                        
#>  collate  en_US.UTF-8                 
#>  ctype    en_US.UTF-8                 
#>  tz       UTC                         
#>  date     2019-01-11                  
#> 
#> ─ Packages ──────────────────────────────────────────────────────────────
#>  ! package                 * version date lib source
#>  R r4ds.exercise.solutions   <NA>    <NA> [?] <NA>  
#> 
#> [1] /home/travis/R/Library
#> [2] /usr/local/lib/R/site-library
#> [3] /home/travis/R-bin/lib/R/library
#> 
#>  R ── Package was removed from disk.
```

<!-- match unopened div --><div>
