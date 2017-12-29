---
title: "Plotting background data for groups with ggplot2"
author: "Linh Bun"
date: '2017-09-20'
categories: ["R"]
tags: ["R Markdown", "plot", "regression"]
output: 
  html_document:
    toc: TRUE
---



# Plotting


{% highlight r %}
library("ggplot2")
suppressMessages(library("tidyverse"))
library("hrbrthemes")
library("ggthemes")
{% endhighlight %}


{% highlight r %}
ggplot(iris, aes(x = Sepal.Width)) +
    geom_histogram()
{% endhighlight %}



{% highlight text %}
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
{% endhighlight %}

![plot of chunk unnamed-chunk-2](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-2-1.png)


{% highlight r %}
ggplot(iris, aes(x = Sepal.Width, fill = Species)) +
    geom_histogram() +
    theme_minimal()
{% endhighlight %}



{% highlight text %}
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
{% endhighlight %}

![plot of chunk unnamed-chunk-3](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-3-1.png)


{% highlight r %}
ggplot(iris, aes(x = Sepal.Width)) +
    geom_histogram() +
    facet_wrap(~ Species) +
    theme_minimal()
{% endhighlight %}



{% highlight text %}
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
{% endhighlight %}

![plot of chunk unnamed-chunk-4](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-4-1.png)

## General Idea

{% highlight r %}
ggplot(data = full_data_frame,
       mapping = aes(...)) +
    geom_*(data = data_frame_without_grouping_var,
           mapping = aes(...),
           colour/fill = "neutral_color") +
    geom_*() +
    facet_wrap(~ grouping_var) +
    theme_*()
{% endhighlight %}

### Alternative

{% highlight r %}
ggplot(data = full_data_frame) +
    geom_*(data = data_frame_without_grouping_var,
           mapping = aes(...),
           colour/fill = "neutral_color") +
    geom_*(mapping = aes(..., group = grouping_var)) +
    facet_wrap(~ grouping_var) +
    theme_minimal()
{% endhighlight %}


{% highlight r %}
d <- iris        # Full data set
d_bg <- d[, -5]  # Background Data - full without the 5th column (Species)
{% endhighlight %}


{% highlight r %}
head(d)
{% endhighlight %}



{% highlight text %}
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
{% endhighlight %}



{% highlight r %}
tail(d)
{% endhighlight %}



{% highlight text %}
##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
## 145          6.7         3.3          5.7         2.5 virginica
## 146          6.7         3.0          5.2         2.3 virginica
## 147          6.3         2.5          5.0         1.9 virginica
## 148          6.5         3.0          5.2         2.0 virginica
## 149          6.2         3.4          5.4         2.3 virginica
## 150          5.9         3.0          5.1         1.8 virginica
{% endhighlight %}


{% highlight r %}
d
{% endhighlight %}



