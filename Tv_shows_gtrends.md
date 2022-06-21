Google Trends Research project
================

Here is some practice project about working with ggtrends.

For this mini piece, I’m just wondering if a few of my favorite shows
that have the same genre/type such as “Vikings”, “The Witcher”, and “The
Last Kingdom” spike together in popularity as people tend to look for
similar shows when they are done binge-watching one. Most likely the
spikes will also be visible at each shows release of a new season.

``` r
library(dplyr)
library(readr)
library(magrittr)
library(gtrendsR)
library(tidyr)
library(tidyverse)
library(quantmod)
```

Time to gather the data from google trends by searching for keywords.

``` r
trends <- gtrends(keyword=c("Vikings", "The Last Kingdom","Valhalla","Witcher"))
trends_over_time <- trends$interest_over_time
trends_over_time <- trends_over_time %>% mutate(hits = as.numeric(hits))
```

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

``` r
trends_over_time <- trends_over_time %>% replace_na(list(hits = 0))
head(trends_over_time)
```

    ##         date hits keyword   geo      time gprop category
    ## 1 2017-06-25    5 Vikings world today+5-y   web        0
    ## 2 2017-07-02    5 Vikings world today+5-y   web        0
    ## 3 2017-07-09    5 Vikings world today+5-y   web        0
    ## 4 2017-07-16    6 Vikings world today+5-y   web        0
    ## 5 2017-07-23    6 Vikings world today+5-y   web        0
    ## 6 2017-07-30    6 Vikings world today+5-y   web        0

``` r
tail(trends_over_time)
```

    ##            date hits keyword   geo      time gprop category
    ## 1039 2022-05-15    3 Witcher world today+5-y   web        0
    ## 1040 2022-05-22    3 Witcher world today+5-y   web        0
    ## 1041 2022-05-29    3 Witcher world today+5-y   web        0
    ## 1042 2022-06-05    3 Witcher world today+5-y   web        0
    ## 1043 2022-06-12    3 Witcher world today+5-y   web        0
    ## 1044 2022-06-19    0 Witcher world today+5-y   web        0

``` r
# Plotting the trends

viz <- trends_over_time %>%
    ggplot() + geom_line(aes(x = date, hits, color = keyword)) +
    scale_color_discrete(name = "Show", labels = c("The Last Kingdom", "Vikings: Valhalla",
                                                   "Vikings","The Witcher")) +
  labs(title = "Google Trend Data For viking/medieval related tv Shows",y = "Hits (Normalized to be between 0 and 1000", x = "Date") 
 
viz
```

![](Tv_shows_gtrends_files/figure-gfm/unnamed-chunk-3-1.jpeg)<!-- -->

``` r
ggsave(filename = "gtrend_tv.png", plot = viz, width = 15, height = 10, units = "in", dpi = 300)
```

It appears that there is a popularity increase for all shows in the
beginning of 2020, and 2022. It also correlates with their season
release date.

``` r
jpeg(file = "tv_show_trend.jpeg")
```
