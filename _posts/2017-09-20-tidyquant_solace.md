---
title: 'Tidyquant Solace: Replication'
author: Linh Bun
date: '2017-09-20'
slug: tidyquant_solace
categories: ["R"]
tags: ["R Markdown", "plot", "regression"]
output:
  html_document:
    toc: yes
---



# Tidyquant of Solace


{% highlight r %}
# *** --------------------------------------------------------------------------*** #
## Step 0: Load Libraries ##
# *** --------------------------------------------------------------------------*** #
suppressMessages(library("tidyverse"))
suppressMessages(library("tidyquant"))
suppressMessages(library("scales"))
suppressMessages(library("ggbeeswarm"))
suppressMessages(library("hrbrthemes"))
suppressMessages(library("ggthemes"))
{% endhighlight %}


{% highlight r %}
# *** --------------------------------------------------------------------------*** #
## Step 1: Prepare for data  ##
# *** --------------------------------------------------------------------------*** #
# Get HVS data
# First, list the Mnemonics from FRED
tickers <- c("ETOTALUSQ176N",    # All housing units
             "EVACANTUSQ176N",   # Vacant
             "EYRVACUSQ176N",    # Year-round vacant
             "ERENTUSQ176N",     # Vacant for rent
             "ESALEUSQ176N",     # Vacant for sale
             "ERNTSLDUSQ176N",   # Vacant rented or sold
             "EOFFMARUSQ176N",   # Vacant held off market
             "EOCCUSEUSQ176N",   # Vacant occasional use
             "EUREUSQ176N",      # Vacant usual residence elsewhere
             "EOTHUSQ176N",      # Vacant other
             "ESEASONUSQ176N",   # Vacant seasonal
             "EOCCUSQ176N",      # Occupied
             "EOWNOCCUSQ176N",   # Owner occupied
             "ERNTOCCUSQ176N",   # Renter occupied
             "RRVRUSQ156N",      # Rental vacancy rate
             "RHVRUSQ156N"       # Homeowner vacancy rate
)

# Next, list human readable variable names
myvars <- c("All housing units",
            "Vacant",
            "Year-round vacant",
            "Vacant for rent",
            "Vacant for sale",
            "Vacant rented or sold",
            "Vacant held off market",
            "Vacant occasional use",
            "Vacant usual residence elsewhere",
            "Vacant other",
            "Vacant seasonal",
            "Occupied",
            "Owner occupied",
            "Renter occupied",
            "Rental vacancy rate",
            "Homeowner vacancy rate"
)
{% endhighlight %}


{% highlight r %}
# Create a lookup dataset
mylookup <- data.frame(symbol = tickers, var = myvars)
knitr::kable(mylookup)
{% endhighlight %}



|symbol         |var                              |
|:--------------|:--------------------------------|
|ETOTALUSQ176N  |All housing units                |
|EVACANTUSQ176N |Vacant                           |
|EYRVACUSQ176N  |Year-round vacant                |
|ERENTUSQ176N   |Vacant for rent                  |
|ESALEUSQ176N   |Vacant for sale                  |
|ERNTSLDUSQ176N |Vacant rented or sold            |
|EOFFMARUSQ176N |Vacant held off market           |
|EOCCUSEUSQ176N |Vacant occasional use            |
|EUREUSQ176N    |Vacant usual residence elsewhere |
|EOTHUSQ176N    |Vacant other                     |
|ESEASONUSQ176N |Vacant seasonal                  |
|EOCCUSQ176N    |Occupied                         |
|EOWNOCCUSQ176N |Owner occupied                   |
|ERNTOCCUSQ176N |Renter occupied                  |
|RRVRUSQ156N    |Rental vacancy rate              |
|RHVRUSQ156N    |Homeowner vacancy rate           |


{% highlight r %}
# *** --------------------------------------------------------------------------*** #
## Step 2: Pull data  ##
# *** --------------------------------------------------------------------------*** #

tickers %>% tq_get(get = "economic.data", 
                   from = "2001-04-01"   # we start from April 2001 due to break in HVS
) -> df
# Take a look:
knitr::kable(head(df))
{% endhighlight %}



