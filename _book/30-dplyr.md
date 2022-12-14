# dplyr

## Introduction

For more help **PLEASE** check out  [Introduction to dplyr](https://dplyr.tidyverse.org/articles/dplyr.html) introducing the key functionality of the dplyr package.

https://dplyr.tidyverse.org/articles/dplyr.html

> Your life is about to change.  For the better, even.


## A Neat Resource

* [RStudio's Data Wrangling Cheat Sheet](http://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf) for dplyr and tidyr


## Single table verbs

`dplyr` aims to provide a function for each basic verb of data manipulation. These verbs can be organised into three categories based on the component of the dataset that they work with:

Rows:

- `filter()` chooses rows based on column values.
- `slice()` chooses rows based on location.
- `arrange()` changes the order of the rows.
    
Columns:

- `select()` changes whether or not a column is included.
- `rename()` changes the name of columns.
        `mutate()` changes the values of columns and creates new columns.
- `relocate()` changes the order of the columns.
    Groups of rows:
- `summarise()` collapses a group into a single row.  It’s not that useful until we learn the `group_by()` verb below.

## The pipe

All of the `dplyr` functions take a data frame (or tibble) as the first argument.  You can use the pipe to rewrite multiple operations that you can read left-to-right, top-to-bottom (reading the pipe operator as “then”).

> What is this: `%>%`?

## Loading `dplyr` 


```r
# You should already have done this but you'll need it
install.packages("dplyr")
```




## `starwars` examples


```r
library(dplyr)

starwars %>% 
  filter(species == "Droid")
#> # A tibble: 6 × 14
#>   name   height  mass hair_color skin_color  eye_color
#>   <chr>   <int> <dbl> <chr>      <chr>       <chr>    
#> 1 C-3PO     167    75 <NA>       gold        yellow   
#> 2 R2-D2      96    32 <NA>       white, blue red      
#> 3 R5-D4      97    32 <NA>       white, red  red      
#> 4 IG-88     200   140 none       metal       red      
#> 5 R4-P17     96    NA none       silver, red red, blue
#> 6 BB8        NA    NA none       none        black    
#> # … with 8 more variables: birth_year <dbl>, sex <chr>,
#> #   gender <chr>, homeworld <chr>, species <chr>,
#> #   films <list>, vehicles <list>, starships <list>


starwars %>% 
  select(name, ends_with("color"))
#> # A tibble: 87 × 4
#>    name               hair_color    skin_color  eye_color
#>    <chr>              <chr>         <chr>       <chr>    
#>  1 Luke Skywalker     blond         fair        blue     
#>  2 C-3PO              <NA>          gold        yellow   
#>  3 R2-D2              <NA>          white, blue red      
#>  4 Darth Vader        none          white       yellow   
#>  5 Leia Organa        brown         light       brown    
#>  6 Owen Lars          brown, grey   light       blue     
#>  7 Beru Whitesun lars brown         light       blue     
#>  8 R5-D4              <NA>          white, red  red      
#>  9 Biggs Darklighter  black         light       brown    
#> 10 Obi-Wan Kenobi     auburn, white fair        blue-gray
#> # … with 77 more rows


starwars %>% 
  mutate(name, bmi = mass / ((height / 100)  ^ 2)) %>%
  select(name:mass, bmi)
#> # A tibble: 87 × 4
#>    name               height  mass   bmi
#>    <chr>               <int> <dbl> <dbl>
#>  1 Luke Skywalker        172    77  26.0
#>  2 C-3PO                 167    75  26.9
#>  3 R2-D2                  96    32  34.7
#>  4 Darth Vader           202   136  33.3
#>  5 Leia Organa           150    49  21.8
#>  6 Owen Lars             178   120  37.9
#>  7 Beru Whitesun lars    165    75  27.5
#>  8 R5-D4                  97    32  34.0
#>  9 Biggs Darklighter     183    84  25.1
#> 10 Obi-Wan Kenobi        182    77  23.2
#> # … with 77 more rows


starwars %>% 
  arrange(desc(mass))
#> # A tibble: 87 × 14
#>    name         height  mass hair_color skin_color eye_color
#>    <chr>         <int> <dbl> <chr>      <chr>      <chr>    
#>  1 Jabba Desil…    175  1358 <NA>       green-tan… orange   
#>  2 Grievous        216   159 none       brown, wh… green, y…
#>  3 IG-88           200   140 none       metal      red      
#>  4 Darth Vader     202   136 none       white      yellow   
#>  5 Tarfful         234   136 brown      brown      blue     
#>  6 Owen Lars       178   120 brown, gr… light      blue     
#>  7 Bossk           190   113 none       green      red      
#>  8 Chewbacca       228   112 brown      unknown    blue     
#>  9 Jek Tono Po…    180   110 brown      fair       blue     
#> 10 Dexter Jett…    198   102 none       brown      yellow   
#> # … with 77 more rows, and 8 more variables:
#> #   birth_year <dbl>, sex <chr>, gender <chr>,
#> #   homeworld <chr>, species <chr>, films <list>,
#> #   vehicles <list>, starships <list>

starwars %>%
  group_by(species) %>%
  summarise(
    n = n(),
    mass = mean(mass, na.rm = TRUE)
  ) %>%
  filter(
    n > 1,
    mass > 50
  )
#> # A tibble: 8 × 3
#>   species      n  mass
#>   <chr>    <int> <dbl>
#> 1 Droid        6  69.8
#> 2 Gungan       3  74  
#> 3 Human       35  82.8
#> 4 Kaminoan     2  88  
#> 5 Mirialan     2  53.1
#> 6 Twi'lek      2  55  
#> 7 Wookiee      2 124  
#> 8 Zabrak       2  80
```

