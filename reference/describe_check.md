# Describe one or more checks

Describe one or more checks

## Usage

``` r
describe_check(check_name = NULL)
```

## Arguments

- check_name:

  Names of checks to be described.

## Value

List of character descriptions for each `check_name`

## See also

Other check_groups:
[`all_check_groups()`](https://docs.ropensci.org/goodpractice/reference/all_check_groups.md),
[`all_checks()`](https://docs.ropensci.org/goodpractice/reference/all_checks.md),
[`checks_by_group()`](https://docs.ropensci.org/goodpractice/reference/checks_by_group.md),
[`default_checks()`](https://docs.ropensci.org/goodpractice/reference/default_checks.md),
[`describe_check_groups()`](https://docs.ropensci.org/goodpractice/reference/describe_check_groups.md),
[`tidyverse_checks()`](https://docs.ropensci.org/goodpractice/reference/tidyverse_checks.md)

## Examples

``` r
describe_check("rcmdcheck_non_portable_makevars")
#> $rcmdcheck_non_portable_makevars
#> [1] "Check for non-portable Makevars flags"
#> 
check_name <- c("no_description_depends",
                "lintr_assignment_linter",
                "no_import_package_as_a_whole",
                "rcmdcheck_missing_docs")
describe_check(check_name)
#> $no_description_depends
#> [1] "No \"Depends\" in DESCRIPTION"
#> 
#> $lintr_assignment_linter
#> [1] "'<-' and not '=' is used for assignment"
#> 
#> $no_import_package_as_a_whole
#> [1] "Packages are not imported as a whole"
#> 
#> $rcmdcheck_missing_docs
#> [1] "Check for undocumented exported objects"
#> 
# Or to see all checks:
if (FALSE) { # \dontrun{
  describe_check(all_checks())
} # }
```
