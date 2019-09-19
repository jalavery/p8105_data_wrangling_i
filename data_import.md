Data Import
================
Jessica Lavery
9/17/2019

# Load in a dataset

``` r
# be careful to use read_csv (reads in as a tibble) instead of read.csv (reads in data frame)
litters_data = read_csv(file = "./data/FAS_litters.csv")
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

# Clean up variable names

``` r
# view existing variable names
names(litters_data)
```

    ## [1] "Group"             "Litter Number"     "GD0 weight"       
    ## [4] "GD18 weight"       "GD of Birth"       "Pups born alive"  
    ## [7] "Pups dead @ birth" "Pups survive"

``` r
# clean up variable names
litters_data <-  janitor::clean_names(litters_data)

# view new variable names
names(litters_data)
```

    ## [1] "group"           "litter_number"   "gd0_weight"      "gd18_weight"    
    ## [5] "gd_of_birth"     "pups_born_alive" "pups_dead_birth" "pups_survive"

# Inspect data

``` r
# View(litters_data)

# check first few rows of the data
litters_data
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #3/83/3-3           NA          NA            20               9
    ##  9 Con8  #2/95/3             NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## # … with 39 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
# check last few rows of the data
tail(litters_data, 5)
```

    ## # A tibble: 5 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #100                20          39.2          20               8
    ## 2 Low8  #4/84               21.8        35.2          20               4
    ## 3 Low8  #108                25.6        47.5          20               8
    ## 4 Low8  #99                 23.5        39            20               6
    ## 5 Low8  #110                25.5        42.7          20               7
    ## # … with 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
# skim the data
skimr::skim(litters_data)
```

    ## Skim summary statistics
    ##  n obs: 49 
    ##  n variables: 8 
    ## 
    ## ── Variable type:character ─────────────────────────────────────────────────────────────────
    ##       variable missing complete  n min max empty n_unique
    ##          group       0       49 49   4   4     0        6
    ##  litter_number       0       49 49   3  15     0       49
    ## 
    ## ── Variable type:numeric ───────────────────────────────────────────────────────────────────
    ##         variable missing complete  n  mean   sd   p0   p25   p50   p75
    ##      gd_of_birth       0       49 49 19.65 0.48 19   19    20    20   
    ##       gd0_weight      15       34 49 24.38 3.28 17   22.3  24.1  26.67
    ##      gd18_weight      17       32 49 41.52 4.05 33.4 38.88 42.25 43.8 
    ##  pups_born_alive       0       49 49  7.35 1.76  3    6     8     8   
    ##  pups_dead_birth       0       49 49  0.33 0.75  0    0     0     0   
    ##     pups_survive       0       49 49  6.41 2.05  1    5     7     8   
    ##  p100     hist
    ##  20   ▅▁▁▁▁▁▁▇
    ##  33.4 ▁▃▇▇▇▆▁▁
    ##  52.7 ▂▃▃▇▆▂▁▁
    ##  11   ▂▂▃▃▇▅▁▁
    ##   4   ▇▂▁▁▁▁▁▁
    ##   9   ▂▂▃▃▅▇▇▅

# Read in pups dataset

``` r
pups_data <- read_csv("./data/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )

# Clean pups variable names

``` r
# look at existing variable names
names(pups_data)
```

    ## [1] "Litter Number" "Sex"           "PD ears"       "PD eyes"      
    ## [5] "PD pivot"      "PD walk"

``` r
# clean variable names
pups_data = janitor::clean_names(pups_data)

