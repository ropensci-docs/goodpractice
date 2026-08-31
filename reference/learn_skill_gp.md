# Function for AI agents to run and learn how to use the 'goodpractice' package.

You tell agents to directly use this function in order for them to learn
the skill. If you want to use the skill yourself to guide an agent, then
use the
[use_skill_gp](https://docs.ropensci.org/goodpractice/reference/use_skill_gp.md)
function.

## Usage

``` r
learn_skill_gp()
```

## Value

The content of the file
`system.file("skills", "goodpractice4agents.md", package = "goodpractice"))`

## Examples

``` r
if (FALSE) { # \dontrun{
learn_gp_skill()
} # }
```