|symbol        |date       |  price|
|:-------------|:----------|------:|
|ETOTALUSQ176N |2001-04-01 | 117786|
|ETOTALUSQ176N |2001-07-01 | 118216|
|ETOTALUSQ176N |2001-10-01 | 118635|
|ETOTALUSQ176N |2002-01-01 | 119061|
|ETOTALUSQ176N |2002-04-01 | 119483|
|ETOTALUSQ176N |2002-07-01 | 119909|



{% highlight r %}
# *** --------------------------------------------------------------------------*** #
## Step 3: Organize data  ##
# *** --------------------------------------------------------------------------*** #

df <- merge(df, mylookup, by = "symbol") %>% rename(value = price)
df %>% group_by(date) %>% 
  # create a share variable that is the ratio of units to All housing units
  mutate(share = value/value[var == "All housing units"])  %>% 
  ungroup() %>% arrange(date, -value) -> df
knitr::kable(tail(df, 16))
{% endhighlight %}



|symbol         |date       |    value|var                              |  share|
|:--------------|:----------|--------:|:--------------------------------|------:|
|ETOTALUSQ176N  |2017-07-01 | 136684.0|All housing units                | 1.0000|
|EOCCUSQ176N    |2017-07-01 | 119085.0|Occupied                         | 0.8712|
|EOWNOCCUSQ176N |2017-07-01 |  76146.0|Owner occupied                   | 0.5571|
|ERNTOCCUSQ176N |2017-07-01 |  42939.0|Renter occupied                  | 0.3141|
|EVACANTUSQ176N |2017-07-01 |  17599.0|Vacant                           | 0.1288|
|EYRVACUSQ176N  |2017-07-01 |  13580.0|Year-round vacant                | 0.0994|
|EOFFMARUSQ176N |2017-07-01 |   7401.0|Vacant held off market           | 0.0541|
|ESEASONUSQ176N |2017-07-01 |   4020.0|Vacant seasonal                  | 0.0294|
|EOTHUSQ176N    |2017-07-01 |   3944.0|Vacant other                     | 0.0289|
|ERENTUSQ176N   |2017-07-01 |   3551.0|Vacant for rent                  | 0.0260|
|EOCCUSEUSQ176N |2017-07-01 |   2231.0|Vacant occasional use            | 0.0163|
|ERNTSLDUSQ176N |2017-07-01 |   1379.0|Vacant rented or sold            | 0.0101|
|ESALEUSQ176N   |2017-07-01 |   1248.0|Vacant for sale                  | 0.0091|
|EUREUSQ176N    |2017-07-01 |   1226.0|Vacant usual residence elsewhere | 0.0090|
|RRVRUSQ156N    |2017-07-01 |      7.5|Rental vacancy rate              | 0.0001|
|RHVRUSQ156N    |2017-07-01 |      1.6|Homeowner vacancy rate           | 0.0000|


{% highlight r %}
# Make a caption:
mycaption <- "@lenkiefer Source: U.S. Bureau of the Census, Housing Inventory Estimate, retrieved from FRED, Federal Reserve Bank of St. Louis; https://fred.stlouisfed.org/series/EOFFMARUSQ176N, September 18, 2017."
# Wrap caption 90 characters:
mycaption <- paste0(strwrap(mycaption, 90), sep = "", collapse="\n")
{% endhighlight %}


{% highlight r %}
# Make plot
ggplot(data = 
         # We're only going to plot 3 series
         filter(df, var %in% c('Vacant for rent', 
                               "Vacant rented or sold", 
                               'Vacant for sale', 
                               'Vacant held off market')), 
       aes(x = date, y = share, label = var)) +
  # Plot the gray lines
  geom_path(data = filter(df, var %in% c('Vacant for rent', 
                                         "Vacant rented or sold", 
                                         'Vacant for sale', 
                                         'Vacant held off market')) %>%
              #overwrite the var variable with NULL so each line shows up on each panel.
              mutate(var = NULL),  
            inherit.aes = F, color = "darkgray",   # tell this plot to ignore the aes statement
            alpha = 0.5,                        # make the lines faint
            aes(x = date, y = share, group = symbol)) +
  geom_line(size = 1, color = "royalblue") + # draw the real plot
  #  guides(color = F) +                        # ditch 
  theme_minimal() +
  facet_wrap(~ var) +
  scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
  theme(plot.caption = element_text(hjust = 0), 
        strip.text = element_text(color = "royalblue") ) +
  labs(x = "", y = "",
       title = "U.S. Housing Inventory: Year-round vacant units as share of total housing stock",
       caption = mycaption)
{% endhighlight %}

