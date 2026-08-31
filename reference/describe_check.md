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
