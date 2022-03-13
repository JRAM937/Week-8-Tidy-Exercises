Tidy Data
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.2.0     ✓ stringr 1.4.0
    ## ✓ readr   2.1.2     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(babynames)

# Toy data
cases <- tribble(
  ~Country, ~"2011", ~"2012", ~"2013",
      "FR",    7000,    6900,    7000,
      "DE",    5800,    6000,    6200,
      "US",   15000,   14000,   13000
)

pollution <- tribble(
       ~city, ~size, ~amount,
  "New York", "large",      23,
  "New York", "small",      14,
    "London", "large",      22,
    "London", "small",      16,
   "Beijing", "large",     121,
   "Beijing", "small",     121
)

names(who) <- stringr::str_replace(names(who), 
                                   "newrel", 
                                   "new_rel")
```

Run the code below to look at the tables from the presentation

``` r
table1
```

    ## # A tibble: 6 × 4
    ##   country      year  cases population
    ##   <chr>       <int>  <int>      <int>
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3 Brazil       1999  37737  172006362
    ## 4 Brazil       2000  80488  174504898
    ## 5 China        1999 212258 1272915272
    ## 6 China        2000 213766 1280428583

``` r
table2
```

    ## # A tibble: 12 × 4
    ##    country      year type            count
    ##    <chr>       <int> <chr>           <int>
    ##  1 Afghanistan  1999 cases             745
    ##  2 Afghanistan  1999 population   19987071
    ##  3 Afghanistan  2000 cases            2666
    ##  4 Afghanistan  2000 population   20595360
    ##  5 Brazil       1999 cases           37737
    ##  6 Brazil       1999 population  172006362
    ##  7 Brazil       2000 cases           80488
    ##  8 Brazil       2000 population  174504898
    ##  9 China        1999 cases          212258
    ## 10 China        1999 population 1272915272
    ## 11 China        2000 cases          213766
    ## 12 China        2000 population 1280428583

``` r
table3
```

    ## # A tibble: 6 × 3
    ##   country      year rate             
    ## * <chr>       <int> <chr>            
    ## 1 Afghanistan  1999 745/19987071     
    ## 2 Afghanistan  2000 2666/20595360    
    ## 3 Brazil       1999 37737/172006362  
    ## 4 Brazil       2000 80488/174504898  
    ## 5 China        1999 212258/1272915272
    ## 6 China        2000 213766/1280428583

``` r
table4a
```

    ## # A tibble: 3 × 3
    ##   country     `1999` `2000`
    ## * <chr>        <int>  <int>
    ## 1 Afghanistan    745   2666
    ## 2 Brazil       37737  80488
    ## 3 China       212258 213766

``` r
table4b
```

    ## # A tibble: 3 × 3
    ##   country         `1999`     `2000`
    ## * <chr>            <int>      <int>
    ## 1 Afghanistan   19987071   20595360
    ## 2 Brazil       172006362  174504898
    ## 3 China       1272915272 1280428583

``` r
table5
```

    ## # A tibble: 6 × 4
    ##   country     century year  rate             
    ## * <chr>       <chr>   <chr> <chr>            
    ## 1 Afghanistan 19      99    745/19987071     
    ## 2 Afghanistan 20      00    2666/20595360    
    ## 3 Brazil      19      99    37737/172006362  
    ## 4 Brazil      20      00    80488/174504898  
    ## 5 China       19      99    212258/1272915272
    ## 6 China       20      00    213766/1280428583

# tidyr

## Your Turn 1

On a sheet of paper, draw how the cases data set would look if it had
the same values grouped into three columns: **country**, **year**, **n**

tidyr is transitioning `gather()` to `pivot_longer()`. Here are
equivalent codes:

``` r
cases %>% pivot_longer(names_to = "year", values_to = "n", 2:4)
```

    ## # A tibble: 9 × 3
    ##   Country year      n
    ##   <chr>   <chr> <dbl>
    ## 1 FR      2011   7000
    ## 2 FR      2012   6900
    ## 3 FR      2013   7000
    ## 4 DE      2011   5800
    ## 5 DE      2012   6000
    ## 6 DE      2013   6200
    ## 7 US      2011  15000
    ## 8 US      2012  14000
    ## 9 US      2013  13000

``` r
cases %>% gather(key="year",value="n", 2:4)
```

    ## # A tibble: 9 × 3
    ##   Country year      n
    ##   <chr>   <chr> <dbl>
    ## 1 FR      2011   7000
    ## 2 DE      2011   5800
    ## 3 US      2011  15000
    ## 4 FR      2012   6900
    ## 5 DE      2012   6000
    ## 6 US      2012  14000
    ## 7 FR      2013   7000
    ## 8 DE      2013   6200
    ## 9 US      2013  13000

## Your Turn 2

Try the following codes - what do they do? (annotate them with a
comment)

``` r
cases %>% pivot_longer(names_to="year",values_to="n", 2:4) #Pivot the table into 3 columns: Country, year, and n
```

    ## # A tibble: 9 × 3
    ##   Country year      n
    ##   <chr>   <chr> <dbl>
    ## 1 FR      2011   7000
    ## 2 FR      2012   6900
    ## 3 FR      2013   7000
    ## 4 DE      2011   5800
    ## 5 DE      2012   6000
    ## 6 DE      2013   6200
    ## 7 US      2011  15000
    ## 8 US      2012  14000
    ## 9 US      2013  13000

``` r
cases %>% pivot_longer(names_to="year",values_to= "n", c("2011", "2012", "2013")) #Same as the one above but a vector of years from the table is also made.
```

    ## # A tibble: 9 × 3
    ##   Country year      n
    ##   <chr>   <chr> <dbl>
    ## 1 FR      2011   7000
    ## 2 FR      2012   6900
    ## 3 FR      2013   7000
    ## 4 DE      2011   5800
    ## 5 DE      2012   6000
    ## 6 DE      2013   6200
    ## 7 US      2011  15000
    ## 8 US      2012  14000
    ## 9 US      2013  13000

``` r
cases %>% pivot_longer(names_to="year",values_to="n", starts_with("201")) #Only puts years starting with "201" in the year column
```

    ## # A tibble: 9 × 3
    ##   Country year      n
    ##   <chr>   <chr> <dbl>
    ## 1 FR      2011   7000
    ## 2 FR      2012   6900
    ## 3 FR      2013   7000
    ## 4 DE      2011   5800
    ## 5 DE      2012   6000
    ## 6 DE      2013   6200
    ## 7 US      2011  15000
    ## 8 US      2012  14000
    ## 9 US      2013  13000

``` r
cases %>% pivot_longer(names_to="year",values_to="n", -Country) #arranges all data into columns except for the country column which was already there.
```

    ## # A tibble: 9 × 3
    ##   Country year      n
    ##   <chr>   <chr> <dbl>
    ## 1 FR      2011   7000
    ## 2 FR      2012   6900
    ## 3 FR      2013   7000
    ## 4 DE      2011   5800
    ## 5 DE      2012   6000
    ## 6 DE      2013   6200
    ## 7 US      2011  15000
    ## 8 US      2012  14000
    ## 9 US      2013  13000

``` r
#In short, all lines of code produce the exact same thing.
```

## Your Turn 3

Use `pivot_longer()` to reorganize `table4a` into three columns:
**country**, **year**, and **cases**.

``` r
table4a %>% pivot_longer(names_to = "year", values_to = "cases", 2:3, names_transform = list(year = as.integer)) %>% 
  write_csv("newtable4a.csv")
