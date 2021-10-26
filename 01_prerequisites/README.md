
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

Tips:

-   Have a dedicated folder for your projects.
-   Launch recent projects with: Alt + F &gt; J
-   Launch projects with a dedicated app (Alfred, Albert, etc.)

[Practice safe paths](https://rstats.wtf/safe-paths.html):

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

[How to name files](https://rstats.wtf/how-to-name-files.html):

<img src=https://i.imgur.com/Lo7JFxc.png width=700>

<https://speakerdeck.com/jennybc/how-to-name-files?slide=3>
