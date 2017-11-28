
<!-- README.md is generated from README.Rmd. Please edit that file -->
tipR: R tools for tipping point sensitivity analyses
====================================================

[![Build Status](https://travis-ci.org/LucyMcGowan/tipr.svg?branch=master)](https://travis-ci.org/LucyMcGowan/tipr)

**Authors:** [Lucy D'Agostino McGowan](http://www.lucymcgowan.com)<br/> **License:** [MIT](https://opensource.org/licenses/MIT)

Installation
------------

``` r
# install.packages(devtools)
devtools::install_github("lucymcgowan/tipr")
```

``` r
library("tipr")
```

Usage
-----

After fitting your model, you can determine the unmeasured confounder needed to tip your analysis. This unmeasured confounder is determined by two quantities, the association between the exposure and the unmeasured confounder (if the unmeasured confounder is continuous, this is indicated with `mean_diff`, if binary, with `p1` and `p0`), and the association between the unmeasured confounder and outcome `gamma`. Using this 📦, we can fix one of these and solve for the other. Alternatively, we can fix both and solve for `n`, that is, how many unmeasured confounders of this magnitude would tip the analysis.

In this example, a model was fit and the exposure-outcome relationship was 1.5 (95% CI: 1.2, 1.8).

Continuous unmeasured confounder example
----------------------------------------

We are interested in a continuous unmeasured confounder, so we will use the `tip_with_continuous()` function.

Let's assume the relationship between the unmeasured confounder and outcome is 1.5 (`gamma = 1.5`), let's solve for the association between the exposure and unmeasured confounder needed to tip the analysis (in this case, we are solving for `mean_diff`, the mean difference needed between the exposed and unexposed).

``` r
tip_with_continuous(gamma = 1.5, lb = 1.2, ub = 1.8)
```

    ## [1] 0.4496603

A hypothetical unobserved continuous confounder that has an association of 1.5 with the outcome would need a scaled mean difference between exposure groups of 0.45 to tip this analysis at the 5% level, rendering it inconclusive.

Binary unmeasured confounder example
------------------------------------

Now we are interested in the binary unmeasured confounder, so we will use the `tip_with_binary()` function.

Let's assume the unmeasured confounder is prevalent in 25% of the exposed population (`p1 = 0.25`) and in 10% of the unexposed population (`p0 = 0.10`) -- let's solve for the association between the unmeasured confounder and the outcome needed to tip the analysis (`gamma`).

``` r
tip_with_binary(p1 = 0.25, p0 = 0.10, lb = 1.2, ub = 1.8)
```

    ## [1] 2.538462

A hypothetical unobserved binary confounder that is prevalent in 10% of the unexposed population and 25% of the exposed population would need to have an association with the outcome of 2.54 to tip this analysis at the 5% level, rendering it inconclusive.

Many unmeasured confounders
---------------------------

Suppose we are concerned that there are many small, independent, continuous, unmeasured confounders present.

``` r
tip_with_continuous(mean_diff = 0.25, gamma = 1.05, lb = 1.2, ub = 1.8)
```

    ## [1] 14.9474

It would take about 15 more independent unmeasured confounders with a scaled mean difference between exposure groups of 0.25 to and an association with the outcome of 1.05 tip the observed analysis at the 5% level, rendering it inconclusive.
