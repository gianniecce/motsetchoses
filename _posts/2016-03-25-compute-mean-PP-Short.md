---
layout: post
title:  "Compute mean for activities - all format"
categories: [jekyll]
tags: [mtus]
---


<span class='newthought'>This post</span> will demonstrate how to simply calcualte mean time of different activities from a *PP-short* dataset. 


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

### Mean of Short PP file 

The `sum` of all activites duration for each individuals for a day must be **1440**. 


{% highlight r %}
dta %>% group_by(id, day) %>% summarise(sum(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 3]
## Groups: id
## 
##   id      day sum(duration)
## 1  1   Monday          1440
## 2  1 Saturday          1440
## 3  2   Monday          1440
## 4  2 Saturday          1440
{% endhighlight %}

The individual mean can be computed such as : 

{% highlight r %}
dta %>% group_by(id, activities) %>% summarise(mean(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [12 x 3]
## Groups: id
## 
##    id activities mean(duration)
## 1   1         TV      270.00000
## 2   1   domestic      300.00000
## 3   1     eating      113.33333
## 4   1  free time      400.00000
## 5   1  paid work      500.00000
## 6   1      sleep      400.00000
## 7   2         TV      220.00000
## 8   2   domestic      350.00000
## 9   2     eating       63.33333
## 10  2  free time      300.00000
## 11  2  paid work      600.00000
## 12  2      sleep      500.00000
{% endhighlight %}

The aggregate mean by day : 

{% highlight r %}
dta %>% group_by(day, activities) %>% summarise(mean(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [9 x 3]
## Groups: day
## 
##        day activities mean(duration)
## 1   Monday         TV            350
## 2   Monday     eating             95
## 3   Monday  paid work            550
## 4   Monday      sleep            350
## 5 Saturday         TV            140
## 6 Saturday   domestic            325
## 7 Saturday     eating             75
## 8 Saturday  free time            350
## 9 Saturday      sleep            550
{% endhighlight %}

The aggregate mean by activities, regardless of the day : 

{% highlight r %}
dta %>% group_by(activities) %>% summarise(mean(duration))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [6 x 2]
## 
##   activities mean(duration)
## 1         TV      245.00000
## 2   domestic      325.00000
## 3     eating       88.33333
## 4  free time      350.00000
## 5  paid work      550.00000
## 6      sleep      450.00000
{% endhighlight %}

### Mean of Long PP file 


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
## Groups: activities
## 
##   activities      day mean(time)
## 1         TV   Monday        350
## 2         TV Saturday        140
## 3   domestic Saturday        325
## 4     eating   Monday        190
## 5     eating Saturday         75
## 6  free time Saturday        350
## 7  paid work   Monday        550
## 8      sleep   Monday        350
## 9      sleep Saturday        550
{% endhighlight %}

### Mean of Sequence File 


#### Aggregate  

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
{% endhighlight %}



{% highlight text %}
## Source: local data frame [1 x 1]
## 
##   n
## 1 4
{% endhighlight %}



{% highlight r %}
# by act only
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(value) %>% summarise(n = n()) %>% # back to PP 
  mutate(time =  (n* 1) / 4 ) %>% # divide by count 
  mutate(TimeClock(time)) 
{% endhighlight %}



{% highlight text %}
## Source: local data frame [6 x 4]
## 
##       value    n  time TimeClock(time)
## 1        TV  980 245.0           04:05
## 2  domestic  650 162.5           02:42
## 3    eating  530 132.5           02:12
## 4 free time  700 175.0           02:55
## 5 paid work 1100 275.0           04:35
## 6     sleep 1800 450.0           07:30
{% endhighlight %}



{% highlight r %}
# by act and days 
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(day, value) %>% summarise(n = n()) %>% 
  mutate(time =  (n* 1) / 2) %>% # divide by count 
  mutate(TimeClock(time)) 
{% endhighlight %}



{% highlight text %}
## Source: local data frame [9 x 5]
## Groups: day
## 
##        day     value    n time TimeClock(time)
## 1   Monday        TV  700  350           05:50
## 2   Monday    eating  380  190           03:10
## 3   Monday paid work 1100  550           09:10
## 4   Monday     sleep  700  350           05:50
## 5 Saturday        TV  280  140           02:20
## 6 Saturday  domestic  650  325           05:25
## 7 Saturday    eating  150   75           01:15
## 8 Saturday free time  700  350           05:50
## 9 Saturday     sleep 1100  550           09:10
{% endhighlight %}



{% highlight r %}
# if you haev the full data then group by what you want  
dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(idind, day, value) %>% summarise(n = n()) %>% 
  group_by(value) %>% summarise( mean(n) ) 
{% endhighlight %}



{% highlight text %}
## Source: local data frame [6 x 2]
## 
##       value mean(n)
## 1        TV   245.0
## 2  domestic   325.0
## 3    eating   132.5
## 4 free time   350.0
## 5 paid work   550.0
## 6     sleep   450.0
{% endhighlight %}



{% highlight r %}
  mutate(TimeClock(time)) 
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): not compatible with requested type
{% endhighlight %}



{% highlight r %}
# or simply activities 
  dtaSeq %>% select(idind, day, matches('main')) %>% 
  melt(id.vars = c("idind", "day")) %>% group_by(idind, day, value) %>% summarise(n = n()) %>% 
  group_by(value, day) %>% summarise( mean(n) ) 
{% endhighlight %}



{% highlight text %}
## Source: local data frame [9 x 3]
## Groups: value
## 
##       value      day mean(n)
## 1        TV   Monday     350
## 2        TV Saturday     140
## 3  domestic Saturday     325
## 4    eating   Monday     190
## 5    eating Saturday      75
## 6 free time Saturday     350
## 7 paid work   Monday     550
## 8     sleep   Monday     350
## 9     sleep Saturday     550
{% endhighlight %}



{% highlight r %}
  mutate(TimeClock(time)) 
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): not compatible with requested type
{% endhighlight %}

#### Individual

From a PP file  


{% highlight r %}
head(dtaPP)
{% endhighlight %}



{% highlight text %}
## Source: local data frame [6 x 5]
## Groups: id, day
## 
##   id      day activities epnum Time
## 1  1 Saturday      sleep     1    1
## 2  1 Saturday      sleep     1    2
## 3  1 Saturday      sleep     1    3
## 4  1 Saturday      sleep     1    4
## 5  1 Saturday      sleep     1    5
## 6  1 Saturday      sleep     1    6
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
## 
##   id      day act_TV act_domestic act_eating act_free time
## 1  1   Monday    400            0        240             0
## 2  1 Saturday    140          300        100           400
## 3  2   Monday    300            0        140             0
## 4  2 Saturday    140          350         50           300
## Variables not shown: act_paid work (dbl), act_sleep (dbl)
{% endhighlight %}



{% highlight r %}
dtaPP_sum %>% select(-id, -day) %>% summarise_each(funs(mean))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [1 x 6]
## 
##   act_TV act_domestic act_eating act_free time act_paid work
## 1    245        162.5      132.5           175           275
## Variables not shown: act_sleep (dbl)
{% endhighlight %}





