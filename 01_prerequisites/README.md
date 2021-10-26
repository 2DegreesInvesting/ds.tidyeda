
### Defensive workflow

(Find RStudio commands with: Shift + Ctrl/Cmd + P)

[Start from a blank slate every time you restart
R](https://rstats.wtf/save-source.html):

-   Save workspace on quit: Never.
-   Load workspace on start: Off.
-   Restart R session.

[Use RStudio
projects](https://rstats.wtf/project-oriented-workflow.html#rstudio-projects):

-   Create a new project.
-   Open project.
-   File &gt; Recent projects

[Practice safe paths](https://rstats.wtf/safe-paths.html) and [name
files defensively](https://rstats.wtf/how-to-name-files.html):

``` r
# Good
path <- here::here("data", "greeting.txt")
path
#> [1] "/home/mauro/git/ds.tidyeda/data/greeting.txt"
readLines(path)
#> [1] "Hello world"

# Bad
path <- "/a/fragile path/that/only/i/have/data/greeting.txt"
readLines(path)
#> Warning in file(con, "r"): cannot open file '/a/fragile path/that/only/i/have/
#> data/greeting.txt': No such file or directory
#> Error in file(con, "r"): cannot open the connection
```

### The data science workflow

<img src=https://i.imgur.com/MitpHrJ.png width=700>

<https://r4ds.had.co.nz/introduction.html>

### The data science toolkit

<img src=https://i.imgur.com/i61n3xb.png width=700>

<https://www.tidyverse.org/>

``` r
library(tidyverse)
#> ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
#> ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
#> ✓ tibble  3.1.5     ✓ dplyr   1.0.7
#> ✓ tidyr   1.1.4     ✓ stringr 1.4.0
#> ✓ readr   2.0.2     ✓ forcats 0.5.1
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
```

### Visualize: The ggplot template

<img src=https://i.imgur.com/WsoXgV2.png width=700>

Template:

        ggplot(data = <DATA>) +
          <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))

Example:

``` r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut))
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

<https://ggplot2.tidyverse.org/>

### Transform: Key dplyr verbs and how to compose them

Example:

``` r
diamonds
#> # A tibble: 53,940 × 10
#>    carat cut       color clarity depth table price     x     y     z
#>    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
#>  1  0.23 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
#>  2  0.21 Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
#>  3  0.23 Good      E     VS1      56.9    65   327  4.05  4.07  2.31
#>  4  0.29 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
#>  5  0.31 Good      J     SI2      63.3    58   335  4.34  4.35  2.75
#>  6  0.24 Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
#>  7  0.24 Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
#>  8  0.26 Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
#>  9  0.22 Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
#> 10  0.23 Very Good H     VS1      59.4    61   338  4     4.05  2.39
#> # … with 53,930 more rows
```

``` r
select(diamonds, cut, price)
#> # A tibble: 53,940 × 2
#>    cut       price
#>    <ord>     <int>
#>  1 Ideal       326
#>  2 Premium     326
#>  3 Good        327
#>  4 Premium     334
#>  5 Good        335
#>  6 Very Good   336
#>  7 Very Good   336
#>  8 Very Good   337
#>  9 Fair        337
#> 10 Very Good   338
#> # … with 53,930 more rows
```

Same:

``` r
# Same
diamonds %>% select(cut, price)
```

``` r
diamonds %>% 
  select(cut, price) %>% 
  count(cut) %>% 
  filter(n > 10000)
#> # A tibble: 3 × 2
#>   cut           n
#>   <ord>     <int>
#> 1 Very Good 12082
#> 2 Premium   13791
#> 3 Ideal     21551
```

Same but less readable:

``` r
filter(count(select(diamonds, cut, price), cut), n > 10000)
```

<https://dplyr.tidyverse.org/>

### Tidy data: What and why

> Tidy datasets are easy to manipulate, model and visualise, and have a
> specific structure: each variable is a column, each observation is a
> row.

<img src=https://i.imgur.com/nBC5Rk9.png width=700>

<https://vita.had.co.nz/papers/tidy-data.pdf>
