# goodpractice

## Advice on R Package Building

Give advice about good practices when building R packages. Advice
includes functions and syntax to avoid, package structure, code
complexity, code formatting, etc.

## Installation

You can install the release version from CRAN:

``` r

install.packages("goodpractice")
```

The development version can be installed from R-universe by enabling the
[ropensci-review-tools
`r-universe`](https://ropensci-review-tools.r-universe.dev/) and
installing the usual way:

``` r

options(repos = c(
    ropenscireviewtools = "https://ropensci-review-tools.r-universe.dev",
    CRAN = "https://cloud.r-project.org"
))
install.packages("goodpractice")
```

Or the development version can be installed directly from GitHub with:

``` r

pak::pak("ropensci-review-tools/goodpractice")
```

## Usage

``` r

library(goodpractice)
gp("<my-package>")
```

## Example

``` r

library(goodpractice)
# use example package contained in the goodpractice package
pkg_path <- system.file("bad1", package = "goodpractice")
g <- gp(pkg_path)
g
```

``` R
#> ── GP badpackage ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
#> 
#> It is good practice to
#> 
#>   ✖ not use "Depends" in DESCRIPTION, as it can cause name clashes, and poor interaction with other packages. Use "Imports" instead.
#>   ✖ omit "Date" in DESCRIPTION. It is not required and it gets invalid quite often. A build date will be added to the package when you perform `R CMD build` on it.
#>   ✖ add a "URL" field to DESCRIPTION. It helps users find information about your package online. If your package does not have a homepage, add an URL to GitHub, or the CRAN package package page.
#>   ✖ add a "BugReports" field to DESCRIPTION, and point it to a bug tracker. Many online code hosting services provide bug trackers for free, https://github.com, https://gitlab.com, etc.
#>   ✖ add a README.md (or README.Rmd) file to the top-level directory. A good README describes what the package does, how to install it, and includes a short example.
#>   ✖ add a NEWS.md file to track user-visible changes between releases. See <https://style.tidyverse.org/news.html> for formatting guidance.
#>   ✖ use the .R file extension for R scripts, not .r or .q. CRAN requires the uppercase .R extension.
#> 
#>     'R/bad_extension.r'
#> 
#>   ✖ omit trailing semicolons from code lines. They are not needed and most R coding standards forbid them
#> 
#>     'R/semicolons.R:4:30'
#>     'R/semicolons.R:5:29'
#>     'R/semicolons.R:9:38'
#> 
#>   ✖ not import packages as a whole, as this can cause name clashes between the imported packages, especially over time as packages change. Instead, import only the specific functions you need.
#>   ✖ fix this R CMD check ERROR: VignetteBuilder package not declared: ‘knitr’ See section ‘The DESCRIPTION file’ in the ‘Writing R Extensions’ manual.
#> ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

``` r

# show all available checks
# all_checks()

# run only a specific check
g_url <- gp(pkg_path, checks = "description_url")
g_url
```

``` R
#> ── GP badpackage ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
#> 
#> It is good practice to
#> 
#>   ✖ add a "URL" field to DESCRIPTION. It helps users find information about your package online. If your package does not have a homepage, add an URL to GitHub, or the CRAN package package page.
#> ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

``` r

# which checks were carried out?
checks(g_url)
```

``` R
#> [1] "description_url"
```

``` r

# which checks failed?
failed_checks(g)
```

``` R
#>  [1] "no_description_depends"                
#>  [2] "no_description_date"                   
#>  [3] "description_url"                       
#>  [4] "description_bugreports"                
#>  [5] "has_readme"                            
#>  [6] "has_news"                              
#>  [7] "r_file_extension"                      
#>  [8] "lintr_semicolon_linter"                
#>  [9] "no_import_package_as_a_whole"          
#> [10] "rcmdcheck_package_dependencies_present"
```

``` r

# show the first 5 checks carried out and their results
results(g)[1:5,]
```

``` R
#>                    check passed
#> 1                   covr     NA
#> 2              cyclocomp   TRUE
#> 3 no_description_depends  FALSE
#> 4    no_description_date  FALSE
#> 5        description_url  FALSE
```

## Contributing

We welcome any and all contributions to this package. See
[CONTRIBUTING.md](https://docs.ropensci.org/goodpractice/CONTRIBUTING.html)
for details.

## License

MIT © 2022 Ascent Digital Services UK Limited

## Contributors

All contributions to this project are gratefully acknowledged using the
[`allcontributors` package](https://github.com/ropensci/allcontributors)
following the [allcontributors](https://allcontributors.org)
specification. Contributions of any kind are welcome!

### Code

[TABLE]

### Issue Authors

[TABLE]

### Issue Contributors

[TABLE]
