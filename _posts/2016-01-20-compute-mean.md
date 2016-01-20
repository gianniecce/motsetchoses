---
layout: post
title:  "Compute means"
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


{% highlight r %}
kable(dtaEpisode [1:2, 1:5])
{% endhighlight %}



|        | countrya| survey| swave| msamp| hldid|
|:-------|--------:|------:|-----:|-----:|-----:|
|7810405 |       37|   2000|     0|     0|  3809|
|7810406 |       37|   2000|     0|     0|  3809|




{% highlight r %}
dtaEpisodeSumTime = dtaEpisode %>% group_by(idno, weekend, day, av) %>% summarise(sumtime = sum(time)) %>% group_by(idno) %>% mutate(nday = n_distinct(day)) 

# by ind 
dtaEpisodeSumTime %>% group_by(av, weekend) %>% summarise(mean(sumtime))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [75 x 3]
## Groups: av
## 
##    av  weekend mean(sumtime)
## 1   1 weekdays     475.96491
## 2   1  weekend     407.81250
## 3   2 weekdays     171.53846
## 4   2  weekend     110.00000
## 5   3 weekdays      30.00000
## 6   4 weekdays     310.00000
## 7   5 weekdays      58.34646
## 8   5  weekend      56.66667
## 9   6 weekdays      51.01695
## 10  6  weekend      60.25862
## .. ..      ...           ...
{% endhighlight %}



{% highlight r %}
# check 
dtaEpisode %>% group_by(idno, day, av) %>% summarise(sumtime = sum(time)) %>% mutate(sum(sumtime))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [3,512 x 5]
## Groups: idno, day
## 
##        idno day av sumtime sum(sumtime)
## 1  103799_1   1  1     400         1440
## 2  103799_1   1  5     130         1440
## 3  103799_1   1  6      70         1440
## 4  103799_1   1  7      60         1440
## 5  103799_1   1  8      10         1440
## 6  103799_1   1 11     140         1440
## 7  103799_1   1 13      50         1440
## 8  103799_1   1 15      60         1440
## 9  103799_1   1 16     460         1440
## 10 103799_1   1 31      20         1440
## ..      ... ... ..     ...          ...
{% endhighlight %}



{% highlight r %}
# spread 
dtaEpisodeSumTimeSpread = dtaEpisodeSumTime %>% spread(av, sumtime, fill = 0)

# does everybody has filled 2 days? 
table(dtaEpisodeSumTimeSpread$nday) 
{% endhighlight %}



{% highlight text %}
## 
##   2 
## 308
{% endhighlight %}


{% highlight r %}
kable(dtaEpisodeSumTimeSpread[1:10,1:10])
{% endhighlight %}



|idno     |weekend  | day| nday|   1|  2|  3|  4|   5|   6|
|:--------|:--------|---:|----:|---:|--:|--:|--:|---:|---:|
|103799_1 |weekdays |   4|    2| 410|  0|  0|  0|  50|   0|
|103799_1 |weekend  |   1|    2| 400|  0|  0|  0| 130|  70|
|103799_2 |weekdays |   4|    2| 470|  0|  0|  0|  90|  20|
|103799_2 |weekend  |   1|    2|   0|  0|  0|  0|   0|  80|
|110594_1 |weekdays |   3|    2|   0|  0|  0|  0|   0|  50|
|110594_1 |weekend  |   1|    2|   0|  0|  0|  0|   0| 120|
|110594_2 |weekdays |   3|    2|   0|  0|  0|  0|   0|  10|
|110594_2 |weekend  |   1|    2|   0|  0|  0|  0|   0|  10|
|129380_1 |weekdays |   6|    2| 270| 90|  0|  0|  30|  30|
|129380_1 |weekend  |   7|    2|   0| 90|  0|  0|  10|   0|


{% highlight r %}
aggregate(dtaEpisodeSumTimeSpread$`1` ~ dtaEpisodeSumTimeSpread$weekend, FUN = mean) 
{% endhighlight %}



{% highlight text %}
##   dtaEpisodeSumTimeSpread$weekend dtaEpisodeSumTimeSpread$`1`
## 1                        weekdays                   352.33766
## 2                         weekend                    84.74026
{% endhighlight %}



{% highlight r %}
dtaEpisodeSumTimeSpread %>% select(-idno, -nday) %>% group_by(weekend) %>% summarise_each(funs(mean))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [2 x 41]
## 
##    weekend      day         1        2         3        4        5
## 1 weekdays 4.324675 352.33766 14.48052 0.1948052 2.012987 48.11688
## 2  weekend 4.038961  84.74026  5.00000 0.0000000 0.000000 13.24675
## Variables not shown: 6 (dbl), 7 (dbl), 8 (dbl), 9 (dbl), 10 (dbl), 11
##   (dbl), 12 (dbl), 13 (dbl), 14 (dbl), 15 (dbl), 16 (dbl), 17 (dbl),
##   18 (dbl), 19 (dbl), 20 (dbl), 21 (dbl), 22 (dbl), 23 (dbl), 24
##   (dbl), 25 (dbl), 28 (dbl), 29 (dbl), 30 (dbl), 31 (dbl), 32 (dbl),
##   33 (dbl), 34 (dbl), 35 (dbl), 36 (dbl), 37 (dbl), 38 (dbl), 39
##   (dbl), 40 (dbl), 41 (dbl)
{% endhighlight %}



