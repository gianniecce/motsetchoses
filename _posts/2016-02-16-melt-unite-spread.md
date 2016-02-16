---
layout: post
title:  "Melt Unite Spread"
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

load(file = '/Users/giacomovagni/site/motsetchoses/_data/undp.RData')
{% endhighlight %}

The code is : 

{% highlight r %}
undp2 = undp %>% select(Country, Year, Sex, Marital.status, X15.19, X20.24, X25.29, X30.34, X35.39, X40.44, X45.49, X50.54, X60.64, X65.) %>%
  melt(id.vars = c('Country', 'Year', 'Sex', 'Marital.status')) %>% filter(Country == 'United Kingdom' ) 

undp2 = undp2 %>% filter(variable %in% c('X20.24', 'X25.29', 'X30.34', 'X35.39', 'X40.44', 'X45.49'))

UNUK2050ysummary = undp2 %>% filter(Marital.status %in% c("Married", "Single")) %>% group_by(Country, Year, Sex, Marital.status) %>% 
  summarise(meanshare = mean(value)) %>% unite(SexMSVar, Marital.status, Sex) %>% 
  select(-Country) %>% spread(SexMSVar, meanshare) 
{% endhighlight %}