```

## Your Turn 4

On a sheet of paper, draw how this data set would look if it had the
same values grouped into three columns: **city**, **large**, **small**

## Your Turn 5

Use `pivot_wider()` to reorganize `table2` into four columns:
**country**, **year**, **cases**, and **population**.

``` r
table2 %>% pivot_wider(names_from = "type", values_from = "count")
```

    ## # A tibble: 6 × 4
    ##   country      year  cases population
    ##   <chr>       <int>  <int>      <int>
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3 Brazil       1999  37737  172006362
    ## 4 Brazil       2000  80488  174504898
    ## 5 China        1999 212258 1272915272
    ## 6 China        2000 213766 1280428583

# who

## Your Turn 6

Gather the 5th through 60th columns of `who` into columns named `codes`
and `n`.

Then select just the `county`, `year`, `codes` and `n` variables.

``` r
who %>%
  pivot_longer(names_to = "codes", values_to = "n", cols=5:60) %>%
  select(-iso2, -iso3)
```

    ## # A tibble: 405,440 × 4
    ##    country      year codes            n
    ##    <chr>       <int> <chr>        <int>
    ##  1 Afghanistan  1980 new_sp_m014     NA
    ##  2 Afghanistan  1980 new_sp_m1524    NA
    ##  3 Afghanistan  1980 new_sp_m2534    NA
    ##  4 Afghanistan  1980 new_sp_m3544    NA
    ##  5 Afghanistan  1980 new_sp_m4554    NA
    ##  6 Afghanistan  1980 new_sp_m5564    NA
    ##  7 Afghanistan  1980 new_sp_m65      NA
    ##  8 Afghanistan  1980 new_sp_f014     NA
    ##  9 Afghanistan  1980 new_sp_f1524    NA
    ## 10 Afghanistan  1980 new_sp_f2534    NA
    ## # … with 405,430 more rows

Run the following code

``` r
who %>%
  pivot_longer(names_to ="codes", values_to = "n", cols=5:60) %>%
  select(-iso2, -iso3) %>%
  separate(codes, c("new", "type", "sexage"), sep = "_") %>% 
  select(-new)
