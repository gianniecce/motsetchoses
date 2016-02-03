---
layout: post
title:  "Number of Partner"
categories: [jekyll, rstats]
tags: [knitr, servr, httpuv, websocket]
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

load('/Users/giacomovagni/site/motsetchoses/_data/dtaEpisode.RData')
dtaEpisode$weekend = ifelse(dtaEpisode$day == 1 | dtaEpisode$day == 7, 'weekend', 'weekdays')
{% endhighlight %}

The code is : 

{% highlight r %}
dtaEpisode %>% group_by(hldid) %>% mutate(ncouple = n_distinct(idno)) %>% select(idno, hldid, ncouple)
{% endhighlight %}



{% highlight text %}
## Source: local data frame [8,569 x 3]
## Groups: hldid
## 
##      idno hldid ncouple
## 1  3809_1  3809       2
## 2  3809_1  3809       2
## 3  3809_1  3809       2
## 4  3809_1  3809       2
## 5  3809_1  3809       2
## 6  3809_1  3809       2
## 7  3809_1  3809       2
## 8  3809_1  3809       2
## 9  3809_1  3809       2
## 10 3809_1  3809       2
## ..    ...   ...     ...
{% endhighlight %}
