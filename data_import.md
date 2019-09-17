Data Import
================
Jessica Lavery
9/17/2019

# Load in a dataset

``` r
# be careful to use read_csv instead of read.csv
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
