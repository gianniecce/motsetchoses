---
layout: post
title:  "Compute mean for activities - all format"
categories: [jekyll]
tags: [mtus]
---


<span class='newthought'>This post</span> will demonstrate how to simply calcualte mean time of different activities for different format. 


{% highlight r %}
# Load the libraries and data
library(plyr)
library(dplyr)
library(tidyr)
library(mtusRlocal)
library(knitr)
library(reshape2)
library(xtable)

dta = read.csv('/Users/giacomovagni/site/motsetchoses/_data/dtaSpells.csv')
{% endhighlight %}

### 1. Mean of Short PP file 

The `sum` of all activites duration for each individuals for a day must be **1440**. 


{% highlight r %}
dta %>% group_by(id, day) %>% summarise(sum(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 3]
## Groups: id [?]
## 
##      id      day `sum(duration)`
##   <int>   <fctr>           <int>
## 1     1   Monday            1440
## 2     1 Saturday            1440
## 3     2   Monday            1440
## 4     2 Saturday            1440
{% endhighlight %}

The individual mean can be computed such as : 

{% highlight r %}
dta %>% group_by(id, activities) %>% summarise(mean(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [12 x 3]
## Groups: id [?]
## 
##       id activities `mean(duration)`
##    <int>     <fctr>            <dbl>
## 1      1         TV        270.00000
## 2      1   domestic        300.00000
## 3      1     eating        113.33333
## 4      1  free time        400.00000
## 5      1  paid work        500.00000
## 6      1      sleep        400.00000
## 7      2         TV        220.00000
## 8      2   domestic        350.00000
## 9      2     eating         63.33333
## 10     2  free time        300.00000
## 11     2  paid work        600.00000
## 12     2      sleep        500.00000
{% endhighlight %}

The aggregate mean by day : 

{% highlight r %}
dta %>% group_by(day, activities) %>% summarise(mean(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [9 x 3]
## Groups: day [?]
## 
##        day activities `mean(duration)`
##     <fctr>     <fctr>            <dbl>
## 1   Monday         TV              350
## 2   Monday     eating               95
## 3   Monday  paid work              550
## 4   Monday      sleep              350
## 5 Saturday         TV              140
## 6 Saturday   domestic              325
## 7 Saturday     eating               75
## 8 Saturday  free time              350
## 9 Saturday      sleep              550
{% endhighlight %}

The aggregate mean by activities, regardless of the day : 

{% highlight r %}
dta %>% group_by(activities) %>% summarise(mean(duration))
{% endhighlight %}



{% highlight text %}
## # A tibble: 6 <U+00D7> 2
##   activities `mean(duration)`
##       <fctr>            <dbl>
## 1         TV        245.00000
## 2   domestic        325.00000
## 3     eating         88.33333
## 4  free time        350.00000
## 5  paid work        550.00000
## 6      sleep        450.00000
{% endhighlight %}

### 2. Mean of Long PP file 

For Long PP file 


{% highlight r %}
# transform 
dtaPP= dta[rep(1:nrow(dta), dta$duration), ] %>% 
  select(-duration) %>% 
  group_by(id, day) %>% 
  mutate(Time = 1:n()) 

dtaPP %>% group_by(id, day, activities) %>% summarise(time = n()) %>% # transform back to short PP 
  group_by(activities, day) %>% 
  summarise(mean(time))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [9 x 3]
## Groups: activities [?]
## 
##   activities      day `mean(time)`
##       <fctr>   <fctr>        <dbl>
## 1         TV   Monday          350
## 2         TV Saturday          140
## 3   domestic Saturday          325
## 4     eating   Monday          190
## 5     eating Saturday           75
## 6  free time Saturday          350
## 7  paid work   Monday          550
## 8      sleep   Monday          350
## 9      sleep Saturday          550
{% endhighlight %}

### 3. Mean of Sequence File 


#### 3.1. Aggregate  

The mean time of activities for long PP can be computed by the following codes : 


{% highlight r %}
# transform PP to Wide 
dtaSeq = dta[rep(1:nrow(dta), dta$duration), ] %>% 
  select(-duration) %>% 
  group_by(id, day) %>% 
  mutate(Time = 1:n()) %>% # PP 
  arrange(id, day, Time) %>% select(-epnum) %>% spread(Time, activities) # Wide 

# colnames renames  
colnames(dtaSeq) = c('idind', 'day', paste("main", 1:1440, sep = ""))
{% endhighlight %}


{% highlight r %}
N = count(dtaSeq) 
N 
# by act only
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(value, day) %>% 
  summarise(n = n()) %>% # back to PP 
  mutate(time =  (n* 1) / 4 ) %>% # divide by count 
  mutate(TimeClock(time)) 

# if dummy variables 
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% 
  mutate(value = ifelse(value == 'sleep', 1, 0)) %>% 
  group_by(idind, day) %>% summarise(n())


%>% mutate(nid = n_distinct(idind)) %>% 
  group_by(nid, day) %>% 
  summarise(n = sum(value) * 1) %>% 
  mutate(meanN = n / nid) %>% mutate(meanN = TimeClock(meanN))




# by act and days 
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(day, value) %>% 
  summarise(n = n()) %>% 
  mutate(time =  (n* 1) / 2) %>% # divide by count 
  mutate(TimeClock(time)) 

# if you haev the full data then group by what you want  
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(idind, day, value) %>% 
  summarise(n = n()) %>% 
  group_by(value) %>% summarise( mean(n) ) 
  mutate(TimeClock(time)) 
  
# or simply activities 
  dtaSeq %>% select(idind, day, matches('main')) %>% 
    melt(id.vars = c("idind", "day")) %>% 
    group_by(idind, day, value) %>% 
    summarise(n = n()) %>% 
    group_by(value, day) %>% 
    summarise( mean(n) ) 
  mutate(TimeClock(time)) 
{% endhighlight %}



{% highlight text %}
## Error: <text>:17:1: unexpected SPECIAL
## 16: 
## 17: %>%
##     ^
{% endhighlight %}

Alternative when you are unsure of the `count` results 


{% highlight r %}
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(value) %>% 
    summarise(n = n()) %>% # back to PP 
    mutate(p = n / sum(n)) %>% 
    mutate(time =  1440 * p) %>% # divide by count 
  mutate(TimeClock(time)) 
{% endhighlight %}



{% highlight text %}
## Adding missing grouping variables: `id`
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): invalid column index : NA for variable: id = id
{% endhighlight %}



#### 3.2. Individual

From a PP file  


{% highlight r %}
head(dtaPP)
{% endhighlight %}



{% highlight text %}
## Source: local data frame [6 x 5]
## Groups: id, day [1]
## 
##      id      day activities epnum  Time
##   <int>   <fctr>     <fctr> <int> <int>
## 1     1 Saturday      sleep     1     1
## 2     1 Saturday      sleep     1     2
## 3     1 Saturday      sleep     1     3
## 4     1 Saturday      sleep     1     4
## 5     1 Saturday      sleep     1     5
## 6     1 Saturday      sleep     1     6
{% endhighlight %}



{% highlight r %}
dtaPP_sum = dtaPP %>% 
  group_by(id, day, activities) %>% 
  summarise(n = n() * 1) %>% # multiply by the interval - here is 1 minutes 
  mutate(activities = paste('act_', activities, sep = '')) %>% 
  spread(activities, n, fill = 0) 

dtaPP_sum
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 8]
## Groups: id, day [4]
## 
##      id      day act_TV act_domestic act_eating `act_free time`
## * <int>   <fctr>  <dbl>        <dbl>      <dbl>           <dbl>
## 1     1   Monday    400            0        240               0
## 2     1 Saturday    140          300        100             400
## 3     2   Monday    300            0        140               0
## 4     2 Saturday    140          350         50             300
## # ... with 2 more variables: `act_paid work` <dbl>, act_sleep <dbl>
{% endhighlight %}



{% highlight r %}
dtaPP_sum %>% select(-id, -day) %>% summarise_each(funs(mean))
{% endhighlight %}



{% highlight text %}
## Adding missing grouping variables: `id`, `day`
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 8]
## Groups: id [?]
## 
##      id      day act_TV act_domestic act_eating `act_free time`
##   <int>   <fctr>  <dbl>        <dbl>      <dbl>           <dbl>
## 1     1   Monday    400            0        240               0
## 2     1 Saturday    140          300        100             400
## 3     2   Monday    300            0        140               0
## 4     2 Saturday    140          350         50             300
## # ... with 2 more variables: `act_paid work` <dbl>, act_sleep <dbl>
{% endhighlight %}

#### 3.3. Weighted Sum and Weighted Mean 



{% highlight r %}
# weights of weekdays 
weekendproba = (2 / 7)

# weights - 1 is weekdays 
dtaSeq$day_weights = ifelse(dtaSeq$day %in% "Monday", 1, 1 - weekendproba)

# Weighted mean 
dtaSeq %>% select(idind, day, day_weights, matches('main')) %>% 
  melt(id.vars = c("idind", "day", "day_weights")) %>% 
  group_by(idind, day, day_weights, value) %>% 
  summarise(n = n()) %>% 
  mutate(value = paste('act_', value, sep = '')) %>% 
  spread(value, n, fill = 0) %>% 
  summarise(meanTV = mean(act_TV), weightedmean = weighted.mean(act_TV, day_weights)) 
{% endhighlight %}



{% highlight text %}
## Adding missing grouping variables: `id`
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): invalid column index : NA for variable: id = id
{% endhighlight %}

The weighted sum 

{% highlight r %}
dtaSeq %>% count(day) %>% mutate(p = n / sum(n))
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): unknown column 'idind'
{% endhighlight %}



{% highlight r %}
dtaSeq %>% count(day, wt = day_weights) %>% mutate(p = n / sum(n))
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): unknown column 'idind'
{% endhighlight %}





