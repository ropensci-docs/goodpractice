# Positions of check failures in the source code

Note that not all checks refer to the source code. For these the result
will be `NULL`.

## Usage

``` r
failed_positions(gp)
```

## Arguments

- gp:

  [`gp`](https://docs.ropensci.org/goodpractice/reference/gp.md) output.

## Value

A list of lists of positions. See details below.

## Details

For the ones that do, the results is a list, one for each failure. Since
the same check can fail multiple times. A single failure is a list with
entries: `filename`, `line_number`, `column_number`, `ranges`. `ranges`
is a list of pairs of start and end positions for each line involved in
the check.

## Examples

``` r
path <- system.file("bad1", package = "goodpractice")
g <- gp(path, checks = "description_url")
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [8ms]
#> 
failed_positions(g)
#> $description_url
#> list()
#> 
```
