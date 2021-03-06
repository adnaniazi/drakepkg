
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Project Status: WIP – Initial development is in progress, but there
has not yet been a stable, usable release suitable for the
public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip)

# drakepkg

The goal of `drakepkg` is to demonstrate how a
[`drake`](https://ropensci.github.io/drake/) workflow can be organized
as an R package.

This package is a work-in-progress that began with a request for
guidance: [`drake` issue
\#471](https://github.com/ropensci/drake/issues/471)

The following table shows how each feature of a
[`drake`](https://ropensci.github.io/drake/) workflow is made accessible
within an R
package:

| `drake`                   | R Package                                                                                                     |
| :------------------------ | :------------------------------------------------------------------------------------------------------------ |
| plans, commands           | functions (`R/*.R`)                                                                                           |
| targets                   | stored in the cache (`.drake/`)                                                                               |
| input files, output files | internal data (`inst/intdata/*`), external data (`inst/extdata/*`), images and documents (`inst/documents/*`) |

## Installation

You can install the released version of `drakepkg` from its Github
[repository](https://github.com/tiernanmartin/drakepkg) with:

``` r
devtools::install_packages("tiernanmartin/drakepkg")
```

## Usage

The package comes with two example
[`drake`](https://ropensci.github.io/drake/) plans that are loosely
based on the
[`main`](https://github.com/ropensci/drake/tree/master/inst/examples/main)
example included in the [`drake`](https://ropensci.github.io/drake/)
package.

The first plan is a simple one:

``` r
library(drakepkg)
#> Loading required package: drake
#> Warning: package 'drake' was built under R version 3.5.3

get_example_plan_simple()
#> # A tibble: 5 x 2
#>   target     command                                                       
#>   <chr>      <expr>                                                        
#> 1 raw_data   readxl::read_excel(file_in("intdata/iris-internal.xlsx"))    ~
#> 2 ready_data dplyr::mutate(raw_data, Species = forcats::fct_inorder(Specie~
#> 3 hist       create_plot(ready_data)                                      ~
#> 4 fit        lm(Sepal.Width ~ Petal.Width + Species, ready_data)          ~
#> 5 report     write_report_simple(hist, fit)                               ~
```

Several commands used in the plan (e.g,`create_plot()`,
`write_report_simple()`) are included as part of the `drakepkg` R
package and so is the plan itself; the documentation for each of these
functions can be accessed using R’s `help()` function (for example,
`help(get_example_plan_simple)`).

You can reproduce the simple plan’s workflow by performing the following
steps:

1.  Copy the package’s directories and source code files into your
    working directory with the `copy_drakepkg_files()` function
2.  View the plan (`get_example_plan_simple()`) and then make it
    (`make(get_example_plan_simple())`)
3.  Access the plan’s targets using `drake` functions like `readd()` or
    `loadd()`
4.  View the html documents created by the workflow in the `documents/`
    directory

<!-- end list -->

``` r
# Step 1: copy the source code files into the working directory

copy_drakepkg_files()
```

``` r
# Step 2A: view the example plan

get_example_plan_simple()
#> # A tibble: 5 x 2
#>   target     command                                                       
#>   <chr>      <expr>                                                        
#> 1 raw_data   readxl::read_excel(file_in("intdata/iris-internal.xlsx"))    ~
#> 2 ready_data dplyr::mutate(raw_data, Species = forcats::fct_inorder(Specie~
#> 3 hist       create_plot(ready_data)                                      ~
#> 4 fit        lm(Sepal.Width ~ Petal.Width + Species, ready_data)          ~
#> 5 report     write_report_simple(hist, fit)                               ~
```

``` r
# Step 2B: make the example plan

make(get_example_plan_simple())
#> All targets are already up to date.
```

``` r
# Step 3: examine the plan's targets

readd(fit)
#> 
#> Call:
#> lm(formula = Sepal.Width ~ Petal.Width + Species, data = ready_data)
#> 
#> Coefficients:
#>       (Intercept)        Petal.Width  Speciesversicolor  
#>             3.236              0.781             -1.501  
#>  Speciesvirginica  
#>            -1.844

readd(hist)
```

<img src="man/figures/README-step4-1.png" width="100%" />
