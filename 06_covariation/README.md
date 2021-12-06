
### Setup

``` r
library(tidyverse)
#> ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
#> ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
#> ✓ tibble  3.1.6     ✓ dplyr   1.0.7
#> ✓ tidyr   1.1.4     ✓ stringr 1.4.0
#> ✓ readr   2.1.1     ✓ forcats 0.5.1
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
```

# Covariation

-   The tendency for two or more variables to vary together in a related
    way.

-   To spot covariation, visualise the relationship between the
    variables.

-   How you do it depends on the type of variables you have:

    -   A categorical variable and a continuous variable (today).
    -   Two categorical variables.
    -   Two continuous variables.

### A categorical and continuous variable.

``` r
diamonds %>% 
  select(price, cut)
#> # A tibble: 53,940 × 2
#>    price cut      
#>    <int> <ord>    
#>  1   326 Ideal    
#>  2   326 Premium  
#>  3   327 Good     
#>  4   334 Premium  
#>  5   335 Good     
#>  6   336 Very Good
#>  7   336 Very Good
#>  8   337 Very Good
#>  9   337 Fair     
#> 10   338 Very Good
#> # … with 53,930 more rows
```

#### Freaquency plot

This isn’t very useful.

``` r
ggplot(data = diamonds) + 
  geom_freqpoly(mapping = aes(x = price, colour = cut))
```

Same

``` r
ggplot(diamonds) + 
  geom_freqpoly(aes(x = price, colour = cut))
#> `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> The variation
of count across values of cut is too much. To make the comparison easier
we need to standardize the valyes of `y`. Let’s plot density: The count
standardize so that the area under each curve is one.

``` r
ggplot(diamonds) + 
  geom_freqpoly(aes(x = price, y = ..density.., colour = cut))
#> `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Surprisingly, fair diamonds (the lowest quality) seem to have the
highest average price!

#### Box plot

![](eda-boxplot.png)

``` r
ggplot(diamonds, aes(x = cut, y = price)) +
  geom_boxplot()
```

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

-   The boxplots are compact so we can more easily compare them.

-   Are better quality diamonds cheaper on average?

–

-   `cut` is an “ordered” factor.

``` r
diamonds %>% 
  distinct(cut) %>% 
  pull()
#> [1] Ideal     Premium   Good      Very Good Fair     
#> Levels: Fair < Good < Very Good < Premium < Ideal
```

-   In the `mpg` dataset, the variable `class` isn’t ordered.

``` r
mpg %>% 
  distinct(class) %>% 
  pull()
#> [1] "compact"    "midsize"    "suv"        "2seater"    "minivan"   
#> [6] "pickup"     "subcompact"
```

-   The trend is hard to see on how highway mileage (`hwy`) varies
    across classes:

``` r
class_hwy <- aes(
  x = class,
  y = hwy
)
class_hwy
#> Aesthetic mapping: 
#> * `x` -> `class`
#> * `y` -> `hwy`
```

``` r
ggplot(mpg) +
  geom_boxplot(class_hwy)
```

![](README_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

`reorder()` helps you order values of a categorical variable like `cut`.

``` r
class_reordered_by_median_hwy <- aes(
  x = reorder(class, hwy, FUN = median),
  y = hwy
)
class_reordered_by_median_hwy
#> Aesthetic mapping: 
#> * `x` -> `reorder(class, hwy, FUN = median)`
#> * `y` -> `hwy`
```

``` r
p <- ggplot(mpg) +
  geom_boxplot(class_reordered_by_median_hwy)
p
```

![](README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

If you have long variable names you may better flip the plot.

``` r
p + coord_flip()
```

![](README_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
