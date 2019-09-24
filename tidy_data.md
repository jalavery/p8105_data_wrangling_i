Tidy Data
================
Jessica Lavery
9/24/2019

## Wide to long

``` r
pulse_data <-  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()

pulse_data
```

    ## # A tibble: 1,087 x 7
    ##      id   age sex   bdi_score_bl bdi_score_01m bdi_score_06m bdi_score_12m
    ##   <dbl> <dbl> <chr>        <dbl>         <dbl>         <dbl>         <dbl>
    ## 1 10003  48.0 male             7             1             2             0
    ## 2 10015  72.5 male             6            NA            NA            NA
    ## 3 10022  58.5 male            14             3             8            NA
    ## 4 10026  72.7 male            20             6            18            16
    ## 5 10035  60.4 male             4             0             1             2
    ## # … with 1,082 more rows

``` r
#can also write cols as bdi_score_bl:bdi_score_12m
pulse_tidy_data <- pulse_data %>% 
  pivot_longer(cols = c(bdi_score_bl, bdi_score_01m, bdi_score_06m, bdi_score_12m),
               names_to = "visit", 
               names_prefix = "bdi_score_", #removes bdi_score from the prefix in the visit variable
               values_to = "bdi")

pulse_tidy_data
```

    ## # A tibble: 4,348 x 5
    ##      id   age sex   visit   bdi
    ##   <dbl> <dbl> <chr> <chr> <dbl>
    ## 1 10003  48.0 male  bl        7
    ## 2 10003  48.0 male  01m       1
    ## 3 10003  48.0 male  06m       2
    ## 4 10003  48.0 male  12m       0
    ## 5 10015  72.5 male  bl        6
    ## # … with 4,343 more rows

``` r
#can do all of this in a single step with pipes and further clean up the data
pulse_data <-  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(cols = bdi_score_bl:bdi_score_12m,
               names_to = "visit", 
               names_prefix = "bdi_score_", 
               values_to = "bdi") %>% 
  select(id, visit, everything()) %>% 
  mutate(visit_replace = replace(visit, visit == "bl", "00m"),
         visit_recode = recode(visit, "bl" = "00m"),
         visit_factor = factor(visit_recode, levels = str_c(c("00", "01", "06", "12"), "m"))) %>% 
  arrange(id, visit)

pulse_data
```

    ## # A tibble: 4,348 x 8
    ##      id visit   age sex     bdi visit_replace visit_recode visit_factor
    ##   <dbl> <chr> <dbl> <chr> <dbl> <chr>         <chr>        <fct>       
    ## 1 10003 01m    48.0 male      1 01m           01m          01m         
    ## 2 10003 06m    48.0 male      2 06m           06m          06m         
    ## 3 10003 12m    48.0 male      0 12m           12m          12m         
    ## 4 10003 bl     48.0 male      7 00m           00m          00m         
    ## 5 10015 01m    72.5 male     NA 01m           01m          01m         
    ## # … with 4,343 more rows

## Separate function example in the Litters dataset

``` r
litters_data <- read_csv("./data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  # count(group)
  separate(col = group, into = c("dose", "day_of_tx"), sep = 3) %>% #sep indicates separator between columns, character/numeric have different implications, numeric is position to split at 
  mutate(dose = str_to_lower(dose),
  wt_gain = gd18_weight - gd0_weight) %>%
  arrange(litter_number)
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

``` r
litters_data
```

    ## # A tibble: 49 x 10
    ##   dose  day_of_tx litter_number gd0_weight gd18_weight gd_of_birth
    ##   <chr> <chr>     <chr>              <dbl>       <dbl>       <dbl>
    ## 1 con   7         #1/2/95/2             27        42            19
    ## 2 con   7         #1/5/3/83/3-…         NA        NA            20
    ## 3 con   8         #1/6/2/2/95-2         NA        NA            20
    ## 4 mod   7         #1/82/3-2             NA        NA            19
    ## 5 low   8         #100                  20        39.2          20
    ## # … with 44 more rows, and 4 more variables: pups_born_alive <dbl>,
    ## #   pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

``` r
litters_data_long <- litters_data %>% 
  pivot_longer(cols = gd0_weight:gd18_weight,
               names_to = "gd",
               names_prefix = "gd",
               values_to = "weight") %>% 
  select(litter_number, gd, weight) %>% 
  mutate(gd = recode(gd, "0_weight" = 0, "18_weight" = 18))

