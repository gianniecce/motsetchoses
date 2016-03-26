---
layout: post
title:  "Change the interval time "
categories: [jekyll]
tags: [mtus]
---


<span class='newthought'>This post</span> will discuss how to change the interval time when transforming time use data 



{% highlight r %}
library('schoolmath')
library(dplyr)

dta = read.csv('/Users/giacomovagni/site/motsetchoses/_data/dtaSpells.csv')
{% endhighlight %}


### Find the greatest common divisor 
We need first 


{% highlight r %}
l = length(dta$duration) 
v = array(0, l)

for(i in 2:l){
  v[i] = gcd(dta$duration[i], dta$duration[i-1]) 
}

minV = min(v[-1]) 
minV
{% endhighlight %}



{% highlight text %}
## [1] 20
{% endhighlight %}



{% highlight r %}
dta$duration = dta$duration / minV
{% endhighlight %}


{% highlight r %}
dtaPP = dta[rep(1:nrow(dta), dta$duration), ] %>% 
  select(-duration) %>% 
  group_by(id, day) %>% 
  mutate( Time = 1:n())
{% endhighlight %}

The new Long Person Period file has now `72` episodes, which is equals to `1440` when multiplied by `20` 


{% highlight r %}
dtaPP %>% group_by(id, day) %>% summarise(max(Time))
{% endhighlight %}



{% highlight text %}
## Source: local data frame [4 x 3]
## Groups: id
## 
##   id      day max(Time)
## 1  1   Monday        72
## 2  1 Saturday        72
## 3  2   Monday        72
## 4  2 Saturday        71
{% endhighlight %}



{% highlight r %}
72 * 20
{% endhighlight %}



{% highlight text %}
## [1] 1440
{% endhighlight %}
