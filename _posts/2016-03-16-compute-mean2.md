---
layout: post
title:  "Compute mean for activities"
categories: [jekyll]
tags: [mtus]
---

  hey


{% highlight r %}
library(plyr)
library(dplyr)
library(tidyr)
library(mtusRlocal)
library(knitr)
library(reshape2)
library(xtable)

load('/Users/giacomovagni/site/motsetchoses/_data/ukAllsub.RData')
{% endhighlight %}

First we need to compute the mean at the individual level for each activities. 

Using `matches`, we select the relevant variables of the dataset. 


{% highlight r %}
ukAllsub = ukAllsub %>% group_by(idind, day) %>% mutate(ndiaries = n())
ukAllsub_act = ukAllsub %>% select( idind, day, ndiaries, matches('main')) 
{% endhighlight %}

The database looks like this 


{% highlight r %}
ukAllsub_act %>% as.data.frame() 
{% endhighlight %}



{% highlight text %}
##         idind      day ndiaries main1 main2 main3 main4 main5 main6
## 1 10121511974  Tuesday        1    NA    NA    NA    NA    NA    NA
## 2 10121511974 Thursday        1    NA    NA    NA    NA    NA    NA
## 3 10121511974   Friday        1    NA    NA    NA    NA    NA    NA
## 4 10121511974 Saturday        1    NA    NA    NA    NA    NA    NA
##   main7 main8 main9 main10 main11 main12 main13 main14 main15 main16
## 1    NA    NA    NA     NA     NA     NA      2      2      2      2
## 2    NA    NA    NA     NA     NA     NA      6      6      6      6
## 3    NA    NA    NA     NA     NA     NA      2      2      2      2
## 4    NA    NA    NA     NA     NA     NA      2      2      2      2
##   main17 main18 main19
## 1      2      2      2
## 2      6      6      2
## 3      2      2      2
## 4      2      2      2
{% endhighlight %}


We set up the `missing` values to `character` 

{% highlight r %}
ukAllsub_act[is.na(ukAllsub_act)] <- 'NA'
{% endhighlight %}

The number of episode is 


{% highlight r %}
# nbr of episodes 
19 * 5
{% endhighlight %}



{% highlight text %}
## [1] 95
{% endhighlight %}



{% highlight r %}
ukAllsub_act_summary = ukAllsub_act %>% melt(id.vars = c('idind', 'day', 'ndiaries')) %>% 
  # in order to retreive the time (nbr episodes) # 
  # mutate(variable = substr(variable, 5, 6)) %>% 
  group_by(idind, day, ndiaries, value) %>% 
  summarise(n = n() * 5) %>%
  mutate(value = paste('act_', value, sep = '')) %>% 
  spread(value, n, fill = 0)
{% endhighlight %}

We get 

{% highlight r %}
ukAllsub_act_summary
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 6]
## 
##         idind      day ndiaries act_2 act_6 act_NA
## 1 10121511974  Tuesday        1    35     0     60
## 2 10121511974 Thursday        1     5    30     60
## 3 10121511974   Friday        1    35     0     60
## 4 10121511974 Saturday        1    35     0     60
{% endhighlight %}


{% highlight r %}
ukAllsub_act_summary %>% select(matches('act')) %>% summarise_each(funs(mean))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [1 x 3]
## 
##   act_2 act_6 act_NA
## 1  27.5   7.5     60
{% endhighlight %}


{% highlight r %}
ukAllsub_act_summary %>% group_by(day) %>%  select(day, matches('act')) %>% summarise_each(funs(mean))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 4]
## 
##        day act_2 act_6 act_NA
## 1  Tuesday    35     0     60
## 2 Thursday     5    30     60
## 3   Friday    35     0     60
## 4 Saturday    35     0     60
{% endhighlight %}





