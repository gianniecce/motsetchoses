---
layout: post
title:  "Retrieve Days and Dates in R"
categories: [jekyll, rstats]
tags: [knitr, servr, httpuv, websocket]
---

In this post, we will see how to handle dates in `R`. 

Copy-paste the following lines of codes with the dataset. 


{% highlight r %}
# the data 
dta = structure(list(id = c("1001893912015", "1001893912015", "10067992412015", 
"10067992412015", "10079545612015", "10079545612015", "10079545622015", 
"10079545622015", "1012460812015", "1012460812015"), age = c(36, 
36, 54, 54, 60, 60, 22, 22, 30, 30), DiaryDay = c(13615862400, 
13616035200, 13617504000, 13617849600, 13619664000, 13619923200, 
13619664000, 13619923200, 13633574400, 13633833600)), .Names = c("id", 
"age", "DiaryDay"), row.names = c(NA, 10L), class = "data.frame")
{% endhighlight %}

Let's have a look at it 


{% highlight r %}
head(dta) 
{% endhighlight %}



{% highlight text %}
##               id age    DiaryDay
## 1  1001893912015  36 13615862400
## 2  1001893912015  36 13616035200
## 3 10067992412015  54 13617504000
## 4 10067992412015  54 13617849600
## 5 10079545612015  60 13619664000
## 6 10079545612015  60 13619923200
{% endhighlight %}

We can see that there is 3 variables in this dataset - a personal identifier `id`, the age and the `DiaryDay`. Diary day is the vector of interest where the date as been stored. 

There exist an international standard for dates. As explained 

> "The purpose of this standard is to provide an unambiguous and well-defined method of representing dates and times."  

[][https://en.wikipedia.org/wiki/ISO_8601] 


This dataset as been imported from `SPSS` and as a special `origin`. In order to get the dates right, we have to indicate what is the origin that the software as taken. 

In order to retreive dates in R, we use the function `as.Dates`. 
Looking at the documentation, we find that 
> Matlab's origin is 719529 days before ours 

However, nothing is specified for `SPSS`. After a bit of research, we found that SPSS takes as origin `1582-10-14` [here][http://www.uic.edu/depts/accc/software/isodates/datepgm.html]. 

The Uni of York propose then a simply command 


{% highlight r %}
spss2date <- function(x) as.Date(x/86400, origin = "1582-10-14")
{% endhighlight %}

They divided by `86400` because they transform "turn seconds into days" [MathYork][http://scs.math.yorku.ca/index.php/R:_Importing_dates_from_SPSS]. 


Let us try 

{% highlight r %}
dta$DiaryDayConverted = spss2date(dta$DiaryDay)
{% endhighlight %}

We get 


{% highlight r %}
dta 
{% endhighlight %}



{% highlight text %}
##                id age    DiaryDay DiaryDayConverted
## 1   1001893912015  36 13615862400        2014-04-03
## 2   1001893912015  36 13616035200        2014-04-05
## 3  10067992412015  54 13617504000        2014-04-22
## 4  10067992412015  54 13617849600        2014-04-26
## 5  10079545612015  60 13619664000        2014-05-17
## 6  10079545612015  60 13619923200        2014-05-20
## 7  10079545622015  22 13619664000        2014-05-17
## 8  10079545622015  22 13619923200        2014-05-20
## 9   1012460812015  30 13633574400        2014-10-25
## 10  1012460812015  30 13633833600        2014-10-28
{% endhighlight %}

Now we want to retreive the days of the week. 

In order to do so, we need to use the library `lubridate`. In this library, you will find the function `wday`. 


{% highlight r %}
library(lubridate)
{% endhighlight %}



{% highlight text %}
## Warning: package 'lubridate' was built under R version 3.2.3
{% endhighlight %}



{% highlight text %}
## Loading required package: methods
{% endhighlight %}



{% highlight r %}
dta$weekdayvar = wday(dta$DiaryDayConverted, label = T) 
dta
{% endhighlight %}



{% highlight text %}
##                id age    DiaryDay DiaryDayConverted weekdayvar
## 1   1001893912015  36 13615862400        2014-04-03      Thurs
## 2   1001893912015  36 13616035200        2014-04-05        Sat
## 3  10067992412015  54 13617504000        2014-04-22       Tues
## 4  10067992412015  54 13617849600        2014-04-26        Sat
## 5  10079545612015  60 13619664000        2014-05-17        Sat
## 6  10079545612015  60 13619923200        2014-05-20       Tues
## 7  10079545622015  22 13619664000        2014-05-17        Sat
## 8  10079545622015  22 13619923200        2014-05-20       Tues
## 9   1012460812015  30 13633574400        2014-10-25        Sat
## 10  1012460812015  30 13633833600        2014-10-28       Tues
{% endhighlight %}



