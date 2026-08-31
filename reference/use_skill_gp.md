# Download a local copy of 'goodpractice4agents.md' to tell AI agents how to use this package.

The 'goodpractice4agents.md' file contains sufficient instructions for
most AI agents to edit package code so that it passes most
'goodpractice' checks. The file can be directly edited and tweaked for
personal use cases. An agent can be instructed to simply "run the file
goodpractice4agents.md". Alternatively, the file can be directly
downloaded from GitHub at
<https://github.com/ropensci-review-tools/goodpractice/blob/main/inst/skills/goodpractice4agents.md>.

## Usage

``` r
use_skill_gp(
  path = "goodpractice4agents.md",
  use_skills_subdir = FALSE,
  overwrite = FALSE
)
```

## Arguments

- path:

  Local path where the file should be written. If `path` is an existing
  directory, the file is written as `goodpractice4agents.md` within it;
  otherwise `path` is treated as the full target file name.

- use_skills_subdir:

  If `TRUE`, the `path` will be edited to write the file in to a
  `skills/` sub-directory.

- overwrite:

  If `FALSE` (default) and file specified by `path` already exists,
  function will error and not overwrite the existing file.

## Value

(Invisibly) the full path to the written file.

## Details

You can also instruct agents to call the companion function,
[learn_skill_gp](https://docs.ropensci.org/goodpractice/reference/learn_skill_gp.md),
directly in order to learn and use the skill itself.

## Examples

``` r
path <- file.path(tempdir(), "skill.md")
f <- use_skill_gp(path)
file.exists(f)
#> [1] TRUE
unlink(f)
```