```

    ## # A tibble: 405,440 × 5
    ##    country      year type  sexage     n
    ##    <chr>       <int> <chr> <chr>  <int>
    ##  1 Afghanistan  1980 sp    m014      NA
    ##  2 Afghanistan  1980 sp    m1524     NA
    ##  3 Afghanistan  1980 sp    m2534     NA
    ##  4 Afghanistan  1980 sp    m3544     NA
    ##  5 Afghanistan  1980 sp    m4554     NA
    ##  6 Afghanistan  1980 sp    m5564     NA
    ##  7 Afghanistan  1980 sp    m65       NA
    ##  8 Afghanistan  1980 sp    f014      NA
    ##  9 Afghanistan  1980 sp    f1524     NA
    ## 10 Afghanistan  1980 sp    f2534     NA
    ## # … with 405,430 more rows

## Your Turn 7

Separate the `sexage` column into `sex` and `age` columns.

*(Hint: Be sure to remove each `_________` before running the code)*

``` r
who %>%
  pivot_longer(names_to ="codes", values_to = "n", cols=5:60) %>%
  select(-iso2, -iso3) %>%
  separate(codes, c("new", "type", "sexage"), sep = "_") %>%
  select(-new) %>% 
  separate(sexage, c("sex", "age"), sep = 1)
```

    ## # A tibble: 405,440 × 6
    ##    country      year type  sex   age       n
    ##    <chr>       <int> <chr> <chr> <chr> <int>
    ##  1 Afghanistan  1980 sp    m     014      NA
    ##  2 Afghanistan  1980 sp    m     1524     NA
    ##  3 Afghanistan  1980 sp    m     2534     NA
    ##  4 Afghanistan  1980 sp    m     3544     NA
    ##  5 Afghanistan  1980 sp    m     4554     NA
    ##  6 Afghanistan  1980 sp    m     5564     NA
    ##  7 Afghanistan  1980 sp    m     65       NA
    ##  8 Afghanistan  1980 sp    f     014      NA
    ##  9 Afghanistan  1980 sp    f     1524     NA
    ## 10 Afghanistan  1980 sp    f     2534     NA
    ## # … with 405,430 more rows

------------------------------------------------------------------------

# Take Aways

Data comes in many formats but R prefers just one: *tidy data*.

A data set is tidy if and only if:

1.  Every variable is in its own column
2.  Every observation is in its own row
3.  Every value is in its own cell (which follows from the above)

What is a variable and an observation may depend on your immediate goal.
