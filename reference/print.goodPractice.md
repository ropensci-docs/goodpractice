# Print goodpractice results

Print goodpractice results

## Usage

``` r
# S3 method for class 'goodPractice'
print(x, groups = NULL, positions_limit = 5, ...)
```

## Arguments

- x:

  Object of class `goodPractice`, as returned by
  [`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md).

- groups:

  Name of check groups for which to print results, as vector of one or
  more of
  [`all_check_groups()`](https://docs.ropensci.org/goodpractice/reference/all_check_groups.md).

- positions_limit:

  How many positions to print at most.

- ...:

  Unused, for compatibility with
  [`base::print()`](https://rdrr.io/r/base/print.html) generic method.