![plot of chunk unnamed-chunk-7](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-7-1.png)


{% highlight r %}
# Make plot
ggplot(data = 
         # We're only going to plot 3 series
         filter(df, var %in% c('Vacant for rent', 
                               "Vacant rented or sold", 
                               'Vacant for sale', 
                               'Vacant held off market')), 
       aes(x = date, y = share, label = var)) +
  # Plot the gray lines
  geom_path(data = filter(df, var %in% c('Vacant for rent', 
                                         "Vacant rented or sold", 
                                         'Vacant for sale', 
                                         'Vacant held off market')) %>%
              #overwrite the var variable with NULL so each line shows up on each panel.
              mutate(var = NULL),  
            inherit.aes = F, color = "darkgray",   # tell this plot to ignore the aes statement
            alpha = 0.5,                        # make the lines faint
            aes(x = date, y = share, group = symbol)) +
  geom_line(size = 1, color = "orange") + # draw the real plot
  #  guides(color = F) +                        # ditch 
  theme_fivethirtyeight() +
  facet_wrap(~ var) +
  scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
  theme(plot.caption = element_text(hjust = 0), 
        strip.text = element_text(color = "blue") ) +
  labs(x = "", y = "",
       title = "U.S. Housing Inventory: Year-round vacant units as share of total housing stock",
       caption = mycaption)
{% endhighlight %}

![plot of chunk unnamed-chunk-8](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-8-1.png)


{% highlight r %}
# New plot
ggplot(data = 
         # We're only going to plot 3 series
         filter(df, var %in% c('Vacant for rent', 
                              "Vacant rented or sold", 
                              'Vacant for sale', 
                              'Vacant held off market')), 
       aes(x = date, y = share, label = var)) +
    # Plot the gray lines
    geom_path(data = filter(df, var %in% c('Vacant for rent', 
                                          "Vacant rented or sold", 
                                          'Vacant for sale', 
                                          'Vacant held off market')) %>%
                #overwrite the var variable with NULL so each line shows up on each panel.
                mutate(var = NULL),  
              inherit.aes = F, color = "white",   # tell this plot to ignore the aes statement
              alpha = 0.5,                        # make the lines faint
              aes(x = date, y = share, group = symbol)) +
    geom_line(size = 1, color = "royalblue") + # draw the real plot
    #  guides(color = F) +                        # ditch 
    # theme_minimal() +
    facet_wrap(~var) +
    scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
    theme(plot.caption = element_text(hjust = 0), 
          strip.text = element_text(color = "royalblue") ) +
    labs(x = "", y = "",
         title = "U.S. Housing Inventory: Year-round vacant units as share of total housing stock", 
         caption = mycaption) +
    theme_modern_rc(grid = "XY")
{% endhighlight %}

![plot of chunk unnamed-chunk-9](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-9-1.png)


## Alternative for Plotting with Background (All Data) Following [Plotting background data for groups with ggplot2](https://drsimonj.svbtle.com/plotting-background-data-for-groups-with-ggplot2)

{% highlight r %}
# working with background plot
df2 <- df %>%
    filter(var %in% c('Vacant for rent',
                      "Vacant rented or sold",
                      'Vacant for sale',
                      'Vacant held off market'))
df2_without_var <- df %>%
    filter(var %in% c('Vacant for rent',
                      "Vacant rented or sold",
                      'Vacant for sale',
                      'Vacant held off market')) %>%
    # mutate(var_grey = var) %>%
    select(-var)
# df2_var2 <- df %>%
#     filter(var %in% c('Vacant for rent',
#                       "Vacant rented or sold",
#                       'Vacant for sale',
#                       'Vacant held off market')) %>%
#     mutate(var2 = var)
{% endhighlight %}