{% highlight text %}
##     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
## 1            5.1         3.5          1.4         0.2     setosa
## 2            4.9         3.0          1.4         0.2     setosa
## 3            4.7         3.2          1.3         0.2     setosa
## 4            4.6         3.1          1.5         0.2     setosa
## 5            5.0         3.6          1.4         0.2     setosa
## 6            5.4         3.9          1.7         0.4     setosa
## 7            4.6         3.4          1.4         0.3     setosa
## 8            5.0         3.4          1.5         0.2     setosa
## 9            4.4         2.9          1.4         0.2     setosa
## 10           4.9         3.1          1.5         0.1     setosa
## 11           5.4         3.7          1.5         0.2     setosa
## 12           4.8         3.4          1.6         0.2     setosa
## 13           4.8         3.0          1.4         0.1     setosa
## 14           4.3         3.0          1.1         0.1     setosa
## 15           5.8         4.0          1.2         0.2     setosa
## 16           5.7         4.4          1.5         0.4     setosa
## 17           5.4         3.9          1.3         0.4     setosa
## 18           5.1         3.5          1.4         0.3     setosa
## 19           5.7         3.8          1.7         0.3     setosa
## 20           5.1         3.8          1.5         0.3     setosa
## 21           5.4         3.4          1.7         0.2     setosa
## 22           5.1         3.7          1.5         0.4     setosa
## 23           4.6         3.6          1.0         0.2     setosa
## 24           5.1         3.3          1.7         0.5     setosa
## 25           4.8         3.4          1.9         0.2     setosa
## 26           5.0         3.0          1.6         0.2     setosa
## 27           5.0         3.4          1.6         0.4     setosa
## 28           5.2         3.5          1.5         0.2     setosa
## 29           5.2         3.4          1.4         0.2     setosa
## 30           4.7         3.2          1.6         0.2     setosa
## 31           4.8         3.1          1.6         0.2     setosa
## 32           5.4         3.4          1.5         0.4     setosa
## 33           5.2         4.1          1.5         0.1     setosa
## 34           5.5         4.2          1.4         0.2     setosa
## 35           4.9         3.1          1.5         0.2     setosa
## 36           5.0         3.2          1.2         0.2     setosa
## 37           5.5         3.5          1.3         0.2     setosa
## 38           4.9         3.6          1.4         0.1     setosa
## 39           4.4         3.0          1.3         0.2     setosa
## 40           5.1         3.4          1.5         0.2     setosa
## 41           5.0         3.5          1.3         0.3     setosa
## 42           4.5         2.3          1.3         0.3     setosa
## 43           4.4         3.2          1.3         0.2     setosa
## 44           5.0         3.5          1.6         0.6     setosa
## 45           5.1         3.8          1.9         0.4     setosa
## 46           4.8         3.0          1.4         0.3     setosa
## 47           5.1         3.8          1.6         0.2     setosa
## 48           4.6         3.2          1.4         0.2     setosa
## 49           5.3         3.7          1.5         0.2     setosa
## 50           5.0         3.3          1.4         0.2     setosa
## 51           7.0         3.2          4.7         1.4 versicolor
## 52           6.4         3.2          4.5         1.5 versicolor
## 53           6.9         3.1          4.9         1.5 versicolor
## 54           5.5         2.3          4.0         1.3 versicolor
## 55           6.5         2.8          4.6         1.5 versicolor
## 56           5.7         2.8          4.5         1.3 versicolor
## 57           6.3         3.3          4.7         1.6 versicolor
## 58           4.9         2.4          3.3         1.0 versicolor
## 59           6.6         2.9          4.6         1.3 versicolor
## 60           5.2         2.7          3.9         1.4 versicolor
## 61           5.0         2.0          3.5         1.0 versicolor
## 62           5.9         3.0          4.2         1.5 versicolor
## 63           6.0         2.2          4.0         1.0 versicolor
## 64           6.1         2.9          4.7         1.4 versicolor
## 65           5.6         2.9          3.6         1.3 versicolor
## 66           6.7         3.1          4.4         1.4 versicolor
## 67           5.6         3.0          4.5         1.5 versicolor
## 68           5.8         2.7          4.1         1.0 versicolor
## 69           6.2         2.2          4.5         1.5 versicolor
## 70           5.6         2.5          3.9         1.1 versicolor
## 71           5.9         3.2          4.8         1.8 versicolor
## 72           6.1         2.8          4.0         1.3 versicolor
## 73           6.3         2.5          4.9         1.5 versicolor
## 74           6.1         2.8          4.7         1.2 versicolor
## 75           6.4         2.9          4.3         1.3 versicolor
## 76           6.6         3.0          4.4         1.4 versicolor
## 77           6.8         2.8          4.8         1.4 versicolor
## 78           6.7         3.0          5.0         1.7 versicolor
## 79           6.0         2.9          4.5         1.5 versicolor
## 80           5.7         2.6          3.5         1.0 versicolor
## 81           5.5         2.4          3.8         1.1 versicolor
## 82           5.5         2.4          3.7         1.0 versicolor
## 83           5.8         2.7          3.9         1.2 versicolor
## 84           6.0         2.7          5.1         1.6 versicolor
## 85           5.4         3.0          4.5         1.5 versicolor
## 86           6.0         3.4          4.5         1.6 versicolor
## 87           6.7         3.1          4.7         1.5 versicolor
## 88           6.3         2.3          4.4         1.3 versicolor
## 89           5.6         3.0          4.1         1.3 versicolor
## 90           5.5         2.5          4.0         1.3 versicolor
## 91           5.5         2.6          4.4         1.2 versicolor
## 92           6.1         3.0          4.6         1.4 versicolor
## 93           5.8         2.6          4.0         1.2 versicolor
## 94           5.0         2.3          3.3         1.0 versicolor
## 95           5.6         2.7          4.2         1.3 versicolor
## 96           5.7         3.0          4.2         1.2 versicolor
## 97           5.7         2.9          4.2         1.3 versicolor
## 98           6.2         2.9          4.3         1.3 versicolor
## 99           5.1         2.5          3.0         1.1 versicolor
## 100          5.7         2.8          4.1         1.3 versicolor
## 101          6.3         3.3          6.0         2.5  virginica
## 102          5.8         2.7          5.1         1.9  virginica
## 103          7.1         3.0          5.9         2.1  virginica
## 104          6.3         2.9          5.6         1.8  virginica
## 105          6.5         3.0          5.8         2.2  virginica
## 106          7.6         3.0          6.6         2.1  virginica
## 107          4.9         2.5          4.5         1.7  virginica
## 108          7.3         2.9          6.3         1.8  virginica
## 109          6.7         2.5          5.8         1.8  virginica
## 110          7.2         3.6          6.1         2.5  virginica
## 111          6.5         3.2          5.1         2.0  virginica
## 112          6.4         2.7          5.3         1.9  virginica
## 113          6.8         3.0          5.5         2.1  virginica
## 114          5.7         2.5          5.0         2.0  virginica
## 115          5.8         2.8          5.1         2.4  virginica
## 116          6.4         3.2          5.3         2.3  virginica
## 117          6.5         3.0          5.5         1.8  virginica
## 118          7.7         3.8          6.7         2.2  virginica
## 119          7.7         2.6          6.9         2.3  virginica
## 120          6.0         2.2          5.0         1.5  virginica
## 121          6.9         3.2          5.7         2.3  virginica
## 122          5.6         2.8          4.9         2.0  virginica
## 123          7.7         2.8          6.7         2.0  virginica
## 124          6.3         2.7          4.9         1.8  virginica
## 125          6.7         3.3          5.7         2.1  virginica
## 126          7.2         3.2          6.0         1.8  virginica
## 127          6.2         2.8          4.8         1.8  virginica
## 128          6.1         3.0          4.9         1.8  virginica
## 129          6.4         2.8          5.6         2.1  virginica
## 130          7.2         3.0          5.8         1.6  virginica
## 131          7.4         2.8          6.1         1.9  virginica
## 132          7.9         3.8          6.4         2.0  virginica
## 133          6.4         2.8          5.6         2.2  virginica
## 134          6.3         2.8          5.1         1.5  virginica
## 135          6.1         2.6          5.6         1.4  virginica
## 136          7.7         3.0          6.1         2.3  virginica
## 137          6.3         3.4          5.6         2.4  virginica
## 138          6.4         3.1          5.5         1.8  virginica
## 139          6.0         3.0          4.8         1.8  virginica
## 140          6.9         3.1          5.4         2.1  virginica
## 141          6.7         3.1          5.6         2.4  virginica
## 142          6.9         3.1          5.1         2.3  virginica
## 143          5.8         2.7          5.1         1.9  virginica
## 144          6.8         3.2          5.9         2.3  virginica
## 145          6.7         3.3          5.7         2.5  virginica
## 146          6.7         3.0          5.2         2.3  virginica
## 147          6.3         2.5          5.0         1.9  virginica
## 148          6.5         3.0          5.2         2.0  virginica
## 149          6.2         3.4          5.4         2.3  virginica
## 150          5.9         3.0          5.1         1.8  virginica
{% endhighlight %}


