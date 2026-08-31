# List all checks performed

List all checks performed

## Usage

``` r
checks(gp)
```

## Arguments

- gp:

  [`gp`](https://docs.ropensci.org/goodpractice/reference/gp.md) output.

## Value

Character vector of check names.

## See also

Other API:
[`failed_checks()`](https://docs.ropensci.org/goodpractice/reference/failed_checks.md),
[`results()`](https://docs.ropensci.org/goodpractice/reference/results.md)

## Examples

``` r
path <- system.file("bad1", package = "goodpractice")
# Run a subset of all checks available
g <- gp(path, checks = all_checks()[9:16])
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [11ms]
#> 
checks(g)
#> [1] "no_description_depends"             "no_description_date"               
#> [3] "description_url"                    "description_not_start_with_package"
#> [5] "description_urls_in_angle_brackets" "description_doi_format"            
#> [7] "description_urls_not_http"          "no_description_duplicate_deps"     
# Or run with named check groups
g <- gp(path, checks = checks_by_group("description", "namespace"))
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [9ms]
#> 
#> ℹ Preparing: namespace
#> ✔ Preparing: namespace [7ms]
#> 
#> ℹ Preparing: rd
#> ✔ Preparing: rd [6ms]
#> 
#> ℹ Preparing: revdep
#> ✔ Preparing: revdep [1.2s]
#> 
checks(g)
#>  [1] "no_obsolete_deps"                   "complexity_unused_internal"        
#>  [3] "no_description_depends"             "no_description_date"               
#>  [5] "description_url"                    "description_not_start_with_package"
#>  [7] "description_urls_in_angle_brackets" "description_doi_format"            
#>  [9] "description_urls_not_http"          "no_description_duplicate_deps"     
#> [11] "description_valid_roles"            "description_pkgname_single_quoted" 
#> [13] "description_bugreports"             "no_import_package_as_a_whole"      
#> [15] "no_export_pattern"                  "rd_has_examples"                   
#> [17] "rd_has_return"                      "reverse_dependencies"              
```
