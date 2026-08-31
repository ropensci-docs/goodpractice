# Names of the failed checks

Names of the failed checks

## Usage

``` r
failed_checks(gp)
```

## Arguments

- gp:

  [`gp`](https://docs.ropensci.org/goodpractice/reference/gp.md) output.

## Value

Names of the failed checks.

## See also

Other API:
[`checks()`](https://docs.ropensci.org/goodpractice/reference/checks.md),
[`results()`](https://docs.ropensci.org/goodpractice/reference/results.md)

## Examples

``` r
path <- system.file("bad1", package = "goodpractice")
# run a subset of all checks available
g <- gp(path, checks = all_checks()[9:16])
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [8ms]
#> 
failed_checks(g)
#> [1] "no_description_depends" "no_description_date"    "description_url"       
# Or run with named check groups
g <- gp(path, checks = checks_by_group("description", "namespace"))
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [14ms]
#> 
#> ℹ Preparing: namespace
#> ✔ Preparing: namespace [6ms]
#> 
#> ℹ Preparing: rd
#> ✔ Preparing: rd [6ms]
#> 
#> ℹ Preparing: revdep
#> ✔ Preparing: revdep [187ms]
#> 
failed_checks(g)
#> [1] "complexity_unused_internal"   "no_description_depends"      
#> [3] "no_description_date"          "description_url"             
#> [5] "description_bugreports"       "no_import_package_as_a_whole"
```
