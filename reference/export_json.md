# Export failed checks to JSON

Export failed checks to JSON

## Usage

``` r
export_json(gp, file, pretty = FALSE)
```

## Arguments

- gp:

  [`gp`](https://docs.ropensci.org/goodpractice/reference/gp.md) output.

- file:

  Output connection or file.

- pretty:

  Whether to pretty-print the JSON.

## Value

Invisibly returns the path to the output file.

## Examples

``` r
path <- system.file("bad1", package = "goodpractice")
g <- gp(path, checks = "description_url")
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [16ms]
#> 
tmp <- tempfile(fileext = ".json")
export_json(g, tmp)
unlink(tmp)
```
