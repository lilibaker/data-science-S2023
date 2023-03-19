Antibiotics
================
Lili Baker
2023-03-02

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category    | Needs Improvement                                                                                                | Satisfactory                                                                                                               |
|-------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| Effort      | Some task **q**’s left unattempted                                                                               | All task **q**’s attempted                                                                                                 |
| Observed    | Did not document observations, or observations incorrect                                                         | Documented correct observations based on analysis                                                                          |
| Supported   | Some observations not clearly supported by analysis                                                              | All observations clearly supported by analysis (table, graph, etc.)                                                        |
| Assessed    | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support      |
| Specified   | Uses the phrase “more data are necessary” without clarification                                                  | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability                                 | Code sufficiently close to the [style guide](https://style.tidyverse.org/)                                                 |

## Due Date

<!-- ------------------------- -->

All the deliverables stated in the rubrics above are due **at midnight**
before the day of the class discussion of the challenge. See the
[Syllabus](https://docs.google.com/document/d/1qeP6DUS8Djq_A0HMllMqsSqX3a9dbcx1/edit?usp=sharing&ouid=110386251748498665069&rtpof=true&sd=true)
for more information.

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.4.0      ✔ purrr   1.0.1 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. You must also show whether or not each bacterium is Gram
positive or negative.

``` r
df_antibiotics %>% 
  pivot_longer(
    names_to = "antibiotic",
    values_to = "MIC",
    cols = c(`penicillin`, `streptomycin`, `neomycin`)
  ) %>% 
  mutate(MIC = as.double(MIC)) %>% 
  
  ggplot(aes(antibiotic, MIC)) +
  geom_point(aes(color = bacteria)) +
  scale_y_log10() +
  facet_wrap(~ gram) +
  theme(
    axis.text.x = element_text(angle = 45, vjust = 1, hjust=1),
    legend.key.size = unit(0.3, 'cm')
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. You must also show whether or not each bacterium is Gram
positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>% 
  pivot_longer(
    names_to = "antibiotic",
    values_to = "MIC",
    cols = c(`penicillin`, `streptomycin`, `neomycin`)
  ) %>% 
  mutate(MIC = as.double(MIC)) %>% 
  
  ggplot(aes(antibiotic, MIC)) +
  geom_col(aes(fill = gram)) +
  facet_wrap(~ bacteria) +
  scale_y_log10() +
  theme(
    legend.key.size = unit(0.3, 'cm'),
    axis.text.x = element_text(angle = 45, vjust = 1, hjust=1),
    plot.title = element_text(size = 2),
    strip.text = element_text(size = 6.5)
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>% 
  pivot_longer(
    names_to = "antibiotic",
    values_to = "MIC",
    cols = c(`penicillin`, `streptomycin`, `neomycin`)
  ) %>% 
  mutate(MIC = as.double(MIC)) %>% 
  
  ggplot(aes(antibiotic, MIC)) +
  geom_point() +
  geom_point(
    data = . %>% filter(startsWith(bacteria, 'Streptococcus')),
    mapping = aes(color = bacteria),
    size = 2
  ) +
  geom_point(
    data = . %>% filter(bacteria == 'Diplococcus pneumonia'),
    mapping = aes(shape = bacteria, color = bacteria),
    size = 3
  ) +
  scale_y_log10() +
  facet_wrap(~ gram) +
  theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust=1))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.3-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>% 
  pivot_longer(
    names_to = "antibiotic",
    values_to = "MIC",
    cols = c(`penicillin`, `streptomycin`, `neomycin`)
  ) %>% 
  mutate(MIC = as.double(MIC)) %>% 
  
  ggplot(aes(antibiotic, MIC)) +
  geom_boxplot() +
  geom_point(aes(color = bacteria)) +
  scale_y_log10() +
  facet_wrap(~ gram) +
  theme(
    axis.text.x = element_text(angle = 45, vjust = 1, hjust=1),
    legend.key.size = unit(0.3, 'cm')
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>% 
  pivot_longer(
    names_to = "antibiotic",
    values_to = "MIC",
    cols = c(`penicillin`, `streptomycin`, `neomycin`)
  ) %>% 
  mutate(MIC = as.double(MIC)) %>% 
  
  ggplot(aes(bacteria, MIC, color = gram)) +
  geom_point() +
  facet_wrap(~ antibiotic) +
  scale_y_log10() +
  theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust=1, size = 5))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations* - What is your response to the question above? - With the
exception of penicillin, the MIC values amongst negative grams for the
antibiotics tend to lie closer to each other. On the other hand, the MIC
values for the positive grams differs more for each bacteria. However,
in the case of penicillin, the MIC values for negative grams differ much
more than those of the positive strains. - Which of your visuals above
(1 through 5) is **most effective** at helping to answer this
question? - visual 4 - Why? - The boxplot enables the isolation of the
effectiveness of the antibiotics at the gram level, facilitating the
comparison across grams. The addition of points for each of the bacteria
demonstrates how these differences are evident at the bacteria level. In
that regard, the combination of the boxplot and the individual points
enables the comparison at both the gram and bacteria level.

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* was renamed *Streptococcus
> pneumoniae*?

*Observations* - What is your response to the question above? -
*Diplococcus pneumoniae* exhibits similar MIC values to the
*Streptococcus* bacteria, with the exception of *Streptococcus fecalis*.
Specifically, for neomycin, each of the bacteria exhibit MIC values
around 1e+01. For penicillin, the bacteria exhibit MIC values around
1e-02. Finally, for streptomycin, the bacteria exhibit MIC values around
1e+01. Consequently, the similarities between the MIC values for
*Diplococcus pneumoniae* and the *Streptococcus* bacteria indicate that
*Diplococcus pneumoniae* should actually fall within the *Streptococcus*
genus. - Which of your visuals above (1 through 5) is **most effective**
at helping to answer this question? - visual 2 - Why? - Visual 2 enables
a quick comparison of how the bacteria are affected by the antibiotics.
The individual graphs for each bacteria make it easier to compare across
bacteria as the graphs are not cluttered and the axes contain the same
ranges. Thus, it is easy to see how *Diplococcus pneumoniae* and the
*Streptococcus* bacteria individually react to the antibiotics and
perform a comparison, whilst also comparing how this differs from the
other included bacteria.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)