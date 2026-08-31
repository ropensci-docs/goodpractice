# Return all check results in a data frame

Return all check results in a data frame

## Usage

``` r
results(gp)
```

## Arguments

- gp:

  [`gp`](https://docs.ropensci.org/goodpractice/reference/gp.md) output.

## Value

Data frame, with columns:

- check:

  The name of the check.

- passed:

  Logical, whether the check passed.

## See also

Other API:
[`checks()`](https://docs.ropensci.org/goodpractice/reference/checks.md),
[`failed_checks()`](https://docs.ropensci.org/goodpractice/reference/failed_checks.md)

## Examples

``` r
path <- system.file("bad1", package = "goodpractice")
# Run a subset of all checks available
g <- gp(path, checks = all_checks()[9:16])
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [9ms]
#> 
results(g)
#>                                check passed
#> 1             no_description_depends  FALSE
#> 2                no_description_date  FALSE
#> 3                    description_url  FALSE
#> 4 description_not_start_with_package   TRUE
#> 5 description_urls_in_angle_brackets   TRUE
#> 6             description_doi_format   TRUE
#> 7          description_urls_not_http   TRUE
#> 8      no_description_duplicate_deps   TRUE
# Or run with named check groups
g <- gp(path, checks = checks_by_group("description", "namespace"))
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [9ms]
#> 
#> ℹ Preparing: namespace
#> ✔ Preparing: namespace [6ms]
#> 
#> ℹ Preparing: rd
#> ✔ Preparing: rd [11ms]
#> 
#> ℹ Preparing: revdep
#> ✔ Preparing: revdep [180ms]
#> 
results(g)
#>                                 check passed
#> 1                    no_obsolete_deps   TRUE
#> 2          complexity_unused_internal  FALSE
#> 3              no_description_depends  FALSE
#> 4                 no_description_date  FALSE
#> 5                     description_url  FALSE
#> 6  description_not_start_with_package   TRUE
#> 7  description_urls_in_angle_brackets   TRUE
#> 8              description_doi_format   TRUE
#> 9           description_urls_not_http   TRUE
#> 10      no_description_duplicate_deps   TRUE
#> 11            description_valid_roles     NA
#> 12  description_pkgname_single_quoted   TRUE
#> 13             description_bugreports  FALSE
#> 14       no_import_package_as_a_whole  FALSE
#> 15                  no_export_pattern   TRUE
#> 16                    rd_has_examples     NA
#> 17                      rd_has_return     NA
#> 18               reverse_dependencies   TRUE
```
