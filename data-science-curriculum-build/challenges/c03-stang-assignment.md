Aluminum Data
================
Maya McKone-Sweet
2025-2-20

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [Loading and Wrangle](#loading-and-wrangle)
  - [**q1** Tidy `df_stang` to produce `df_stang_long`. You should have
    column names `thick, alloy, angle, E, nu`. Make sure the `angle`
    variable is of correct type. Filter out any invalid
    values.](#q1-tidy-df_stang-to-produce-df_stang_long-you-should-have-column-names-thick-alloy-angle-e-nu-make-sure-the-angle-variable-is-of-correct-type-filter-out-any-invalid-values)
- [EDA](#eda)
  - [Initial checks](#initial-checks)
    - [**q2** Perform a basic EDA on the aluminum data *without
      visualization*. Use your analysis to answer the questions under
      *observations* below. In addition, add your own *specific*
      question that you’d like to answer about the data—you’ll answer it
      below in
      q3.](#q2-perform-a-basic-eda-on-the-aluminum-data-without-visualization-use-your-analysis-to-answer-the-questions-under-observations-below-in-addition-add-your-own-specific-question-that-youd-like-to-answer-about-the-datayoull-answer-it-below-in-q3)
  - [Visualize](#visualize)
    - [**q3** Create a visualization to investigate your question from
      q2 above. Can you find an answer to your question using the
      dataset? Would you need additional information to answer your
      question?](#q3-create-a-visualization-to-investigate-your-question-from-q2-above-can-you-find-an-answer-to-your-question-using-the-dataset-would-you-need-additional-information-to-answer-your-question)
    - [**q4** Consider the following
      statement:](#q4-consider-the-following-statement)
- [References](#references)

*Purpose*: When designing structures such as bridges, boats, and planes,
the design team needs data about *material properties*. Often when we
engineers first learn about material properties through coursework, we
talk about abstract ideas and look up values in tables without ever
looking at the data that gave rise to published properties. In this
challenge you’ll study an aluminum alloy dataset: Studying these data
will give you a better sense of the challenges underlying published
material values.

In this challenge, you will load a real dataset, wrangle it into tidy
form, and perform EDA to learn more about the data.

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

*Background*: In 1946, scientists at the Bureau of Standards tested a
number of Aluminum plates to determine their
[elasticity](https://en.wikipedia.org/wiki/Elastic_modulus) and
[Poisson’s ratio](https://en.wikipedia.org/wiki/Poisson%27s_ratio).
These are key quantities used in the design of structural members, such
as aircraft skin under [buckling
loads](https://en.wikipedia.org/wiki/Buckling). These scientists tested
plats of various thicknesses, and at different angles with respect to
the [rolling](https://en.wikipedia.org/wiki/Rolling_(metalworking))
direction.

# Loading and Wrangle

<!-- -------------------------------------------------- -->

The `readr` package in the Tidyverse contains functions to load data
form many sources. The `read_csv()` function will help us load the data
for this challenge.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/stang.csv"

## Load the data
df_stang <- read_csv(filename)
```

    ## Rows: 9 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): alloy
    ## dbl (7): thick, E_00, nu_00, E_45, nu_45, E_90, nu_90
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_stang
```

    ## # A tibble: 9 × 8
    ##   thick  E_00 nu_00  E_45  nu_45  E_90 nu_90 alloy  
    ##   <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl> <chr>  
    ## 1 0.022 10600 0.321 10700  0.329 10500 0.31  al_24st
    ## 2 0.022 10600 0.323 10500  0.331 10700 0.323 al_24st
    ## 3 0.032 10400 0.329 10400  0.318 10300 0.322 al_24st
    ## 4 0.032 10300 0.319 10500  0.326 10400 0.33  al_24st
    ## 5 0.064 10500 0.323 10400  0.331 10400 0.327 al_24st
    ## 6 0.064 10700 0.328 10500  0.328 10500 0.32  al_24st
    ## 7 0.081 10000 0.315 10000  0.32   9900 0.314 al_24st
    ## 8 0.081 10100 0.312  9900  0.312 10000 0.316 al_24st
    ## 9 0.081 10000 0.311    -1 -1      9900 0.314 al_24st

Note that these data are not tidy! The data in this form are convenient
for reporting in a table, but are not ideal for analysis.

### **q1** Tidy `df_stang` to produce `df_stang_long`. You should have column names `thick, alloy, angle, E, nu`. Make sure the `angle` variable is of correct type. Filter out any invalid values.

*Hint*: You can reshape in one `pivot` using the `".value"` special
value for `names_to`.

``` r
## TASK: Tidy `df_stang`
df_stang_long <-
  df_stang %>% 
  pivot_longer(
    names_to = c(".value","angle"),
    names_sep = "_",
    cols = c(-thick, -alloy)
  ) %>% 
  filter(E > 0) %>% 
  transform(angle = as.integer(angle))
  
df_stang_long
```

    ##    thick   alloy angle     E    nu
    ## 1  0.022 al_24st     0 10600 0.321
    ## 2  0.022 al_24st    45 10700 0.329
    ## 3  0.022 al_24st    90 10500 0.310
    ## 4  0.022 al_24st     0 10600 0.323
    ## 5  0.022 al_24st    45 10500 0.331
    ## 6  0.022 al_24st    90 10700 0.323
    ## 7  0.032 al_24st     0 10400 0.329
    ## 8  0.032 al_24st    45 10400 0.318
    ## 9  0.032 al_24st    90 10300 0.322
    ## 10 0.032 al_24st     0 10300 0.319
    ## 11 0.032 al_24st    45 10500 0.326
    ## 12 0.032 al_24st    90 10400 0.330
    ## 13 0.064 al_24st     0 10500 0.323
    ## 14 0.064 al_24st    45 10400 0.331
    ## 15 0.064 al_24st    90 10400 0.327
    ## 16 0.064 al_24st     0 10700 0.328
    ## 17 0.064 al_24st    45 10500 0.328
    ## 18 0.064 al_24st    90 10500 0.320
    ## 19 0.081 al_24st     0 10000 0.315
    ## 20 0.081 al_24st    45 10000 0.320
    ## 21 0.081 al_24st    90  9900 0.314
    ## 22 0.081 al_24st     0 10100 0.312
    ## 23 0.081 al_24st    45  9900 0.312
    ## 24 0.081 al_24st    90 10000 0.316
    ## 25 0.081 al_24st     0 10000 0.311
    ## 26 0.081 al_24st    90  9900 0.314

Use the following tests to check your work.

``` r
## NOTE: No need to change this
## Names
assertthat::assert_that(
              setequal(
                df_stang_long %>% names,
                c("thick", "alloy", "angle", "E", "nu")
              )
            )
```

    ## [1] TRUE

``` r
## Dimensions
assertthat::assert_that(all(dim(df_stang_long) == c(26, 5)))
```

    ## [1] TRUE

``` r
## Type
assertthat::assert_that(
              (df_stang_long %>% pull(angle) %>% typeof()) == "integer"
            )
```

    ## [1] TRUE

``` r
print("Very good!")
```

    ## [1] "Very good!"

# EDA

<!-- -------------------------------------------------- -->

## Initial checks

<!-- ------------------------- -->

### **q2** Perform a basic EDA on the aluminum data *without visualization*. Use your analysis to answer the questions under *observations* below. In addition, add your own *specific* question that you’d like to answer about the data—you’ll answer it below in q3.

``` r
## Is there "One True Value"
glimpse(df_stang_long)
```

    ## Rows: 26
    ## Columns: 5
    ## $ thick <dbl> 0.022, 0.022, 0.022, 0.022, 0.022, 0.022, 0.032, 0.032, 0.032, 0…
    ## $ alloy <chr> "al_24st", "al_24st", "al_24st", "al_24st", "al_24st", "al_24st"…
    ## $ angle <int> 0, 45, 90, 0, 45, 90, 0, 45, 90, 0, 45, 90, 0, 45, 90, 0, 45, 90…
    ## $ E     <dbl> 10600, 10700, 10500, 10600, 10500, 10700, 10400, 10400, 10300, 1…
    ## $ nu    <dbl> 0.321, 0.329, 0.310, 0.323, 0.331, 0.323, 0.329, 0.318, 0.322, 0…

``` r
## Get the Number of Aluminum alloys
df_stang_long %>% 
  distinct(alloy)
```

    ##     alloy
    ## 1 al_24st

``` r
## What angles?
df_stang_long %>% 
  distinct(angle)
```

    ##   angle
    ## 1     0
    ## 2    45
    ## 3    90

``` r
## What thicknesses?
df_stang_long %>% 
  distinct(thick)
```

    ##   thick
    ## 1 0.022
    ## 2 0.032
    ## 3 0.064
    ## 4 0.081

``` r
## Mean Poisson's Ratio of al_24st
df_stang_long %>% 
  group_by(angle) %>% 
  summarise(E = mean(E))
```

    ## # A tibble: 3 × 2
    ##   angle      E
    ##   <int>  <dbl>
    ## 1     0 10356.
    ## 2    45 10362.
    ## 3    90 10289.

**Observations**:

- Is there “one true value” for the material properties of Aluminum?
  - **I know that any alloy of aluminum has multiple unique material
    properties. This includes elasticity modulus and possions ratio
    which are talked about above. So there is not “one true value” for a
    given alloy. There are multiple that can be used to characterize
    that alloy in relation to other alloys.**
- How many aluminum alloys are in this dataset? How do you know?
  - There is one alloy. I know this because counted the number of
    distinct alloy names. The result was one..
- What angles were tested?
  - 0, 45, and 90 degrees were tested which I found by using the
    distinct function.
- What thicknesses were tested?
  - 0.022, 0.032, 0.064, and 0.081 (inches) were the thicknesses
    measured. I found this by using the distinct function.
- Maya’s Questions: Is there a relationship between angle and young’s
  modulus? If so what is it?
  - see below
  - it is young’s modulus because of how they took the data (aka tension
    test)

## Visualize

<!-- ------------------------- -->

### **q3** Create a visualization to investigate your question from q2 above. Can you find an answer to your question using the dataset? Would you need additional information to answer your question?

``` r
## TASK: Investigate your question from q1 here
## transform angle values from integeter to characters
df_stang_plot <- 
  df_stang_long %>%
  transform(angle = as.character(angle))

## summarise the median and mean of E for each angle
df_stang_long %>% 
  group_by(angle) %>% 
  summarise(
    Median_E = median(E), 
    Mean_E = mean(E)
    )
```

    ## # A tibble: 3 × 3
    ##   angle Median_E Mean_E
    ##   <int>    <dbl>  <dbl>
    ## 1     0    10400 10356.
    ## 2    45    10450 10362.
    ## 3    90    10400 10289.

``` r
## scatter plot of angle and youngs modulus
df_stang_plot %>%
  group_by(angle) %>% 
  ggplot(aes(x = angle, y = E, fill = angle)) +
  geom_point()
```

![](c03-stang-assignment_files/figure-gfm/q3-task-1.png)<!-- -->

``` r
df_stang_plot %>%
  group_by(angle) %>% 
  ggplot(aes(x = angle, y = E, fill = angle)) +
  geom_boxplot()
```

![](c03-stang-assignment_files/figure-gfm/q3-task-2.png)<!-- -->

**Observations**:

- I started by taking the mean and mode of young’s modulus for each
  angle. The medians results in 0 and 90 degrees having the same value
  and 45 having a higher value. This suggests there might be no
  correlation or just a small one. The median suggests that same thing.
  So far I can not form any conclusions.
- Plot 1 shows not tends between angle and young’s modulus. In fact
  young’s modulus ranges between relatively all the same values for each
  angle. This suggests there is not trend between young’s modulus and
  angle. I want to look at the data one other way before finalizing my
  answer.
- Plot 2 is a box plot. As predicted the median for all three angles is
  about the same. 0 and 90 degrees have far more variation in their data
  from the mean and angle 45 is the only one with an outlier. Although
  from plot 1 we know that the values range for all angles.
- Answer: Angle does not affect Young’s Modulus. As angle changes
  young’s modulus does not notably change. This is clear in plot 1 where
  there is variation in young’s modulus for each angle and in plot 2
  where the median is roughly a horizontal trend line.

### **q4** Consider the following statement:

> “A material’s property (or material property) is an intensive property
> of some material, i.e. a physical property that does not depend on the
> amount of the material.”\[2\]

Note that the “amount of material” would vary with the thickness of a
tested plate. Does the following graph support or contradict the claim
that “elasticity `E` is an intensive material property.” Why or why not?
Is this evidence *conclusive* one way or another? Why or why not?

``` r
## NOTE: No need to change; run this chunk
df_stang_long %>%

  ggplot(aes(nu, E, color = as_factor(thick))) +
  geom_point(size = 3) +
  theme_minimal()
```

![](c03-stang-assignment_files/figure-gfm/q4-vis-1.png)<!-- -->

**Observations**:

- Does this graph support or contradict the claim above?
  - It contradicts the statement.
- Is this evidence *conclusive* one way or another?
  - No. 0.022, 0.032, and 0.064 are tend to have a higher elasticity
    value then 0.081 which is distinctly separate on the graph. More
    data needs to be taken by testing more thicknesses of material. This
    could potentially give more insight into this relationship between
    material thickness and elasticity.

# References

<!-- -------------------------------------------------- -->

\[1\] Stang, Greenspan, and Newman, “Poisson’s ratio of some structural
alloys for large strains” (1946) Journal of Research of the National
Bureau of Standards, (pdf
link)\[<https://nvlpubs.nist.gov/nistpubs/jres/37/jresv37n4p211_A1b.pdf>\]

\[2\] Wikipedia, *List of material properties*, accessed 2020-06-26,
(link)\[<https://en.wikipedia.org/wiki/List_of_materials_properties>\]
