Activity 6
================
Luke B Cadagin

## Data and packages

Again, we will load all of the `{tidyverse}` for this activity.

``` r
knitr::opts_chunk$set(error = TRUE)

library(tidyverse)
```

In this activity we will explore the differences between human-readable
datasets (untidy data) and computer-readable datasets (tidy data).
Before making any visualizations, it is important to understand whether
the data we have is tidy or not.

In this activity you will be working with data from the popular tv
sitcom Friends. Transcripts for all 10 seasons of the show were parsed
and all utterances are broken down by season, episode, and scene. The
`by_speaker` dataset summarizes the total number of words spoken by main
character for each season.

![FRIENDS tv show
logo](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Friends_logo.svg/1186px-Friends_logo.svg.png)

``` r
by_speaker <- read_csv(here::here("data","by_speaker.csv"))
```

In the `load_-_data` chunk, notice that I am using a new function: the
`here` function from `{here}` (typed as `here::here` or in
`<package>::<function>` format). This function is a another great tool
to use for sharing projects with others when their file structure might
be different than yours. Read Jenny Bryan’s [Ode to the here
package](https://github.com/jennybc/here_here) and/or read more about
`{here}`… [here](https://here.r-lib.org/articles/here.html). Also,
Martin Chan provides a guide of [RStudio Projects and Working
Directories](https://martinctc.github.io/blog/rstudio-projects-and-working-directories-a-beginner's-guide/)
and Jenny Richmond gives an overview on [How to use the `here`
package](http://jenrichmond.rbind.io/post/how-to-use-the-here-package/).

In short, folder structures vary from [computer-to-computer and
user-to-user](https://r4ds.had.co.nz/workflow-projects.html#paths-and-directories).
RStudio Projects are one great way to make sure that others that use
your code won’t have to worry about the path to files within the Project
folder. Then, `{here}` will always recognizes the top-level directory of
your RStudio project! Type `here::here()` in your **Console** to verify
that it is recognizing your current project is located at
`.../activity06-pivoting` (your front matter will be different than
mine, but it should end with this project folder).

![Illustration by Allison
Horst](https://raw.githubusercontent.com/allisonhorst/stats-illustrations/master/rstats-artwork/here.png)

Art by \[@allison\_horst\](<https://twitter.com/allison_horst>)

Now, why is `by_speaker` untidy data? How would this data be organized
if it were tidy? Do not use any of your new `{tidyr}` skills yet, only
describe it.

**Response**: The data frame `by_speaker` is untidy because each
variable does not have its own column (words and season do not have
their own columns). This is the number one aspect of tidy data.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Analysis

### Gather the… I mean `pivot_longer` the data

From before, I hope that it was evident that the variable *Season* is
spread across multiple columns and *Words* count is stored within the
cell values for these columns. Therefore, to “tidy-fy” these data we
need to reshape the information contained in columns `Season 1` through
`Season 10`. One way to do this is as follows:

``` r
friends_tidy <- by_speaker %>% 
  pivot_longer(
    cols = `Season 1`:`Season 10`,
    names_to = "Season",
    values_to = "Words"
  )

friends_tidy
```

    ## # A tibble: 60 x 3
    ##    speaker  Season    Words
    ##    <chr>    <chr>     <dbl>
    ##  1 Chandler Season 1   8796
    ##  2 Chandler Season 2   8248
    ##  3 Chandler Season 3   8952
    ##  4 Chandler Season 4   9695
    ##  5 Chandler Season 5   8611
    ##  6 Chandler Season 6  10764
    ##  7 Chandler Season 7   8712
    ##  8 Chandler Season 8   6262
    ##  9 Chandler Season 9  10980
    ## 10 Chandler Season 10  6589
    ## # … with 50 more rows

Notice that the columns for each season have a space in them:
“Season”SPACE“Number”. To be able to use column names with spaces, we
need to encapsulate them in backticks (the tickmark above your Tab key).
Also notice that I am directing the names of these old columns
(`Season 1` through `Season 10`) to a new column called `Season`. Since
I am creating this new column, I need to encapsulate the name in
quotation marks. Similarly with directing the values to a new `Words`
column.

There are a number of helper functions to help with selecting columns.

1.  Try reproducing the pivoted data in the `pivot_season_longer` code
    chunck using either the `contains` or `starts_with` helper functions
    from `{tidyselect}` (this is loaded with the `{tidyverse}` by
    default). Remember that you can view the help documentation by using
    the `?` method in your **Console**.
2.  Assign your reformatted dataset to `friends_tidy`, and
3.  Name your code chunk either `pivot_contains` or `pivot_ends`
    depending on which helper function you used.

``` r
friends_tidy <- by_speaker %>% 
  pivot_longer(
    cols = contains("season"),
    names_to = "season",
    values_to = "words"
  )

friends_tidy
```

    ## # A tibble: 60 x 3
    ##    speaker  season    words
    ##    <chr>    <chr>     <dbl>
    ##  1 Chandler Season 1   8796
    ##  2 Chandler Season 2   8248
    ##  3 Chandler Season 3   8952
    ##  4 Chandler Season 4   9695
    ##  5 Chandler Season 5   8611
    ##  6 Chandler Season 6  10764
    ##  7 Chandler Season 7   8712
    ##  8 Chandler Season 8   6262
    ##  9 Chandler Season 9  10980
    ## 10 Chandler Season 10  6589
    ## # … with 50 more rows

### Save the dataset

Now that we have this tidy dataset, we might want to share this with a
collaborator or use in alternative analysis or visualization tools. We
can do this by writing it to a file within our directory.

1.  Using the `write_csv` and `here::here` functions, write the
    `friends_tidy` dataset to the `data` folder.
2.  Call the file that is being created “`friends_tidy.csv`”.
3.  Name your code chunk `writing_file`

``` r
friends_tidy %>% 
write_csv(here::here("data", "friends_tidy.csv"))
```

Inspect that this file was created by looking in the `data` project
folder in the **Files** pane. You should see three files:
`by_speaker.csv`, `by_season.csv`, and `friends_tidy.csv`. Simply
verifying that it exists is sufficient.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Try it another way

While exploring the `data` project folder, there should have also been
an additional data files: `by_season.csv`. This is the same information
that we were just worked with, but structured in a different way.

1.  Write the code that reads in the data file.
2.  Assign this data file into meaningful objects.
3.  Name this code chunk `read_season_file`.

``` r
by_season <- read_csv(here::here("data", "by_season.csv"))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   season = col_character(),
    ##   Chandler = col_double(),
    ##   Joey = col_double(),
    ##   Monica = col_double(),
    ##   Phoebe = col_double(),
    ##   Rachel = col_double(),
    ##   Ross = col_double()
    ## )

``` r
by_season
```

    ## # A tibble: 10 x 7
    ##    season    Chandler  Joey Monica Phoebe Rachel  Ross
    ##    <chr>        <dbl> <dbl>  <dbl>  <dbl>  <dbl> <dbl>
    ##  1 Season 1      8796  6195   7851   6522   8854 10569
    ##  2 Season 2      8248  7207   7822   8713   8600  9382
    ##  3 Season 3      8952  8504   8301   9177   8809 10725
    ##  4 Season 4      9695  8541   7897   8560   9896  9178
    ##  5 Season 5      8611  9731   8703   8695  10352  9234
    ##  6 Season 6     10764 10923   8784   8372  11013 10307
    ##  7 Season 7      8712  9777   9397   7910  11813  8960
    ##  8 Season 8      6262 10695   8768   7688  11650 10295
    ##  9 Season 9     10980  9318   9400   9397   9915 10482
    ## 10 Season 10     6589  6886   6958   7420   7960  8289

Now,

1.  Pivot the `by_season` dataset so that it is now tidy,
2.  Assign it to an object called `speaker_tidy`, and
3.  Name the code chunk `pivot_speaker`.

``` r
speaker_tidy <- by_season %>% 
  pivot_longer(`Chandler`:`Ross`, names_to = "speaker", values_to = "words")

speaker_tidy
```

    ## # A tibble: 60 x 3
    ##    season   speaker  words
    ##    <chr>    <chr>    <dbl>
    ##  1 Season 1 Chandler  8796
    ##  2 Season 1 Joey      6195
    ##  3 Season 1 Monica    7851
    ##  4 Season 1 Phoebe    6522
    ##  5 Season 1 Rachel    8854
    ##  6 Season 1 Ross     10569
    ##  7 Season 2 Chandler  8248
    ##  8 Season 2 Joey      7207
    ##  9 Season 2 Monica    7822
    ## 10 Season 2 Phoebe    8713
    ## # … with 50 more rows

#### Total words spoken

Using `by_season` and your skills from past activities, write the code
that computes the total number of words spoken by each season and the
total words spoken across the series (challenge, can you display this
information in one table?). Name this code chunk `season_summary`.

``` r
by_season %>% 
  rowwise() %>% 
  mutate(total_words_by_season = sum(`Chandler`,`Joey`,`Monica`,`Phoebe`,`Rachel`,`Ross`)) %>% 
  ungroup() %>% 
  summarize(season, total_words_by_season, sum(total_words_by_season))
```

    ## # A tibble: 10 x 3
    ##    season    total_words_by_season `sum(total_words_by_season)`
    ##    <chr>                     <dbl>                        <dbl>
    ##  1 Season 1                  48787                       538004
    ##  2 Season 2                  49972                       538004
    ##  3 Season 3                  54468                       538004
    ##  4 Season 4                  53767                       538004
    ##  5 Season 5                  55326                       538004
    ##  6 Season 6                  60163                       538004
    ##  7 Season 7                  56569                       538004
    ##  8 Season 8                  55358                       538004
    ##  9 Season 9                  59492                       538004
    ## 10 Season 10                 44102                       538004

Using `by_speaker` and your skills from past activities, write the code
that computes the total number of words spoken by each season and the
total words spoken across the series (challenge, can you display this
information in one table?). Name this code chunk `speaker_summary`.

``` r
by_speaker %>% 
  summarize(
    season_1_word_total = sum(`Season 1`),
    season_2_word_total = sum(`Season 2`),
    season_3_word_total = sum(`Season 3`),
    season_4_word_total = sum(`Season 4`),
    season_5_word_total = sum(`Season 5`),
    season_6_word_total = sum(`Season 6`),
    season_7_word_total = sum(`Season 7`),
    season_8_word_total = sum(`Season 8`),
    season_9_word_total = sum(`Season 9`),
    season_10_word_total = sum(`Season 10`),
    series_word_total = sum(season_1_word_total, season_2_word_total, season_3_word_total, season_4_word_total, season_5_word_total, season_6_word_total, season_7_word_total, season_8_word_total, season_9_word_total, season_10_word_total)
  ) %>% 
  pivot_longer(
    cols = contains("word"),
    names_to = "season",
    values_to = "total",
  )
```

    ## # A tibble: 11 x 2
    ##    season                total
    ##    <chr>                 <dbl>
    ##  1 season_1_word_total   48787
    ##  2 season_2_word_total   49972
    ##  3 season_3_word_total   54468
    ##  4 season_4_word_total   53767
    ##  5 season_5_word_total   55326
    ##  6 season_6_word_total   60163
    ##  7 season_7_word_total   56569
    ##  8 season_8_word_total   55358
    ##  9 season_9_word_total   59492
    ## 10 season_10_word_total  44102
    ## 11 series_word_total    538004

Using the `friends_tidy` object and your skills from past activities,
write the code that computes the total number of words spoken by each
season and the total words spoken across the series (challenge, can you
display this information in one table?). Call this code chunk
`tidy_summary`

``` r
 test <- friends_tidy %>% 
  group_by(season) %>% 
  summarize(total_words_by_season = sum(words))
  #summarize(season, total_words_by_season, total_words_by_series = sum(total_words_by_season))
  
total <- friends_tidy %>% 
  summarize(total_words_by_season = sum(words))

test2 <- tibble(season = "total", total)

bind_rows(test, test2)
```

    ## # A tibble: 11 x 2
    ##    season    total_words_by_season
    ##    <chr>                     <dbl>
    ##  1 Season 1                  48787
    ##  2 Season 10                 44102
    ##  3 Season 2                  49972
    ##  4 Season 3                  54468
    ##  5 Season 4                  53767
    ##  6 Season 5                  55326
    ##  7 Season 6                  60163
    ##  8 Season 7                  56569
    ##  9 Season 8                  55358
    ## 10 Season 9                  59492
    ## 11 total                    538004

How does the code for `friends_tidy` compare to the two `by_` code?
Reflect on the process of writing this code and on the code itself.
Which is easier to write? Which is easier to read?

**Response**: The `friends_tidy` code was much easier to write because I
could quickly group my data by season. The `friends_tidy` code is much
easier to read as it is less code and easier to follow.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

#### Preparing tables

So far, we have taken a variable spread across multiple variables (e.g.,
word counts for each character) and pivoted so that we have tidy data
where,

1.  Each observation is its own row,
2.  Each variable is its own column, and
3.  Each value has its own cell.

Now we will use `pivot_wider` to “untidy-fy” data. You may be wondering,
“but you just said untidy data is bad…” However, like the tables that we
started off with, untidy data is useful at the end of an analysis to
create human-readable tables. Also, sometimes the analysis tool/function
that we want to use needs data organized in this format.

The code below takes our tidy dataset, then creates one variable for
each *Season* (like we started with).

``` r
friends_tidy %>% 
  pivot_wider(names_from = season, values_from = words)
```

    ## # A tibble: 6 x 11
    ##   speaker  `Season 1` `Season 2` `Season 3` `Season 4` `Season 5` `Season 6`
    ##   <chr>         <dbl>      <dbl>      <dbl>      <dbl>      <dbl>      <dbl>
    ## 1 Chandler       8796       8248       8952       9695       8611      10764
    ## 2 Joey           6195       7207       8504       8541       9731      10923
    ## 3 Monica         7851       7822       8301       7897       8703       8784
    ## 4 Phoebe         6522       8713       9177       8560       8695       8372
    ## 5 Rachel         8854       8600       8809       9896      10352      11013
    ## 6 Ross          10569       9382      10725       9178       9234      10307
    ## # … with 4 more variables: Season 7 <dbl>, Season 8 <dbl>, Season 9 <dbl>,
    ## #   Season 10 <dbl>

Write the code to take the tidy dataset and create one variable for each
*speaker*. Call this code chunk `speaker_wide`.

``` r
speaker_wide <- friends_tidy %>% 
  pivot_wider(names_from = speaker, values_from = words)
```

![](README-img/noun_pause.png) **Final Planned Pause Point**: If you
have any questions, contact your instructor. Otherwise feel free to
continue on.

### Dressing up tables

Explore the
[`knitr::kable`](https://bookdown.org/yihui/rmarkdown-cookbook/kable.html)
function and [`{kableExtra}`](https://haozhu233.github.io/kableExtra/).

A note before installing new packages: The HPC support team has [created
a
guide](https://hpcsupport.atlassian.net/servicedesk/customer/portal/3/article/562429973)
to help you avoid issues when working within the RStudio Server. The
information here is useful to read **prior** to experiencing issues and
I encourage you to read it now (1-2 minutes).

Use `knitr::kable` and `{kableExtra}` to dress up the `speaker_wide`
table.

``` r
speaker_wide %>% 
  knitr::kable()
```

| season    | Chandler |  Joey | Monica | Phoebe | Rachel |  Ross |
|:----------|---------:|------:|-------:|-------:|-------:|------:|
| Season 1  |     8796 |  6195 |   7851 |   6522 |   8854 | 10569 |
| Season 2  |     8248 |  7207 |   7822 |   8713 |   8600 |  9382 |
| Season 3  |     8952 |  8504 |   8301 |   9177 |   8809 | 10725 |
| Season 4  |     9695 |  8541 |   7897 |   8560 |   9896 |  9178 |
| Season 5  |     8611 |  9731 |   8703 |   8695 |  10352 |  9234 |
| Season 6  |    10764 | 10923 |   8784 |   8372 |  11013 | 10307 |
| Season 7  |     8712 |  9777 |   9397 |   7910 |  11813 |  8960 |
| Season 8  |     6262 | 10695 |   8768 |   7688 |  11650 | 10295 |
| Season 9  |    10980 |  9318 |   9400 |   9397 |   9915 | 10482 |
| Season 10 |     6589 |  6886 |   6958 |   7420 |   7960 |  8289 |

### The Curb Cut Effect

> When you design well for disability, you design well for everybody
> else.

[Gary Karp on “The Curb Cut
Effect”](https://www.youtube.com/watch?v=iuIkFRZKfCU)

The curb cut is the ramp section of a side walk:

![Illustration of curb cuts by Jono
Hey](https://images.prismic.io/sketchplanations/f103d71e-0ecb-4e78-bc02-e6dce18d0ed8_SP+718+-+The+curb-cut+effect.png?auto=format&ixlib=react-9.0.3&h=1887.9781420765028&w=1600&q=75&dpr=1)

Summary tables are great ways to present information. However, there is
a big disparity in what sighted users see and what a screen reader goes
through for default tables.

Here is a short video (4:44) that showcases how screen readers interpret
elements on a webpage:

![Screen reader demo for digital accessibility by UC San
Fran](https://www.youtube.com/watch?v=dEbl5jvLKGQ)

**Update** your table in the previous section to add a *table caption*.

## Challenge: One more `pivot_wider`

While you used `pivot_wider` in this activity to make data “untidy”,
that is not the only time this function is useful. Sometimes multiple
variable names are stored in one column. Data from the government
usually has this issue. For example, see:

``` r
tidyr::us_rent_income
```

    ## # A tibble: 104 x 5
    ##    GEOID NAME       variable estimate   moe
    ##    <chr> <chr>      <chr>       <dbl> <dbl>
    ##  1 01    Alabama    income      24476   136
    ##  2 01    Alabama    rent          747     3
    ##  3 02    Alaska     income      32940   508
    ##  4 02    Alaska     rent         1200    13
    ##  5 04    Arizona    income      27517   148
    ##  6 04    Arizona    rent          972     4
    ##  7 05    Arkansas   income      23789   165
    ##  8 05    Arkansas   rent          709     5
    ##  9 06    California income      29454   109
    ## 10 06    California rent         1358     3
    ## # … with 94 more rows

1.  Restructure `us_rent_income` to have the following column names:

-   `GEOID`

-   `NAME`

-   `income_estimate`

-   `income_moe`

-   `rent_estimate`

-   `rent_moe`

    Hint: There are some additional arguments in `pivot_wider` that can
    assist with this. Look at the **Examples** in the help
    documentation.

2.  Assign this to `us_wide`

``` r
us_wide <- us_rent_income %>% 
  pivot_wider(names_from = variable, names_glue = "{variable}_{.value}", values_from = c(estimate, moe))

us_wide
```

    ## # A tibble: 52 x 6
    ##    GEOID NAME                 income_estimate rent_estimate income_moe rent_moe
    ##    <chr> <chr>                          <dbl>         <dbl>      <dbl>    <dbl>
    ##  1 01    Alabama                        24476           747        136        3
    ##  2 02    Alaska                         32940          1200        508       13
    ##  3 04    Arizona                        27517           972        148        4
    ##  4 05    Arkansas                       23789           709        165        5
    ##  5 06    California                     29454          1358        109        3
    ##  6 08    Colorado                       32401          1125        109        5
    ##  7 09    Connecticut                    35326          1123        195        5
    ##  8 10    Delaware                       31560          1076        247       10
    ##  9 11    District of Columbia           43198          1424        681       17
    ## 10 12    Florida                        25952          1077         70        3
    ## # … with 42 more rows

Now,

1.  Re-restructure so that you have the original column names of:

-   `GEOID`

-   `NAME`

-   `variable`

-   `estimate`

-   `moe`

    Hint: There are some additional arguments in `pivot_longer` that can
    assist with this. Look at the **Examples** in the help
    documentation.

2.  Assign this to `us_long`

``` r
us_long <- us_wide %>% 
  pivot_longer(3:6, names_to = c("variable", "estimate", "moe"), values_to = )
```

    ## Error: If you supply multiple names in `names_to` you must also supply one of `names_sep` or `names_pattern`.

## Attribution

This Activity is inspired by materials from Jenny Bryan’s [lotr
tidy](https://github.com/jennybc/lotr-tidy) repository.
