---
layout: post
title:  "A Tufte-style Jekyll blog powered by servr and knitr"
categories: [jekyll, rstats]
tags: [knitr, servr, httpuv, websocket]
---

<span class='newthought'>This post</span> demonstrates [my fork](http://github.com/cpsievert/knitr-jekyll) of Yihui's [knitr-jekyll](https://github.com/yihui/knitr-jekyll) which tweaks the default layout to resemble [tufte-jekyll](https://github.com/clayh53/tufte-jekyll). As Yihui mentions in his [knitr-jekyll blog post](http://yihui.name/knitr-jekyll/2014/09/jekyll-with-knitr.html) (which I _highly_ recommend reading), GitHub Pages does not support arbitrary Jekyll plugins, but I've managed to remove tufte-jekyll's dependency on custom plugins via custom [knitr output hooks](http://yihui.name/knitr/hooks/). Not only does this allow GitHub Pages to build and host this template automagically, but it also fixes [tufte-jekyll's problem with figure paths](https://github.com/clayh53/tufte-jekyll#which-brings-me-to-sorrow-and-shame). 

The rest of this post shows you how to use these custom hooks and some other useful things specific to this template (at some point, you might also want the [source for this post](https://raw.githubusercontent.com/cpsievert/knitr-jekyll/gh-pages/_source/2015-04-20-jekyll-tufte-servr.Rmd))



### Figures

By default, the `fig.width` [chunk option](http://yihui.name/knitr/options/) is equal to 7 inches. Assuming the zoom of your browser window is at 100%, that translates to about 3/4 of the textwidth.


{% highlight r %}
library(ggplot2)
p <- ggplot(diamonds, aes(carat)) 
p + geom_histogram()
{% endhighlight %}

<span class='marginnote'>Figure 1: A nice plot that is not quite wide enough. Note that this figure caption was created using the `fig.cap` chunk option</span><img class='fullwidth' src='/knitr-jekyll/figure/2015-04-20-jekyll-tufte-servr/skinny-1.png'/>

If we increase `fig.width` to a ridiculous number, say 20 inches, it will still be constrained to the text width, even by changing `fig.width` to 20 inches. 


{% highlight r %}
p + geom_histogram(aes(y = ..density..))
{% endhighlight %}

<span class='marginnote'>Figure 2: The `fig.height` for this chunk is same as Figure 1, but the `fig.width` is now 20. Since the width is constrained by the text width, the figure is shrunken quite a bit.</span><img class='fullwidth' src='/knitr-jekyll/figure/2015-04-20-jekyll-tufte-servr/wide-1.png'/>

By constraining the figure width, it will ensure that figure captions (set via `fig.cap`) appear correctly in the side margin. If you don't want to restrict the final figure width, set the `fig.fullwidth` chunk option equal to `TRUE`. In this case, the figure caption is placed in the side margin below the figure.


{% highlight r %}
p + geom_point(aes(y = price), alpha = 0.2) + 
  facet_wrap(~cut, nrow = 1, scales = "free_x") +
  geom_smooth(aes(y = price, fill = cut))
{% endhighlight %}

<div><img class='fullwidth' src='/knitr-jekyll/figure/2015-04-20-jekyll-tufte-servr/full-1.png'/></div><p><span class='marginnote'>Figure 3: Full width plot</span></p>

To place figures in the margin, set the `fig.margin` chunk option equal to `TRUE`.


{% highlight r %}
tmp <- tempfile()
user <- "http://user2014.stat.ucla.edu/images/useR-large.png"
download.file(user, tmp)
img <- png::readPNG(tmp)
plot(0:1, type = 'n', xlab = "", ylab = "")
lim <- par()
rasterImage(img, lim$usr[1], lim$usr[3], lim$usr[2], lim$usr[4])
{% endhighlight %}

<span class='marginnote'><img class='fullwidth' src='/knitr-jekyll/figure/2015-04-20-jekyll-tufte-servr/margin-1.png'/>Figure 4: useR logo</span>

{% highlight r %}
unlink(tmp)
{% endhighlight %}

## Sizing terminal output
 
The default `R` terminal output width is 80, which is a bit too big for the styling of this blog, but a width of 55 works pretty well:


{% highlight r %}
options(width = 55, digits = 3)
(x <- rnorm(40))
{% endhighlight %}



{% highlight text %}
##  [1]  1.7884  0.8210  0.3601  1.0294  0.7522  2.2889
##  [7]  0.4871  0.6038 -1.4693 -0.8763 -0.5079  1.5787
## [13] -1.3754  0.4881  0.2502 -0.1419 -0.2551  0.2767
## [19] -1.0937 -0.4883  0.4633  0.3220 -0.2674  0.3528
## [25] -0.9748  1.1646 -0.0718  0.3706  0.2746  0.6826
## [31] -0.1077  0.6084 -0.3765 -0.4847  0.8459  1.7309
## [37] -0.6345 -0.5931  0.6349 -1.8199
{% endhighlight %}

## Mathjax

If you want inline math rendering, put `$$ math $$` inline. For example, $$ \Gamma(\alpha) = (\alpha - 1)!$$. If you want it on it's own line, do something like:

{% highlight latex %}
$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}.
$$
{% endhighlight %}

which results in

$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}.
$$

## Margin notes

<span class='marginnote'> 
<img class="fullwidth" src="http://i.imgur.com/NCMxz5G.gif">
Much margin. So excite.
</span>

Put stuff in the side margin using the `<span>` HTML tag with a class of 'marginnote':

{% highlight html %}
<span class='marginnote'> 
  Anything here will appear in side margin 
</span>
{% endhighlight %}

Another (less cute) example of margin notes is to add a table caption. In fact, the figure captions above are just margin notes.

<span class='marginnote'> 
Table 1: Output from a simple linear regression in tabular form.
</span>


{% highlight text %}
## Error in loadNamespace(name): there is no package called 'broom'
{% endhighlight %}


## Sidenotes

Similar to a 'marginnote' is a 'sidenote'<sup class='sidenote-number'> 1 </sup> which works like this

<span class='sidenote'>
  <sup class='sidenote-number'> 1 </sup> 
  Sidenotes are kind of like footnotes that appear in the side margin.
</span> 

{% highlight html %}
<sup class='sidenote-number'> 1 </sup>
<span class='sidenote'>
  <sup class='sidenote-number'> 1 </sup> 
  Sidenotes are kind of like footnotes that appear in the side margin.
</span> 
{% endhighlight %}

Unfortunately, this is a lot of HTML markup, but of course[^2], you can also do footnotes, so that might be a better option.

[^2]: I hate it when people say "of course" as though this is obvious everyone.

## Contact me

If you find any issues or want to help improve the implementation, [please let me know](https://github.com/cpsievert/knitr-jekyll/issues/new)!

## Session Information


{% highlight text %}
## R version 3.2.1 (2015-06-18)
## Platform: x86_64-apple-darwin13.4.0 (64-bit)
## Running under: OS X 10.10.5 (Yosemite)
## 
## locale:
## [1] C
## 
## attached base packages:
## [1] methods   stats     graphics  grDevices utils     datasets 
## [7] base     
## 
## other attached packages:
## [1] mgcv_1.8-6    nlme_3.1-120  ggplot2_1.0.1
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.0      codetools_0.2-11 lattice_0.20-31 
##  [4] digest_0.6.8     MASS_7.3-40      grid_3.2.1      
##  [7] plyr_1.8.3       gtable_0.1.2     formatR_1.2     
## [10] magrittr_1.5     evaluate_0.7     scales_0.2.5    
## [13] stringi_0.5-5    reshape2_1.4.1   Matrix_1.2-1    
## [16] proto_0.3-10     tools_3.2.1      servr_0.2       
## [19] stringr_1.0.0    munsell_0.4.2    httpuv_1.3.2    
## [22] colorspace_1.2-6 knitr_1.10.5
{% endhighlight %}
