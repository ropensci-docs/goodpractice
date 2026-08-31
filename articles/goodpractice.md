# goodpractice

## Why goodpractice?

A robust R package needs more than passing `R CMD check`. Tests give you
confidence that things work. A consistent coding style makes your code
readable to others. Keeping functions short and focused reduces the risk
of bugs. Good metadata — a BugReports URL, properly declared
dependencies, documented return values — helps users and reviewers
understand what they’re working with.

goodpractice checks all of this in one pass. It bundles `R CMD check`
with code coverage ([covr](https://cran.r-project.org/package=covr)),
source linting ([lintr](https://cran.r-project.org/package=lintr)),
cyclomatic complexity
([cyclocomp](https://cran.r-project.org/package=cyclocomp)), and its own
checks for documentation, package structure, and common pitfalls. Each
message tells you *what* to fix, *why* it matters, and *where* in your
code to look. You can also add [custom
checks](https://docs.ropensci.org/goodpractice/articles/custom_checks.md)
for your team’s own conventions.

## Quick start

The main function is
[`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md) (short
for
[`goodpractice()`](https://docs.ropensci.org/goodpractice/reference/gp.md)).
Point it at a package directory and it runs the default set of checks:

``` r

library(goodpractice)

# goodpractice ships with an example package that has some issues
pkg_path <- system.file("bad1", package = "goodpractice")
```

``` r

g <- gp(pkg_path)
```

    #> ── Preparing goodpractice for badpackage ───────────────────────────────────────
    #> ℹ Preparing: description
    #> ✔ Preparing: description [18ms]
    #> 
    #> ℹ Preparing: code_structure
    #> ✔ Preparing: code_structure [29ms]
    #> 
    #> ℹ Preparing: namespace
    #> ✔ Preparing: namespace [9ms]
    #> 
    #> ℹ Preparing: covr
    #> ✔ Preparing: covr [1.5s]
    #> 
    #> ℹ Preparing: cyclocomp
    #> ✔ Preparing: cyclocomp [5.9s]
    #> 
    #> ℹ Preparing: package_structure
    #> ✔ Preparing: package_structure [6ms]
    #> 
    #> ℹ Preparing: lintr
    #> ✔ Preparing: lintr [165ms]
    #> 
    #> ℹ Preparing: rcmdcheck
    #> ✔ Preparing: rcmdcheck [5.8s]
    #> 
    #> ℹ Preparing: rd
    #> ✔ Preparing: rd [7ms]
    #> 
    #> ℹ Preparing: revdep
    #> ✔ Preparing: revdep [349ms]
    #> 
    #> ℹ Preparing: roxygen2
    #> ✔ Preparing: roxygen2 [42ms]
    #> 
    #> ℹ Preparing: spelling
    #> ✔ Preparing: spelling [6ms]
    #> 
    #> ℹ Preparing: urlchecker
    #> ℹ Package badpackage
    #> ℹ Preparing: urlcheckerℹ Checking that VignetteBuilder package knitr is installed.
    #> ℹ Preparing: urlchecker✔ VignetteBuilder package knitr is installed.
    #> ℹ Preparing: urlchecker✔ Preparing: urlchecker [289ms]
    #> 
    #> ℹ Preparing: vignette
    #> ✔ Preparing: vignette [8ms]

Printing the result shows only the checks that failed:

``` r

g
#> ── It is good practice to ──────────────────────────────────────────────────────
#> 
#> ✖ remove or use internal functions that are defined but never called. Dead code
#>   increases maintenance burden.
#> 
#>     R/semicolons.R:2
#>     R/tf.R:2
#>     R/tf.R:9
#>     R/tf2.R:2
#> 
#> ✖ not use Depends in DESCRIPTION, as it can cause name clashes, and poor
#>   interaction with other packages. Use Imports instead.
#> 
#> ✖ omit the Date field in DESCRIPTION. It is not required and it gets invalid
#>   quite often. A build date will be added to the package when you perform `R
#>   CMD build` on it.
#> 
#> ✖ add a URL field to DESCRIPTION. It helps users find information about your
#>   package online. If your package does not have a homepage, add an URL to
#>   GitHub, or the CRAN package page.
#> 
#> ✖ add a BugReports field to DESCRIPTION, and point it to a bug tracker. Many
#>   online code hosting services provide bug trackers for free,
#>   <https://github.com>, <https://gitlab.com>, etc.
#> 
#> ✖ add a README.md (or README.Rmd) file to the top-level directory. A good
#>   README describes what the package does, how to install it, and includes a
#>   short example.
#> 
#> ✖ add a NEWS.md file to track user-visible changes between releases. See
#>   <https://style.tidyverse.org/news.html> for formatting guidance.
#> 
#> ✖ use the .R file extension for R scripts, not .r or .q. CRAN requires the
#>   uppercase .R extension.
#> 
#>     R/bad_extension.r
#> 
#> ✖ omit trailing semicolons from code lines. They are not needed and most R
#>   coding standards forbid them
#> 
#>     R/semicolons.R:4:30
#>     R/semicolons.R:5:29
#>     R/semicolons.R:9:38
#> 
#> ✖ avoid `x == TRUE` or `x == FALSE`. Use `x` or `!x` directly. The comparison
#>   is redundant and less readable.
#> 
#>     R/tf.R:3:7
#> 
#> ✖ not import packages as a whole, as this can cause name clashes between the
#>   imported packages, especially over time as packages change. Instead, import
#>   only the specific functions you need.
#> 
#> ✖ fix this R CMD check ERROR: VignetteBuilder package not declared: ‘knitr’ See
#>   section ‘The DESCRIPTION file’ in the ‘Writing R Extensions’ manual.
#> 
#> ────────────────────────────────────────────────────────────────────────────────
```

Each line starting with a cross is one failed check. The text after the
cross explains what to fix and why. The indented file paths below it
show exactly where in your code the problem was found — in terminals and
IDEs that support it, these paths are clickable. If every check passes,
you get a short praise message instead.

## What gp() actually does

When you call
[`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md), two
things happen in sequence:

1.  **Gather data** — goodpractice runs a set of preparation steps, one
    per check group. One step runs `R CMD check`. Another computes code
    coverage by installing your package and exercising the tests.
    Another lints your source files. Each step runs exactly once and
    stores its results for the checks to use.

2.  **Run checks** — each check reads from the stored results and
    returns pass, fail, or skip. A single preparation step can feed many
    checks — the `rcmdcheck` step alone powers over 200 individual
    checks, all drawn from a single `R CMD check` run.

This two-step design is why goodpractice can run 250+ checks without
repeating expensive work. It also means you can skip an entire category
of checks by excluding its group — more on that below.

## Choosing what to run

By default,
[`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md) runs
everything in
[`default_checks()`](https://docs.ropensci.org/goodpractice/reference/default_checks.md)
— about 250 checks covering documentation, code style, test coverage,
namespace hygiene, and CRAN compliance:

``` r

length(default_checks())
#> [1] 307
```

If you only need a specific check, pass its name to the `checks`
argument. You can find check names with
[`all_checks()`](https://docs.ropensci.org/goodpractice/reference/all_checks.md)
and filter with [`grep()`](https://rdrr.io/r/base/grep.html):

``` r

# find checks related to URLs
grep("url", all_checks(), value = TRUE)
#> [1] "description_url"                    "description_urls_in_angle_brackets"
#> [3] "description_urls_not_http"          "urlchecker_ok"                     
#> [5] "urlchecker_no_redirects"
```

Then run just the ones you care about:

``` r

g_url <- gp(pkg_path, checks = "description_url")
#> ── Preparing goodpractice for badpackage ───────────────────────────────────────
#> ℹ Preparing: description
#> ✔ Preparing: description [9ms]
#> 
g_url
#> ── It is good practice to ──────────────────────────────────────────────────────
#> 
#> ✖ add a URL field to DESCRIPTION. It helps users find information about your
#>   package online. If your package does not have a homepage, add an URL to
#>   GitHub, or the CRAN package page.
#> 
#> ────────────────────────────────────────────────────────────────────────────────
```

To go the other direction and run *more* than the defaults, combine
check sets. For example, to add the optional tidyverse style checks on
top of the defaults:

``` r

g <- gp(".", checks = c(default_checks(), tidyverse_checks()))
```

Three helper functions give you the available check names:

- [`default_checks()`](https://docs.ropensci.org/goodpractice/reference/default_checks.md)
  — the standard set, run by default
- [`tidyverse_checks()`](https://docs.ropensci.org/goodpractice/reference/tidyverse_checks.md)
  — opt-in style checks following [tidyverse
  conventions](https://style.tidyverse.org/)
- [`all_checks()`](https://docs.ropensci.org/goodpractice/reference/all_checks.md)
  — both combined

``` r

length(tidyverse_checks())
#> [1] 30
length(all_checks())
#> [1] 337
```

## Check groups

Every check belongs to at least one *check group*. Groups let you work
with categories of checks instead of individual names:

``` r

all_check_groups()
#>  [1] "covr"              "cyclocomp"         "description"      
#>  [4] "lintr"             "namespace"         "rcmdcheck"        
#>  [7] "rd"                "revdep"            "roxygen2"         
#> [10] "code_structure"    "package_structure" "spelling"         
#> [13] "tidyverse"         "urlchecker"        "vignette"
```

To see which checks belong to a group, use
[`checks_by_group()`](https://docs.ropensci.org/goodpractice/reference/checks_by_group.md):

``` r

checks_by_group("description")
#>  [1] "no_obsolete_deps"                   "no_description_depends"            
#>  [3] "no_description_date"                "description_url"                   
#>  [5] "description_not_start_with_package" "description_urls_in_angle_brackets"
#>  [7] "description_doi_format"             "description_urls_not_http"         
#>  [9] "no_description_duplicate_deps"      "description_valid_roles"           
#> [11] "description_pkgname_single_quoted"  "description_bugreports"            
#> [13] "reverse_dependencies"
```

You can select multiple groups at once:

``` r

# run only DESCRIPTION and namespace checks
gp(".", checks = checks_by_group("description", "namespace"))
```

There is also a
[`describe_check_groups()`](https://docs.ropensci.org/goodpractice/reference/describe_check_groups.md)
function to return full descriptions of all checks (modified here from
default screen output to look nice):

``` r

describe_check_groups()
```

| Group | Description |
|:---|:---|
| covr | Test coverage report from ‘covr’ package. |
| cyclocomp | Function cyclocomplexity with the ‘cyclocomp’ package; default limit of 50. |
| description | Check common issues with DESCRIPTION files, including formatting issues with URLs, DOIs, BugReports, package names, and author and contributor roles |
| lintr | Check package linting with the ‘lintr’ package (84 linters in total). |
| namespace | Check import and export patterns in NAMESPACE file |
| rcmdcheck | Run ‘R CMD check’ via the ‘rcmdcheck’ package. ~200 checks for documentation, namespace, compilation, tests, vignettes, CRAN compliance. |
| rd | Check whether or not ’man/\*.Rd’ function documentation includes both example code and return values (regardless of whether or not documentation files are generated by ‘Roxygen2’). |
| revdep | Check whether package has reverse dependencies, and recommending running ‘reddev’ package if so. |
| roxygen2 | Only for packages within use ‘Roxygen2’ to generate documentation. Checks for best practices in Roxygen2 tag usage, flags any unknown tags, and ensures ‘inheritParams’ is used correctly. |
| code_structure | Common issues like duplicated or unused function bodies, that ‘print()’ returns insivibly, that ‘on.exit()’ uses ‘add = TRUE’, and checks on function length (default max50 lines). |
| package_structure | Generic checks like whether a package has a README, a NEWS file, or whether all files use a ‘.R’ extension, and not ‘.r’. |
| spelling | Check spelling with the ‘spelling’ package. |
| tidyverse | Check compliance with the Tidyverse style guide; mostly via ‘lintr’ package. (These checks are not run by default; and only if ‘tidyverse_checks()’ are added to ‘checks_by_group()’. |
| urlchecker | Check whether all URLs are valid, includingidentifying any redirects. |
| vignette | Check that vignette code does not use either ‘rm()’ or ‘setwd()’. |

## Excluding check groups

The checks themselves run in milliseconds — what takes time is the data
gathering. Computing code coverage with `covr` requires installing your
package and running all your tests. Running `R CMD check` via
`rcmdcheck` exercises documentation, examples, vignettes, and
compilation. On a large package, these two steps alone can take several
minutes.

If you only care about code style or DESCRIPTION metadata right now, you
can skip the slow groups entirely. Set the
`goodpractice.exclude_check_groups` option to a character vector of
group names:

``` r

# skip coverage and R CMD check — just run the fast checks
options(goodpractice.exclude_check_groups = c("covr", "rcmdcheck"))
gp(".")
```

Every check that depends on a skipped group is automatically excluded
too.

For CI/CD pipelines, you can exclude check groups by specifying them in
comma-separate format via the `GP_EXCLUDE_CHECK_GROUPS` environment
variable:

``` bash
GP_EXCLUDE_CHECK_GROUPS='covr,lintr,rcmdcheck'
```

When both the R option and the environment variable are set, the R
option wins. Exclusion only applies when you use the default check set —
if you explicitly pass check names to the `checks` argument, exclusion
settings are ignored and those checks always run.

``` r

options(goodpractice.exclude_check_groups = "covr")

# covr checks are excluded here (using defaults)
gp(".")

# the "covr" check runs here because we asked for it by name
gp(".", checks = "covr")
```

## Excluding files

If your package contains generated code or vendored files that should
not be checked, you can exclude specific files. Set the
`goodpractice.exclude_path` option to a character vector of paths
relative to the package root:

``` r

options(goodpractice.exclude_path = c("R/RcppExports.R", "R/generated_bindings.R"))
gp(".")
```

Or via the `GP_EXCLUDE_PATH` environment variable:

``` bash
GP_EXCLUDE_PATH=R/RcppExports.R,R/generated.R Rscript -e 'goodpractice::gp(".")'
```

Excluded files are skipped by lintr, treesitter, expression, and
roxygen2 checks.

## Parallel preparation

Preparation steps run sequentially by default. If you have the
[future.apply](https://cran.r-project.org/package=future.apply) package
installed, you can run them in parallel by setting a
[`future::plan()`](https://future.futureverse.org/reference/plan.html):

``` r

future::plan("multisession")
gp(".")
```

This can significantly speed up runs on large packages where multiple
slow preps (covr, rcmdcheck, lintr) would otherwise run one after
another. Preps run in parallel only when a non-sequential plan is active
— the default behaviour is unchanged.

## Tidyverse style checks

The default checks deliberately stay away from style preferences — they
focus on things that are good practice regardless of how you format your
code.

If you or your team follows the [tidyverse style
guide](https://style.tidyverse.org/), you can opt into an additional set
of style checks:

``` r

# add tidyverse checks to the defaults
gp(".", checks = c(default_checks(), tidyverse_checks()))

# or run only the tidyverse checks
gp(".", checks = tidyverse_checks())
```

Most of these are powered by
[lintr](https://cran.r-project.org/package=lintr) — brace placement,
spacing, naming conventions, and so on. They run
[`lintr::lint_package()`](https://lintr.r-lib.org/reference/lint.html)
once and share the results across all lintr-based checks. If your
package has a `.lintr` configuration file, those settings are respected:
disabled linters stay disabled, exclusions are honoured.

A few tidyverse checks go beyond linting and look at package structure
directly — whether R file names use snake_case, whether test files
mirror source files, whether functions use
[`missing()`](https://rdrr.io/r/base/missing.html) where `NULL` defaults
would be clearer, and whether exported functions appear before internal
helpers.

All tidyverse checks belong to the `tidyverse` group:

``` r

checks_by_group("tidyverse")
#>  [1] "tidyverse_brace_linter"                    
#>  [2] "tidyverse_commas_linter"                   
#>  [3] "tidyverse_commented_code_linter"           
#>  [4] "tidyverse_equals_na_linter"                
#>  [5] "tidyverse_function_left_parentheses_linter"
#>  [6] "tidyverse_indentation_linter"              
#>  [7] "tidyverse_infix_spaces_linter"             
#>  [8] "tidyverse_object_length_linter"            
#>  [9] "tidyverse_object_name_linter"              
#> [10] "tidyverse_object_usage_linter"             
#> [11] "tidyverse_paren_body_linter"               
#> [12] "tidyverse_pipe_consistency_linter"         
#> [13] "tidyverse_pipe_continuation_linter"        
#> [14] "tidyverse_quotes_linter"                   
#> [15] "tidyverse_return_linter"                   
#> [16] "tidyverse_spaces_inside_linter"            
#> [17] "tidyverse_spaces_left_parentheses_linter"  
#> [18] "tidyverse_trailing_blank_lines_linter"     
#> [19] "tidyverse_trailing_whitespace_linter"      
#> [20] "tidyverse_vector_logic_linter"             
#> [21] "tidyverse_whitespace_linter"               
#> [22] "tidyverse_assignment_linter"               
#> [23] "tidyverse_line_length_linter"              
#> [24] "tidyverse_semicolon_linter"                
#> [25] "tidyverse_seq_linter"                      
#> [26] "tidyverse_T_and_F_symbol_linter"           
#> [27] "tidyverse_r_file_names"                    
#> [28] "tidyverse_test_file_names"                 
#> [29] "tidyverse_no_missing"                      
#> [30] "tidyverse_export_order"
```

## Working with results

Beyond printing, the object returned by
[`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md) gives
you programmatic access to the results.

[`checks()`](https://docs.ropensci.org/goodpractice/reference/checks.md)
returns the names of all checks that were run:

``` r

checks(g_url)
#> [1] "description_url"
```

[`failed_checks()`](https://docs.ropensci.org/goodpractice/reference/failed_checks.md)
returns only the names of the checks that failed:

``` r

failed_checks(g)
#>  [1] "complexity_unused_internal"            
#>  [2] "no_description_depends"                
#>  [3] "no_description_date"                   
#>  [4] "description_url"                       
#>  [5] "description_bugreports"                
#>  [6] "has_readme"                            
#>  [7] "has_news"                              
#>  [8] "r_file_extension"                      
#>  [9] "lintr_semicolon_linter"                
#> [10] "lintr_redundant_equals_linter"         
#> [11] "no_import_package_as_a_whole"          
#> [12] "rcmdcheck_package_dependencies_present"
```

[`results()`](https://docs.ropensci.org/goodpractice/reference/results.md)
gives you a data frame with one row per check and a `passed` column that
is `TRUE`, `FALSE`, or `NA` (skipped):

``` r

results(g)[1L:5L, ]
#>                        check passed
#> 1           no_obsolete_deps   TRUE
#> 2     print_return_invisible   TRUE
#> 3            on_exit_has_add   TRUE
#> 4 complexity_function_length   TRUE
#> 5 complexity_unused_internal  FALSE
```

A check shows `NA` when its preparation step failed or was excluded — it
was not evaluated, so it neither passed nor failed.

To see all file positions for failed checks (not just the first five
that [`print()`](https://rdrr.io/r/base/print.html) shows), use
[`print()`](https://rdrr.io/r/base/print.html) with a higher limit:

``` r

print(g, positions_limit = Inf)
```

You can also export the full results to JSON for use in other tools:

``` r

export_json(g, "gp_results.json")
```

## Custom checks

goodpractice is extensible — you can define your own checks and
preparation steps to enforce team-specific conventions. The `gp` string
in each check supports [cli inline
markup](https://cli.r-lib.org/reference/inline-markup.html) — use
`{.fn func}` for function names, `{.code expression}` for code,
`{.file path}` for file paths, `{.field name}` for field names, and
`{.url url}` for URLs.

See
[`vignette("custom_checks")`](https://docs.ropensci.org/goodpractice/articles/custom_checks.md)
for the full guide.
