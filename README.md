
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.3.3

    ## Warning: package 'tidyr' was built under R version 4.3.3

    ## Warning: package 'purrr' was built under R version 4.3.3

    ## Warning: package 'forcats' was built under R version 4.3.3

    ## Warning: package 'lubridate' was built under R version 4.3.3

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(dplyr)
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
deaths <- av %>%
  pivot_longer(cols = starts_with("Death"),
               names_to = "Time",
               values_to = "Death") %>%
               filter(Death != "")

deaths$Time <- parse_number(deaths$Time)

head(deaths)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## 3 http://marvel.wiki… "Anthony …        3068 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Robert B…        2089 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>

Similarly, deal with the returns of characters.

``` r
returns <- deaths %>%
  pivot_longer(cols = starts_with("Return"),
               names_to = "Time_returns",
               values_to = "Return") %>%
               filter(Return != "")

returns$Time_returns <- parse_number(returns$Time_returns)

head(returns)
```

    ## # A tibble: 6 × 15
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## 3 http://marvel.wiki… "Anthony …        3068 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Robert B…        2089 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## # ℹ 9 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>, Time_returns <dbl>, Return <chr>

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avg <- colSums(returns == "YES")
avg[13]/173
```

    ##     Death 
    ## 0.8265896

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

“There’s a 2-in-3 chance that a member of the Avengers returned from
their first stint in the afterlife”

### Include the code

``` r
firsts <- subset(returns, returns$Time == 1)
firsts <- subset(firsts, firsts$Time_returns == 1)
firstreturn <- subset(firsts, firsts$Return == "YES")
count(firstreturn) / count(firsts)
```

    ##           n
    ## 1 0.6666667

### Include your answer

When it comes to return rate of first time deaths for the Avengers,
there is exactly a 2 in 3 chance that if you die, you will return. This
was found by narrowing the results to first time deaths and returns and
then dividing the count of people who returned by the total count of
first deaths.

### FiveThirtyEight Statement
**Out of 173 listed Avengers, my analysis found that 69 had died at least one time after they joined the team. That’s about 40 percent of all people who have ever signed on to the team."**
### Include the code
```{r}
#use original av data to get the 173 Avengers. Check for duplicate names
num_unique_avengers <- av %>%
        distinct(Name.Alias, .keep_all = TRUE)

#keep only rows with a recorded death
recorded_deaths <- num_unique_avengers %>%
  pivot_longer(cols = starts_with("Death"),
               names_to = "Time",
               values_to = "Death") %>%
               filter(Death == "YES")

#count how many total avengers and how many died calculate percentage.
num_avengers <- nrow(num_unique_avengers)
num_deaths <- nrow(recorded_deaths)
percent_died <- (num_deaths/num_avengers) * 100

num_avengers
num_deaths
percent_died
```
### Include your answer
**My analysis exposed an mistake in this statement. The authors of this analysis did not make sure that there were no duplicate avengers in the dataset (for exaple Thor Odinson showed up 2x). After keeping only rows with distinct avenger names there were only 163 listed avengers, 83 of which died at least once. That makes almost 51% of signees who have died at least once.**

Upload your changes to the repository. Discuss and refine answers as a
team.