{% highlight r %}
ggplot(data = df2) +
    # key change
      # added var_grey (= var) for grouping
        # no need to add new variable
        # can just use symbol variable
      # this helps distinguish the four variables (var)
      # otherwise, geom_line() will use all points, creating zig-zag of all four line combined
    geom_line(data = df2_without_var,
              # mapping = aes(x = date, y = share, group = var_grey),
              mapping = aes(x = date, y = share, group = symbol),
              alpha = 0.7,
              color = "grey") +
    geom_line(mapping = aes(x = date, y = share),
              size = 1,
              color = "royalblue") +
    facet_wrap(~ var) +
    theme_minimal() +
    scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
    theme(plot.caption = element_text(hjust = 0),
          strip.text = element_text()) +
    labs(x = "Year",
         y = "Vacant Units as Share of Total Housing Stock",
         title = "U.S. Housing Inventory: Year-Round",
         caption = mycaption)
{% endhighlight %}

![plot of chunk unnamed-chunk-11](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-11-1.png)

## Increasing DPI (Higher Resolution)

{% highlight r %}
ggplot(data = df2) +
    # key change
      # added var_grey (= var) for grouping
        # no need to add new variable
        # can just use symbol variable
      # this helps distinguish the four variables (var)
      # otherwise, geom_line() will use all points, creating zig-zag of all four line combined
    geom_line(data = df2_without_var,
              # mapping = aes(x = date, y = share, group = var_grey),
              mapping = aes(x = date, y = share, group = symbol),
              alpha = 0.7,
              color = "grey") +
    geom_line(mapping = aes(x = date, y = share),
              size = 1,
              color = "royalblue") +
    facet_wrap(~ var) +
    theme_minimal() +
    scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
    theme(plot.caption = element_text(hjust = 0),
          strip.text = element_text()) +
    labs(x = "Year",
         y = "Vacant Units as Share of Total Housing Stock",
         title = "U.S. Housing Inventory: Year-Round",
         caption = mycaption)
{% endhighlight %}

![plot of chunk unnamed-chunk-12](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-12-1.png)


{% highlight r %}
ggplot(data = df2) +
    # key change
      # added var_grey (= var) for grouping
        # no need to add new variable
        # can just use symbol variable
      # this helps distinguish the four variables (var)
      # otherwise, geom_line() will use all points, creating zig-zag of all four line combined
    geom_line(data = df2_without_var,
              # mapping = aes(x = date, y = share, group = var_grey),
              mapping = aes(x = date, y = share, group = symbol),
              alpha = 0.7,
              color = "grey") +
    geom_line(mapping = aes(x = date, y = share),
              size = 1,
              color = "orange") +
    facet_wrap(~ var) +
    theme_fivethirtyeight() +
    scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
    theme(plot.caption = element_text(hjust = 0),
          strip.text = element_text()) +
    labs(x = "",
         y = "",
         title = "U.S. Housing Inventory: Year-round vacant units as share of total housing stock",
         caption = mycaption)
{% endhighlight %}

![plot of chunk unnamed-chunk-13](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-13-1.png)

{% highlight r %}
ggplot(data = df2) +
    # key change
      # added var_grey (= var) for grouping
        # no need to add new variable
        # can just use symbol variable
      # this helps distinguish the four variables (var)
      # otherwise, geom_line() will use all points, creating zig-zag of all four line combined
    geom_line(data = df2_without_var,
              # mapping = aes(x = date, y = share, group = var_grey),
              mapping = aes(x = date, y = share, group = symbol),
              alpha = 0.5,
              color = "white") +
    geom_line(mapping = aes(x = date, y = share),
              size = 1,
              color = "royalblue") +
    facet_wrap(~ var) +
    theme_modern_rc(grid = "XY") +
    scale_y_continuous(labels = scales::percent, breaks = seq(0, 1, .01)) +
    theme(plot.caption = element_text(hjust = 0),
          strip.text = element_text()) +
    labs(x = "Year",
         y = "Vacant Units as Share of Total Housing Stock",
         title = "U.S. Housing Inventory: Year-Round",
         caption = mycaption)
{% endhighlight %}

![plot of chunk unnamed-chunk-14](/figure/./_posts/2017-09-20-tidyquant_solace/unnamed-chunk-14-1.png)
