---
title: "Hello R Markdown"
author: "Frida Gomam"
date: 2015-07-23T21:13:14-05:00
categories: ["R"]
tags: ["R Markdown", "plot", "regression"]
---



# R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

You can embed an R code chunk like this:


{% highlight r %}
summary(cars)
{% endhighlight %}



{% highlight text %}
##      speed           dist    
##  Min.   : 4.0   Min.   :  2  
##  1st Qu.:12.0   1st Qu.: 26  
##  Median :15.0   Median : 36  
##  Mean   :15.4   Mean   : 43  
##  3rd Qu.:19.0   3rd Qu.: 56  
##  Max.   :25.0   Max.   :120
{% endhighlight %}



{% highlight r %}
fit <- lm(dist ~ speed, data = cars)
fit
{% endhighlight %}



{% highlight text %}
## 
## Call:
## lm(formula = dist ~ speed, data = cars)
## 
## Coefficients:
## (Intercept)        speed  
##      -17.58         3.93
{% endhighlight %}

# Including Plots

You can also embed plots. See Figure \@ref(fig:pie) for example:


{% highlight r %}
par(mar = c(0, 1, 0, 1))
pie(
  c(280, 60, 20),
  c('Sky', 'Sunny side of pyramid', 'Shady side of pyramid'),
  col = c('#0292D8', '#F7EA39', '#C4B632'),
  init.angle = -50, border = NA
)
{% endhighlight %}

![A fancy pie chart.](/figure/./_posts/2015-07-23-r-rmarkdown/pie-1.png)
