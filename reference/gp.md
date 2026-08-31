# Run good practice checks

To see the results, just print it to the screen.

## Usage

``` r
gp(
  path = ".",
  checks = default_checks(),
  extra_preps = NULL,
  extra_checks = NULL,
  quiet = TRUE
)
```

## Arguments

- path:

  Path to a package root.

- checks:

  Character vector, the checks to run. Defaults to
  [`default_checks`](https://docs.ropensci.org/goodpractice/reference/default_checks.md).
  Use
  [`all_checks`](https://docs.ropensci.org/goodpractice/reference/all_checks.md)
  to list all checks, or add optional sets like
  [`tidyverse_checks`](https://docs.ropensci.org/goodpractice/reference/tidyverse_checks.md).
  When `NULL`, all registered checks are run, subject to any exclusions
  from `goodpractice.exclude_check_groups` or `GP_EXCLUDE_CHECK_GROUPS`.

- extra_preps:

  Custom preparation functions. See
  [`make_prep`](https://docs.ropensci.org/goodpractice/reference/customization.md)
  on creating preparation functions.

- extra_checks:

  Custom checks. See
  [`make_check`](https://docs.ropensci.org/goodpractice/reference/customization.md)
  on creating checks.

- quiet:

  Whether to suppress output from the preparation functions. Note that
  not all preparation functions produce output, even if this option is
  set to `FALSE`.

## Value

A goodpractice object that you can query with a simple API. See
[`results`](https://docs.ropensci.org/goodpractice/reference/results.md)
to start.

## Excluding check groups

When using the default `checks = all_checks()`, entire groups of checks
can be excluded by group name via the
`goodpractice.exclude_check_groups` option or the
`GP_EXCLUDE_CHECK_GROUPS` environment variable (comma-separated). The
option takes precedence.


    # Skip URL and coverage checks:
    options(goodpractice.exclude_check_groups = c("urlchecker", "covr"))

    # Or via environment variable:
    Sys.setenv(GP_EXCLUDE_CHECK_GROUPS = "urlchecker,covr")

Exclusion only applies when `checks = NULL` (the default). Explicit
`checks` arguments are never filtered.

## Excluding files

Specific files can be excluded from checks via the
`goodpractice.exclude_path` option or the `GP_EXCLUDE_PATH` environment
variable (comma-separated). Paths are relative to the package root.


    options(goodpractice.exclude_path = c("R/RcppExports.R", "R/generated.R"))

    # Or via environment variable:
    Sys.setenv(GP_EXCLUDE_PATH = "R/RcppExports.R,R/generated.R")

Excluded files are skipped by lintr, treesitter, expression, and
roxygen2 checks.

## Parallel preparation

Preparation steps run sequentially by default. To run them in parallel,
install future.apply and set a
[`plan`](https://future.futureverse.org/reference/plan.html):


    future::plan("multisession")
    gp(".")

Preps run in parallel only when a non-sequential plan is active. Prep
functions must be independent: in parallel mode each prep receives the
initial state snapshot, so a prep cannot read another prep's output.
Only new state fields are merged back; if two preps write the same
field, the second is dropped with a warning.

## Examples

``` r
path <- system.file("bad1", package = "goodpractice")
# Run a subset of all checks available
g <- gp(path, checks = all_checks()[9:16])
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [9ms]
#> 
g
#> ── It is good practice to ──────────────────────────────────────────────────────
#> 
#> ✖ not use Depends in DESCRIPTION, as it can cause name clashes, and poor
#>   interaction with other packages. Use Imports instead.
#> 
#> 
#> ✖ omit the Date field in DESCRIPTION. It is not required and it gets invalid
#>   quite often. A build date will be added to the package when you perform `R
#>   CMD build` on it.
#> 
#> 
#> ✖ add a URL field to DESCRIPTION. It helps users find information about your
#>   package online. If your package does not have a homepage, add an URL to
#>   GitHub, or the CRAN package page.
#> 
#> 
#> ────────────────────────────────────────────────────────────────────────────────
# Or run with named check groups
g <- gp(path, checks = checks_by_group("description", "namespace"))
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [8ms]
#> 
#> ℹ Preparing: namespace
#> ✔ Preparing: namespace [6ms]
#> 
#> ℹ Preparing: rd
#> ✔ Preparing: rd [6ms]
#> 
#> ℹ Preparing: revdep
#> ✔ Preparing: revdep [177ms]
#> 
```