# look at new variable names
names(pups_data)
```

    ## [1] "litter_number" "sex"           "pd_ears"       "pd_eyes"      
    ## [5] "pd_pivot"      "pd_walk"

# Inspect the pups data

``` r
# look at first few rows
pups_data
```

    ## # A tibble: 313 x 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # … with 303 more rows

``` r
# look at last few rows
tail(pups_data, n = 10)
```

    ## # A tibble: 10 x 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #7/110/3-2        2       4      14        7       9
    ##  2 #2/95/2           2       4      12        7       9
    ##  3 #2/95/2           2       4      12        6       8
    ##  4 #2/95/2           2       4      13        7       9
    ##  5 #2/95/2           2       4      12        7       9
    ##  6 #2/95/2           2       3      13        6       8
    ##  7 #2/95/2           2       3      13        7       9
    ##  8 #82/4             2       4      13        7       9
    ##  9 #82/4             2       3      13        7       9
    ## 10 #82/4             2       3      13        7       9

``` r
# skim the data
skimr::skim(pups_data)
```

    ## Skim summary statistics
    ##  n obs: 313 
    ##  n variables: 6 
    ## 
    ## ── Variable type:character ─────────────────────────────────────────────────────────────────
    ##       variable missing complete   n min max empty n_unique
    ##  litter_number       0      313 313   3  15     0       49
    ## 
    ## ── Variable type:numeric ───────────────────────────────────────────────────────────────────
    ##  variable missing complete   n  mean   sd p0 p25 p50 p75 p100     hist
    ##   pd_ears      18      295 313  3.68 0.59  2   3   4   4    5 ▁▁▅▁▁▇▁▁
    ##   pd_eyes      13      300 313 12.99 0.62 12  13  13  13   15 ▂▁▇▁▁▂▁▁
    ##  pd_pivot      13      300 313  7.09 1.51  4   6   7   8   12 ▃▆▇▃▂▁▁▁
    ##   pd_walk       0      313 313  9.5  1.34  7   9   9  10   14 ▁▅▇▅▃▂▁▁
    ##       sex       0      313 313  1.5  0.5   1   1   2   2    2 ▇▁▁▁▁▁▁▇

# Compare read in with base R

``` r
pups_base = read.csv("./data/FAS_pups.csv")
pups_readr = read_csv("./data/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )

# Read Excel files

``` r
library(readxl)

mlb11_data <- read_excel("./data/mlb11.xlsx", n_max = 20)

# variable names are clean, don't need to use janitor::clean_names
head(mlb11_data, 5)
```

    ## # A tibble: 5 x 12
    ##   team   runs at_bats  hits homeruns bat_avg strikeouts stolen_bases  wins
    ##   <chr> <dbl>   <dbl> <dbl>    <dbl>   <dbl>      <dbl>        <dbl> <dbl>
    ## 1 Texa…   855    5659  1599      210   0.283        930          143    96
    ## 2 Bost…   875    5710  1600      203   0.28        1108          102    90
    ## 3 Detr…   787    5563  1540      169   0.277       1143           49    95
    ## 4 Kans…   730    5672  1560      129   0.275       1006          153    71
    ## 5 St. …   762    5532  1513      162   0.273        978           57    90
    ## # … with 3 more variables: new_onbase <dbl>, new_slug <dbl>, new_obs <dbl>

# Read in a SAS dataset

``` r
library(haven)

pulse_data = read_sas("./data/public_pulse_data.sas7bdat")

#variable names are mixed case, want to clean
head(pulse_data, 5)
```

    ## # A tibble: 5 x 7
    ##      ID   age Sex   BDIScore_BL BDIScore_01m BDIScore_06m BDIScore_12m
    ##   <dbl> <dbl> <chr>       <dbl>        <dbl>        <dbl>        <dbl>
    ## 1 10003  48.0 male            7            1            2            0
    ## 2 10015  72.5 male            6           NA           NA           NA
    ## 3 10022  58.5 male           14            3            8           NA
    ## 4 10026  72.7 male           20            6           18           16
    ## 5 10035  60.4 male            4            0            1            2

``` r
pulse_data = janitor::clean_names(pulse_data)

names(pulse_data)
```

    ## [1] "id"            "age"           "sex"           "bdi_score_bl" 
    ## [5] "bdi_score_01m" "bdi_score_06m" "bdi_score_12m"
