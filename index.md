# Leaps – Regression Subset Selection

**Package Author:** Thomas Lumley <t.lumley@auckland.ac.nz>

**Project Author:** Josh Stim <jstim1@jh.edu>

### Package Description:

This package performs an exhaustive search for the best subsets of a
given set of potential regressors, using a branch-and-bound algorithm,
and also performs searches using a number of less time-consuming
techniques. It is designed to replace the “leaps()” command in S. It is
based on FORTRAN77 code by Alan Miller of CSIRO Division of Mathematics
& Statistics, which is described in more detail in his book “Subset
Selection in Regression”. Parts of the code have been published in the
Applied Statistics algorithms series.

There are several minor but useful improvements over the S
implementation. Firstly, there is no hard-coded limit to the number of
variables. Secondly, it is possible to restrict the search to subsets of
at most a specified size, potentially giving a large saving in time.
Thirdly, it is not necessary that the matrix of possible predictors be
of full rank. This is particularly useful when there are more predictors
than cases and the best “small” model is wanted. Fourthly, when there
are many more cases than predictors, the search can be run on the output
of biglm() and the time and memory use are then independent of the
number of observations.

### Exported Classes and Functions:

`leaps`

-   Subset selection by \`leaps and bounds’ compatible with S.

-   “…performs an exhaustive search for the best subsets of the
    variables in x for predicting y in linear regression, using an
    efficient branch-and-bound algorithm. It is a compatibility wrapper
    for regsubsets does the same thing better.” - From [R
    Documentation](https://cran.r-project.org/web/packages/leaps/leaps.pdf)

`regsubsets`

-   More sophisticated function, including formula method

-   “Model selection by exhaustive search, forward or backward stepwise,
    or sequential replacement.” - From [R
    Documentation](https://cran.r-project.org/web/packages/leaps/leaps.pdf)

### How to Install
``` r
install.packages("leaps")
library(leaps)
```

### `leaps` Example:
``` r
x<-matrix(rnorm(100),ncol=4)
y<-rnorm(25)
leaps(x,y)
```

    ## $which
    ##       1     2     3     4
    ## 1 FALSE FALSE  TRUE FALSE
    ## 1 FALSE  TRUE FALSE FALSE
    ## 1  TRUE FALSE FALSE FALSE
    ## 1 FALSE FALSE FALSE  TRUE
    ## 2 FALSE  TRUE  TRUE FALSE
    ## 2  TRUE FALSE  TRUE FALSE
    ## 2 FALSE FALSE  TRUE  TRUE
    ## 2  TRUE  TRUE FALSE FALSE
    ## 2 FALSE  TRUE FALSE  TRUE
    ## 2  TRUE FALSE FALSE  TRUE
    ## 3  TRUE  TRUE  TRUE FALSE
    ## 3 FALSE  TRUE  TRUE  TRUE
    ## 3  TRUE FALSE  TRUE  TRUE
    ## 3  TRUE  TRUE FALSE  TRUE
    ## 4  TRUE  TRUE  TRUE  TRUE
    ## 
    ## $label
    ## [1] "(Intercept)" "1"           "2"           "3"           "4"          
    ## 
    ## $size
    ##  [1] 2 2 2 2 3 3 3 3 3 3 4 4 4 4 5
    ## 
    ## $Cp
    ##  [1] 1.681903 2.536230 6.654005 6.688600 1.337056 3.544980 3.631842 3.859235
    ##  [9] 4.247740 8.348539 3.023003 3.323505 5.482288 5.658566 5.000000

### `regsubsets` Example:

``` r
data(swiss)
a<-regsubsets(as.matrix(swiss[,-1]),swiss[,1])
summary(a)
b<-regsubsets(Fertility~.,data=swiss,nbest=2)
summary(b)

coef(a, 1:3)
vcov(a, 3)
```

    ## Subset selection object
    ## 5 Variables  (and intercept)
    ##                  Forced in Forced out
    ## Agriculture          FALSE      FALSE
    ## Examination          FALSE      FALSE
    ## Education            FALSE      FALSE
    ## Catholic             FALSE      FALSE
    ## Infant.Mortality     FALSE      FALSE
    ## 1 subsets of each size up to 5
    ## Selection Algorithm: exhaustive
    ##          Agriculture Examination Education Catholic Infant.Mortality
    ## 1  ( 1 ) " "         " "         "*"       " "      " "             
    ## 2  ( 1 ) " "         " "         "*"       "*"      " "             
    ## 3  ( 1 ) " "         " "         "*"       "*"      "*"             
    ## 4  ( 1 ) "*"         " "         "*"       "*"      "*"             
    ## 5  ( 1 ) "*"         "*"         "*"       "*"      "*"             
    ## Subset selection object
    ## Call: UseMethod("regsubsets",x)
    ## 5 Variables  (and intercept)
    ##                  Forced in Forced out
    ## Agriculture          FALSE      FALSE
    ## Examination          FALSE      FALSE
    ## Education            FALSE      FALSE
    ## Catholic             FALSE      FALSE
    ## Infant.Mortality     FALSE      FALSE
    ## 2 subsets of each size up to 5
    ## Selection Algorithm: exhaustive
    ##          Agriculture Examination Education Catholic Infant.Mortality
    ## 1  ( 1 ) " "         " "         "*"       " "      " "             
    ## 1  ( 2 ) " "         "*"         " "       " "      " "             
    ## 2  ( 1 ) " "         " "         "*"       "*"      " "             
    ## 2  ( 2 ) " "         " "         "*"       " "      "*"             
    ## 3  ( 1 ) " "         " "         "*"       "*"      "*"             
    ## 3  ( 2 ) "*"         " "         "*"       "*"      " "             
    ## 4  ( 1 ) "*"         " "         "*"       "*"      "*"             
    ## 4  ( 2 ) " "         "*"         "*"       "*"      "*"             
    ## 5  ( 1 ) "*"         "*"         "*"       "*"      "*"             
    ## [[1]]
    ## (Intercept)   Education 
    ##  79.6100585  -0.8623503 
    ## 
    ## [[2]]
    ## (Intercept)   Education    Catholic 
    ##  74.2336892  -0.7883293   0.1109210 
    ## 
    ## [[3]]
    ##      (Intercept)        Education         Catholic Infant.Mortality 
    ##      48.67707330      -0.75924577       0.09606607       1.29614813 
    ## 
    ##                   (Intercept)     Education      Catholic Infant.Mortality
    ## (Intercept)      62.711883147 -0.2349982009 -0.0011120059     -2.952862263
    ## Education        -0.234998201  0.0136416868  0.0004427309      0.003360365
    ## Catholic         -0.001112006  0.0004427309  0.0007408169     -0.001716363
    ## Infant.Mortality -2.952862263  0.0033603646 -0.0017163629      0.149759535
