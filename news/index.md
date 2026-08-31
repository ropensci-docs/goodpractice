# Changelog

## goodpractice 1.1.00X (dev version)

### Major changes

- Require R version \>= 4.3, because of treesitter dependency
  ([\#302](https://github.com/ropensci-review-tools/goodpractice/issues/302);
  thanks to [@christian-million](https://github.com/christian-million))
- New
  [`use_skill_gp()`](https://docs.ropensci.org/goodpractice/reference/use_skill_gp.md)
  and
  [`learn_skill_gp()`](https://docs.ropensci.org/goodpractice/reference/learn_skill_gp.md)
  functions, plus a bundled `goodpractice4agents.md` skill in
  `inst/skills/`, giving AI agents instructions to fix issues flagged by
  `goodpractice`
  ([\#308](https://github.com/ropensci-review-tools/goodpractice/issues/308),
  [\#312](https://github.com/ropensci-review-tools/goodpractice/issues/312),
  [\#313](https://github.com/ropensci-review-tools/goodpractice/issues/313);
  thanks to [@mpadge](https://github.com/mpadge),
  [@drmowinckels](https://github.com/drmowinckels), and
  [@jonthegeek](https://github.com/jonthegeek)).

### Minor changes

- New GitHub issue templates for bug reports and feature requests
- `complexity_unused_internal` check no longer flags standard R package
  hook functions (`.onLoad`, `.onAttach`, `.onUnload`, `.onDetach`,
  `.Last.lib`) as unused
  ([\#319](https://github.com/ropensci-review-tools/goodpractice/issues/319);
  thanks [@TanguyBarthelemy](https://github.com/TanguyBarthelemy)).
- Fix `urlchecker` prep step to pass `fail = FALSE` to
  [`urlchecker::url_check()`](https://urlchecker.r-lib.org/reference/url_check.html),
  restoring previous default behaviour after an upstream change
  (r-lib/urlchecker#42).
- `.Rprofile` now sources the system-level `~/.Rprofile` if it exists,
  rather than entirely replacing it.
- Fix `test-integrity.R` to only read `.R` source files (avoiding
  installed `.rdb` binaries) and skip informatively when none are found.
- `tidyverse_r_file_names`: allow file names containing hyphens (`-`)
  but require file extension `.R`
  ([\#307](https://github.com/ropensci-review-tools/goodpractice/issues/307);
  thanks to [@JesseAlderliesten](https://github.com/JesseAlderliesten)).
- Fix bug in check group exclusions through env vars, and update
  vignette.
- Fix prep assignment to check groups for two checks (revdep + 1
  tidyverse), including attaching “namespace” prep only internally to
  one of those so full tidyverse prep stage is only run for that group.
- Fix bug in printing some check results that would produce an error
  caused by invalid marker types
  ([\#304](https://github.com/ropensci-review-tools/goodpractice/issues/304);
  thanks to [@JesseAlderliesten](https://github.com/JesseAlderliesten)).
- Convert the remaining base
  [`warning()`](https://rdrr.io/r/base/warning.html) in `ts_parse()` to
  [`cli::cli_warn()`](https://cli.r-lib.org/reference/cli_abort.html),
  completing the package-wide move to cli messaging.

------------------------------------------------------------------------

## goodpractice 1.1

### 1. Check control and output

#### 1.1 Check groups

- New
  [`all_check_groups()`](https://docs.ropensci.org/goodpractice/reference/all_check_groups.md)
  and
  [`checks_by_group()`](https://docs.ropensci.org/goodpractice/reference/checks_by_group.md)
  functions for discovering and selecting checks by category instead of
  individual names
  ([\#239](https://github.com/ropensci-review-tools/goodpractice/issues/239)).
- Every check now belongs to a named group. Use
  [`all_check_groups()`](https://docs.ropensci.org/goodpractice/reference/all_check_groups.md)
  to see the 16 available groups and `checks_by_group("description")` to
  list checks in a group.
- Added new
  [`describe_check_groups()`](https://docs.ropensci.org/goodpractice/reference/describe_check_groups.md)
  function, to produce group descriptions in console
  ([\#290](https://github.com/ropensci-review-tools/goodpractice/issues/290)).
- Added ‘groups’ param to [`print()`](https://rdrr.io/r/base/print.html)
  method, to enable printing only specified groups
  ([\#288](https://github.com/ropensci-review-tools/goodpractice/issues/288)).
- Exclude entire check groups via `goodpractice.exclude_check_groups`
  option or `GP_EXCLUDE_CHECK_GROUPS` environment variable.

#### 1.2 Default checks and check control

- New
  [`default_checks()`](https://docs.ropensci.org/goodpractice/reference/default_checks.md)
  and
  [`tidyverse_checks()`](https://docs.ropensci.org/goodpractice/reference/tidyverse_checks.md)
  helper functions.
- [`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md) now
  defaults to
  [`default_checks()`](https://docs.ropensci.org/goodpractice/reference/default_checks.md)
  instead of
  [`all_checks()`](https://docs.ropensci.org/goodpractice/reference/all_checks.md),
  keeping optional check sets out of the default run.
- New
  [`describe_check()`](https://docs.ropensci.org/goodpractice/reference/describe_check.md)
  function to print descriptions of all implemented checks
  ([@152](https://github.com/152))
- Exclude specific files from checks via `goodpractice.exclude_path`
  option or `GP_EXCLUDE_PATH` environment variable. Useful for generated
  code like `R/RcppExports.R`.

#### 1.3 Screen output

- goodpractice now uses cli, and no longer depends on crayon and
  clisymbols ([@olivroy](https://github.com/olivroy),
  [\#167](https://github.com/ropensci-review-tools/goodpractice/issues/167)).
- Check advice now uses cli inline markup throughout. All `gp` strings
  support `{.code}`, `{.fn}`, `{.pkg}`, `{.file}`, `{.field}`, and
  `{.url}` for consistent styling. Custom checks can use the same markup
  in their `gp` strings.
- If your editor supports it, goodpractice now prints clickable
  hyperlinks to console.
- `gp_advice()` gains a `type` parameter (`"error"`, `"info"`,
  `"warning"`) to control the output symbol and colour. Checks can
  return `list(status = TRUE, type = "info")` to display informational
  messages without a failure cross.

------------------------------------------------------------------------

### 2. treesitter

- R code inspection now uses treesitter for AST-based parsing instead of
  regex or
  [`getParseData()`](https://rdrr.io/r/utils/getParseData.html). Checks
  like `print_return_invisible`, `tidyverse_no_missing`, and
  `tidyverse_export_order` benefit from more robust and faster code
  analysis.
- Tree-sitter function detection now only considers assignment operators
  (`<-`, `=`, `<<-`), avoiding false matches on arithmetic expressions
  such as `x + function() 1`
  ([\#277](https://github.com/ropensci-review-tools/goodpractice/issues/277)).
- `ts_parse()` now honours the package’s declared `Encoding` when
  reading source files, preventing mojibake for packages using non-UTF-8
  encodings.
- New `duplicate_function_bodies` check: flags functions with identical
  bodies across files that should be consolidated into a shared helper
  ([\#232](https://github.com/ropensci-review-tools/goodpractice/issues/232)).

------------------------------------------------------------------------

### 3. New checks

#### 3.1 New linters

- Expanded default lintr checks from 9 to ~53, covering correctness
  (e.g. [`anyDuplicated()`](https://rdrr.io/r/base/duplicated.html) vs
  `any(duplicated())`), performance
  (e.g. [`colSums()`](https://rdrr.io/r/base/colSums.html) vs
  [`apply()`](https://rdrr.io/r/base/apply.html)), readability
  (e.g. [`switch()`](https://rdrr.io/r/base/switch.html) vs long if/else
  chains), and testthat best practices (e.g. `expect_identical()` vs
  `expect_equal()`). All respect `.lintr` configuration files
  ([\#189](https://github.com/ropensci-review-tools/goodpractice/issues/189)).
- New `lintr_installed_packages_linter` check: flags calls to
  [`installed.packages()`](https://rdrr.io/r/utils/installed.packages.html),
  which can be very slow and is rejected by CRAN. Use
  [`find.package()`](https://rdrr.io/r/base/find.package.html) or
  [`system.file()`](https://rdrr.io/r/base/system.file.html) instead
  ([\#278](https://github.com/ropensci-review-tools/goodpractice/issues/278)).
- New optional tidyverse style guide checks: 21 lintr-based checks plus
  2 structural checks (R file naming, test file mirroring). Opt in via
  `checks = c(default_checks(), tidyverse_checks())`.

#### 3.2 New DESCRIPTION checks

- `prep_description` defaults `Encoding` to `UTF-8` when absent, so
  downstream checks always have a concrete value. Unreadable files emit
  a warning instead of being silently skipped
  ([\#277](https://github.com/ropensci-review-tools/goodpractice/issues/277)).
- New DESCRIPTION checks
  ([\#122](https://github.com/ropensci-review-tools/goodpractice/issues/122),
  [\#85](https://github.com/ropensci-review-tools/goodpractice/issues/85)):
  - `description_not_start_with_package`: Description should not start
    with “This package”
  - `description_urls_in_angle_brackets`: URLs in Description must be
    wrapped in angle brackets
  - `description_doi_format`: DOIs should use `<doi:...>` not full URLs
  - `description_urls_not_http`: URLs should use https not http
  - `no_description_duplicate_deps`: No duplicate packages across
    dependency fields
  - `description_valid_roles`: <Authors@R> roles must be valid MARC
    relator codes
  - `description_pkgname_single_quoted`: Package names in
    Title/Description must be single-quoted

#### 3.3 Other new checks

- New `r_file_extension` check: flags R scripts using `.r` or `.q`
  instead of `.R`
  ([\#121](https://github.com/ropensci-review-tools/goodpractice/issues/121)).
- New `print_return_invisible` check: flags print methods that don’t
  return `invisible(x)`
  ([\#49](https://github.com/ropensci-review-tools/goodpractice/issues/49)).
- New `vignette_no_rm_list` check: flags `rm(list = ls())` in vignettes
  ([\#20](https://github.com/ropensci-review-tools/goodpractice/issues/20)).
- New `vignette_no_setwd` check: flags
  [`setwd()`](https://rdrr.io/r/base/getwd.html) in vignettes
  ([\#21](https://github.com/ropensci-review-tools/goodpractice/issues/21)).
- New `reverse_dependencies` check: queries CRAN for reverse
  dependencies and advises running `revdepcheck::revdep_check()` before
  submission.
- New `spelling` check: flags misspelled words in documentation via
  [`spelling::spell_check_package()`](https://docs.ropensci.org/spelling//reference/spell_check_package.html)
  ([\#84](https://github.com/ropensci-review-tools/goodpractice/issues/84)).
- New `has_readme` and `has_news` checks for package documentation
  completeness
  ([\#45](https://github.com/ropensci-review-tools/goodpractice/issues/45)).
- New roxygen2 checks: export/noRd tagging, unknown tags, and
  `@inheritParams`/`@inheritDotParams` validation
  ([\#197](https://github.com/ropensci-review-tools/goodpractice/issues/197)).

------------------------------------------------------------------------

### 4. Other updates

- Added `makefile`
  ([\#203](https://github.com/ropensci-review-tools/goodpractice/issues/203))
- Lowered default cyclomatic complexity limit from 50 to 15, aligning
  with lintr and pkgcheck defaults. Configurable via
  `goodpractice.cyclocomp_limit` option
  ([\#150](https://github.com/ropensci-review-tools/goodpractice/issues/150)).
- Preparation steps can now run in parallel via the `future.apply`
  package. Set `future::plan("multisession")` before calling
  [`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md) to
  enable parallel data gathering
  ([\#47](https://github.com/ropensci-review-tools/goodpractice/issues/47)).
- [`gp()`](https://docs.ropensci.org/goodpractice/reference/gp.md) now
  fails if the path provided to it is not a package (does not contain a
  DESCRIPTION file)
  ([\#190](https://github.com/ropensci-review-tools/goodpractice/issues/190),
  [@maelle](https://github.com/maelle))
- Prep step error handling refactored into `run_prep_step()` helper. New
  prep functions can use
  `run_prep_step(state, "name", function() { ... }, quiet)` instead of
  manually wrapping work in [`try()`](https://rdrr.io/r/base/try.html)
  and emitting warnings on failure.
- Removed `stringsAsFactors = FALSE` arguments throughout, relying on
  the R 4.0 default. Package now requires R \>= 4.0.0.

------------------------------------------------------------------------

## goodpractice 1.0.5

CRAN release: 2024-06-04

- New maintainer: rOpenSci
- Package reinstated on CRAN, after archiving of previous version.
- CRAN fixes - skipping failing test and adding to package Rd
- Adding docs.ropensci site to DESCRIPTION

## goodpractice 1.0.3

CRAN release: 2022-07-13

Additions:

- Limit for cyclomatic complexity check can be adjusted using the
  `goodpractice.cyclocomp.limit` option, default 50
  ([\#132](https://github.com/ropensci-review-tools/goodpractice/issues/132),
  [@fabian-s](https://github.com/fabian-s)).
- The number of lines printed to the console by each check result can be
  set using the new `positions_limit` parameter into
  [`print()`](https://rdrr.io/r/base/print.html) - previously it was
  always 5 lines
  ([\#130](https://github.com/ropensci-review-tools/goodpractice/issues/130),
  [@fabian-s](https://github.com/fabian-s)).
- GitHub Actions now used for CI/CD checks
  ([\#145](https://github.com/ropensci-review-tools/goodpractice/issues/145)),
  as well as to calculate code coverage with {covr} and build the
  package site with {pkgdown}.

Bugfixes:

- Documentation for custom checks significantly improved
  ([\#133](https://github.com/ropensci-review-tools/goodpractice/issues/133),
  [@fabian-s](https://github.com/fabian-s)).
- Year updated in `LICENSE`, and `LICENSE.md` added to clarify that
  {goodpractice} uses the MIT license
  ([\#144](https://github.com/ropensci-review-tools/goodpractice/issues/144)).

## goodpractice 1.0.2 (2018-06-14)

CRAN release: 2018-05-02

First CRAN release.

- added 2 vignettes
- added examples
- added tests
- added pkgdown site
- fixed check on library/require calls on windows
- wrapped prep steps in try

## goodpractice 1.0.0

First public release.