str(litters_data_long)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    98 obs. of  3 variables:
    ##  $ litter_number: chr  "#1/2/95/2" "#1/2/95/2" "#1/5/3/83/3-3/2" "#1/5/3/83/3-3/2" ...
    ##  $ gd           : num  0 18 0 18 0 18 0 18 0 18 ...
    ##  $ weight       : num  27 42 NA NA NA NA NA NA 20 39.2 ...

## Pivot wider

``` r
analysis_result = tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)

analysis_result
```

    ## # A tibble: 4 x 3
    ##   group     time   mean
    ##   <chr>     <chr> <dbl>
    ## 1 treatment pre     4  
    ## 2 treatment post    8  
    ## 3 placebo   pre     3.5
    ## 4 placebo   post    4

``` r
pivot_wider(
  analysis_result, 
  names_from = "time", 
  values_from = "mean")
```

    ## # A tibble: 2 x 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

## Binding rows

``` r
fellowship_ring = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>%
  mutate(movie = "fellowship_ring")

two_towers = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")

lotr_tidy <- bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  pivot_longer(cols = female:male,
               names_to = "sex",
               values_to = "words") %>% 
  mutate(race = str_to_lower(race)) %>% 
  select(movie, everything())

lotr_tidy
```

    ## # A tibble: 18 x 4
    ##    movie           race   sex    words
    ##    <chr>           <chr>  <chr>  <dbl>
    ##  1 fellowship_ring elf    female  1229
    ##  2 fellowship_ring elf    male     971
    ##  3 fellowship_ring hobbit female    14
    ##  4 fellowship_ring hobbit male    3644
    ##  5 fellowship_ring man    female     0
    ##  6 fellowship_ring man    male    1995
    ##  7 two_towers      elf    female   331
    ##  8 two_towers      elf    male     513
    ##  9 two_towers      hobbit female     0
    ## 10 two_towers      hobbit male    2463
    ## 11 two_towers      man    female   401
    ## 12 two_towers      man    male    3589
    ## 13 return_king     elf    female   183
    ## 14 return_king     elf    male     510
    ## 15 return_king     hobbit female     2
    ## 16 return_king     hobbit male    2673
    ## 17 return_king     man    female   268
    ## 18 return_king     man    male    2459

## Joining

``` r
pup_data = 
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 

litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))

#merge litter information onto the pup data (1 rec/pup, merge on 1 rec/litter)
fas_data = 
  left_join(pup_data, litter_data, by = "litter_number")

fas_data %>% view()
```

### Joins learning assessment

``` r
surv_os <- read_csv("./data/survey_results/surv_os.csv") %>% 
  janitor::clean_names() %>% 
  rename(id = what_is_your_uni, os = what_operating_system_do_you_use)
```

    ## Parsed with column specification:
    ## cols(
    ##   `What is your UNI?` = col_character(),
    ##   `What operating system do you use?` = col_character()
    ## )

``` r
  # separate(col = what_is_your_uni, into = c("drop", "student"), sep = "_")
surv_os
```

    ## # A tibble: 173 x 2
    ##   id          os        
    ##   <chr>       <chr>     
    ## 1 student_87  <NA>      
    ## 2 student_106 Windows 10
    ## 3 student_66  Mac OS X  
    ## 4 student_93  Windows 10
    ## 5 student_99  Mac OS X  
    ## # … with 168 more rows

``` r
surv_pr_git <- read_csv("./data/survey_results/surv_program_git.csv") %>% 
  janitor::clean_names() %>% 
  rename(id = what_is_your_uni, 
         degree = what_is_your_degree_program, 
         git = which_most_accurately_describes_your_experience_with_git)
```

    ## Parsed with column specification:
    ## cols(
    ##   `What is your UNI?` = col_character(),
    ##   `What is your degree program?` = col_character(),
    ##   `Which most accurately describes your experience with Git?` = col_character()
    ## )

``` r
#practice different types of joins
left <- left_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

``` r
nrow(left)
```

    ## [1] 175

``` r
inner <- inner_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

``` r
nrow(inner)
```

    ## [1] 129

``` r
anti_os <- anti_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

``` r
nrow(anti_os)
```

    ## [1] 46

``` r
anti_git <- anti_join(surv_pr_git, surv_os)
```

    ## Joining, by = "id"

``` r
nrow(anti_git)
```

    ## [1] 15