{% highlight r %}
str(d)
{% endhighlight %}



{% highlight text %}
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
{% endhighlight %}


{% highlight r %}
ggplot(data = d,
       mapping = aes(x = Sepal.Width)) +
    geom_histogram(data = d_bg,
                   fill = "grey") +
    geom_histogram() +
    facet_wrap(~ Species)
{% endhighlight %}



{% highlight text %}
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
{% endhighlight %}

![plot of chunk unnamed-chunk-10](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-10-1.png)


{% highlight r %}
ggplot(data = d,
       mapping = aes(x = Sepal.Width, fill = Species)) +
    geom_histogram(data = d_bg,
                   fill = "grey",
                   alpha = 0.5) +
    geom_histogram(color = "black") +
    facet_wrap(~ Species) +
    guides(fill = FALSE) +
    theme_minimal()
{% endhighlight %}



{% highlight text %}
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
{% endhighlight %}

![plot of chunk unnamed-chunk-11](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-11-1.png)


{% highlight r %}
# d <- iris        # Full data set
# d_bg <- d[, -5]  # Background Data - full without the 5th column (Species)

ggplot(d, aes(x = Sepal.Width, fill = Species)) +
  geom_histogram(data = d_bg, fill = "grey", alpha = .5) +
  geom_histogram(colour = "black") +
  facet_wrap(~ Species) +
  guides(fill = FALSE) +  # to remove the legend
  theme_bw()              # for clean look overall
{% endhighlight %}