{% highlight r %}
dtMelt = dtaEpisodeSumTimeSpread %>% melt(id.vars = c('idno', 'weekend'))
dtMelt %>% group_by(weekend, variable) %>% summarise(mv = mean(value)) %>% spread(variable, mv)
{% endhighlight %}



{% highlight text %}
## Source: local data frame [2 x 42]
## 
##    weekend      day nday         1        2         3        4
## 1 weekdays 4.324675    2 352.33766 14.48052 0.1948052 2.012987
## 2  weekend 4.038961    2  84.74026  5.00000 0.0000000 0.000000
## Variables not shown: 5 (dbl), 6 (dbl), 7 (dbl), 8 (dbl), 9 (dbl), 10
##   (dbl), 11 (dbl), 12 (dbl), 13 (dbl), 14 (dbl), 15 (dbl), 16 (dbl),
##   17 (dbl), 18 (dbl), 19 (dbl), 20 (dbl), 21 (dbl), 22 (dbl), 23
##   (dbl), 24 (dbl), 25 (dbl), 28 (dbl), 29 (dbl), 30 (dbl), 31 (dbl),
##   32 (dbl), 33 (dbl), 34 (dbl), 35 (dbl), 36 (dbl), 37 (dbl), 38
##   (dbl), 39 (dbl), 40 (dbl), 41 (dbl)
{% endhighlight %}



{% highlight r %}
# dtaEpisodeSumTime  %>% group_by() %>% group_by(av, weekend) %>% summarise(mv = mean(sumtime)) 
{% endhighlight %}


Function 

{% highlight r %}
TimeLongToWide = function(dta = weekend){
  
  # Sequence 
  seqDay = dta[rep(1:nrow(dta), dta[,'time'] ), -3] %>%
    group_by(id) %>% 
    mutate( Time = 1:n() ) %>%
    spread(Time, av)
  
  seqDay = as.data.frame(seqDay)
  
  row.names(seqDay) <- seqDay[,'id']
  seqDay = seqDay [,-1]
  
  return(seqDay)
}

dtaWeekend = dtaEpisode %>% filter(weekend == 'weekend') %>% select(id = idno, av, time)

# use it 
dtaWideWeekend = TimeLongToWide(dtaWeekend)
{% endhighlight %}


{% highlight r %}
kable(dtaWideWeekend [1:4, 1:10]) 
{% endhighlight %}



|         |  1|  2|  3|  4|  5|  6|  7|  8|  9| 10|
|:--------|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|
|103799_1 | 16| 16| 16| 16| 16| 16| 16| 16| 16| 16|
|103799_2 | 16| 16| 16| 16| 16| 16| 16| 16| 16| 16|
|110594_1 | 16| 16| 16| 16| 16| 16| 16| 16| 16| 16|
|110594_2 | 16| 16| 16| 16| 16| 16| 16| 16| 16| 16|


{% highlight r %}
# id
dtaWideWeekend$idno = row.names(dtaWideWeekend)
# melt 
dtaMelt = dtaWideWeekend %>% melt(id.vars = "idno") 
{% endhighlight %}


{% highlight r %}
kable(dtaMelt %>% head())
{% endhighlight %}



|idno     |variable | value|
|:--------|:--------|-----:|
|103799_1 |1        |    16|
|103799_2 |1        |    16|
|110594_1 |1        |    16|
|110594_2 |1        |    16|
|129380_1 |1        |     2|
|129380_2 |1        |    16|



{% highlight r %}
intervals = 1
dtaMelt %>% group_by(idno, value) %>% summarise(nsum = n() * intervals) %>% group_by(idno) %>% mutate(sum(nsum))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [1,745 x 4]
## Groups: idno
## 
##        idno value nsum sum(nsum)
## 1  103799_1     1  400      1440
## 2  103799_1     5  130      1440
## 3  103799_1     6   70      1440
## 4  103799_1     7   60      1440
## 5  103799_1     8   10      1440
## 6  103799_1    11  140      1440
## 7  103799_1    13   50      1440
## 8  103799_1    15   60      1440
## 9  103799_1    16  460      1440
## 10 103799_1    31   20      1440
## ..      ...   ...  ...       ...
{% endhighlight %}


{% highlight r %}
dtaMelt %>% group_by(idno, value) %>% summarise(nsum = n() * intervals) %>% group_by(value) %>% summarise(sum(nsum) / n_distinct(idno))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [37 x 2]
## 
##    value sum(nsum)/n_distinct(idno)
## 1      1                  407.81250
## 2      2                  110.00000
## 3      5                   56.66667
## 4      6                   60.25862
## 5      7                   88.30189
## 6      8                   82.77778
## 7      9                  161.81818
## 8     10                   87.06667
## 9     11                   98.27586
## 10    12                   55.11111
## ..   ...                        ...
{% endhighlight %}



{% highlight r %}
dtaMelt %>% group_by(idno, value) %>% summarise(nsum = n() * intervals) %>% group_by(value) %>% summarise(mean(nsum))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [37 x 2]
## 
##    value mean(nsum)
## 1      1  407.81250
## 2      2  110.00000
## 3      5   56.66667
## 4      6   60.25862
## 5      7   88.30189
## 6      8   82.77778
## 7      9  161.81818
## 8     10   87.06667
## 9     11   98.27586
## 10    12   55.11111
## ..   ...        ...
{% endhighlight %}


