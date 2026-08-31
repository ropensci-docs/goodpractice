# List the names of tidyverse style checks

These checks are optional and not included in the default set. They are
powered by [`lint_package`](https://lintr.r-lib.org/reference/lint.html)
using lintr's default linter set and respect any `.lintr` configuration
file in the package root (e.g. to disable specific linters or add
exclusions). Add them via
`checks = c(default_checks(), tidyverse_checks())`.

## Usage

``` r
tidyverse_checks()
```

## Value

Character vector of tidyverse check names

## Examples

``` r
tidyverse_checks()
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