{% highlight text %}
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
{% endhighlight %}

![plot of chunk unnamed-chunk-12](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-12-1.png)

## point example

{% highlight r %}
ggplot(data = d,
       mapping = aes(x = Sepal.Width, y = Sepal.Length)) +
    geom_point(data = d_bg,
               color = "grey",
               alpha = 0.5) +
    geom_point() +
    facet_wrap(~ Species) +
    theme_minimal()
{% endhighlight %}

![plot of chunk unnamed-chunk-13](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-13-1.png)


{% highlight r %}
ggplot(d, aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point(data = d_bg, colour = "grey") +
  geom_point() + 
  facet_wrap(~ Species)
{% endhighlight %}

![plot of chunk unnamed-chunk-14](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-14-1.png)


{% highlight r %}
ggplot(data = d,
       mapping = aes(x = Sepal.Width, y = Sepal.Length, color = Species)) +
    geom_point(data = d_bg,
               color = "grey",
               alpha = 0.5) +
    geom_point() +
    facet_wrap(~ Species) +
    guides(color = FALSE) +
    theme_minimal()
{% endhighlight %}

![plot of chunk unnamed-chunk-15](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-15-1.png)


{% highlight r %}
ggplot(d, aes(x = Sepal.Width, y = Sepal.Length, colour = Species)) +
  geom_point(data = d_bg, colour = "grey", alpha = .2) +
  geom_point() + 
  facet_wrap(~ Species) +
  guides(colour = FALSE) +
  theme_bw()
{% endhighlight %}

![plot of chunk unnamed-chunk-16](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-16-1.png)

## more example

{% highlight r %}
library("nycflights13")
library(dplyr)

usa_map <- map_data("usa")
{% endhighlight %}



{% highlight text %}
## 
## Attaching package: 'maps'
{% endhighlight %}



{% highlight text %}
## The following object is masked from 'package:purrr':
## 
##     map
{% endhighlight %}



{% highlight r %}
airports <- read.csv("https://raw.githubusercontent.com/jpatokal/openflights/master/data/airports.dat", stringsAsFactors = FALSE, header = FALSE)
airports <- airports[, c(5, 7, 8)]
names(airports) <- c("code", "lat", "long")
orig <- airports %>% dplyr::rename(origin = code, long_o = long, lat_o = lat)
dest <- airports %>% dplyr::rename(dest = code, long_d = long, lat_d = lat)

d <- flights %>%
       left_join(orig) %>% 
       left_join(dest) %>% 
       filter(carrier %in% c("AS", "F9", "OO", "YV", "VX", "FL"))
{% endhighlight %}



{% highlight text %}
## Joining, by = "origin"
{% endhighlight %}



{% highlight text %}
## Joining, by = "dest"
{% endhighlight %}



{% highlight r %}
d_bg <- d %>% select(-carrier)

ggplot(d) +
  geom_polygon(data = usa_map, aes(long, lat, group = region)) +
  geom_segment(data = d_bg, colour = "grey", alpha = .7,
               aes(x = long_o, y = lat_o,
                   xend = long_d, yend = lat_d)) +
  geom_segment(aes(x = long_o, y = lat_o,
                   xend = long_d, yend = lat_d,
                   colour = carrier)) +
  facet_wrap(~ carrier) +
  guides(colour = FALSE) +
  theme_bw()
{% endhighlight %}

![plot of chunk unnamed-chunk-17](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-17-1.png)

### alternative

{% highlight r %}
ggplot(d) +
  geom_polygon(data = usa_map, aes(long, lat, group = region)) +
  geom_segment(data = d_bg, colour = "grey", alpha = .7,
               aes(x = long_o, y = lat_o,
                   xend = long_d, yend = lat_d)) +
  geom_segment(aes(x = long_o, y = lat_o,
                   xend = long_d, yend = lat_d,
                   colour = carrier)) +
  facet_wrap(~ carrier) +
  guides(colour = FALSE) +
  theme_minimal()
{% endhighlight %}

![plot of chunk unnamed-chunk-18](/figure/./_posts/2017-09-20-plot_background_group/unnamed-chunk-18-1.png)

