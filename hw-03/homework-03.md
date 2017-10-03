# hw-03
Melanie Ang  
September 27, 2017  
 
# Homework 3
In this homework, I experiment with dplyr (to manipulate data), kable and ggplot (to visualize data). Click [here](http://stat545.com/hw03_dplyr-and-more-ggplot2.html) to see the assignment on the STAT545 homework on web.

Again, check out the **tips** sprinkled through the documents for some of the things I've learned in this process of completing hwk-03.

## Step 1: Download packages

```r
library(gapminder) # our data set
library(tidyverse) # dplyr and ggplot2

# for kable
library(knitr)
library(kableExtra)
```

## Task menu
I've picked four task from the selection on the [task section of the website](http://stat545.com/hw03_dplyr-and-more-ggplot2.html#task-menu) 

###1. Get the maximum and minimum of GDP per capita for all continents.


```r
# Display in table
# Use dplyr functions to select, group by, summarize and display in kable

gapminder %>%
  select(continent, gdpPercap) %>%  # subset only relevant columns so its neater
  group_by(continent) %>%           # group according to continents
  summarize(min_gdppercap=min(gdpPercap), 
            max_gdppercap=max(gdpPercap)) %>% 
  knitr::kable(format = "markdown")          # display in kable
```



|continent | min_gdppercap| max_gdppercap|
|:---------|-------------:|-------------:|
|Africa    |      241.1659|      21951.21|
|Americas  |     1201.6372|      42951.65|
|Asia      |      331.0000|     113523.13|
|Europe    |      973.5332|      49357.19|
|Oceania   |    10039.5956|      34435.37|

**Tip 1/Question: Interestingly, my knitr::kable tables works in R but aren't showing up in GitHub, until I add in format = "markdown"
Is anyone else having this issue? I found a help me file from a few years ago [here](https://github.com/STAT545-UBC/Discussion/issues/136)**


Taking the same function as above, I pipe it into a geom point plot
geom_point() for continuous x and continuous y but I'm using this just to visualize the data. I plot using 3 different plots: points, boxplot and violin!


```r
# Plot it!
# There are multiple ways to do this. I could pipe contents into a ggplot for example. In this case, I chose to store value in a variable and call that variable later.

gapminder %>%
  select(continent, gdpPercap) %>%  # subset only relevant columns so its neater
  group_by(continent) %>%           # group according to continents
  summarize(min_gdppercap = min(gdpPercap),
            max_gdppercap = max(gdpPercap)) %>% # piping it into a ggplot
  ggplot(aes(x = continent)) +
    geom_point(aes(y=min_gdppercap), colour = "blue") +
    geom_point(aes(y=max_gdppercap), colour = "red") +
    labs(y = "GDP per Cap") +
    ggtitle("Minimum and maximum GDP per country") +
  theme_bw()
```

![](homework-03_files/figure-html/question 1 display plot-1.png)<!-- -->

```r
# Now lets try a boxplot
# Boxplots are usually used for discrete x (like continent) and continuous y
gapminder %>%
  select(continent, gdpPercap) %>%  # subset only relevant columns so its neater
  ggplot(aes(x = continent, y = gdpPercap)) +
    geom_boxplot()
```

![](homework-03_files/figure-html/question 1 display plot-2.png)<!-- -->

```r
# Trying out a geom_violin() plot also for discrete x and continuous y because why not!
gapminder %>%
  select(continent, gdpPercap) %>%  # subset only relevant columns so its neater
  group_by(continent) %>%           # group according to continents
  ggplot() +
    geom_violin(aes(x=continent, y = gdpPercap))
```

![](homework-03_files/figure-html/question 1 display plot-3.png)<!-- -->


I could get fancy here and customize the plot - one of the things I love about ggplot!

**Tip 2: Further customization of your ggplots using ggthemes!**  
I love customizing with colours and fonts etc. 
There are some prebuilt ggplot2 themes. [Click here](http://ggplot2.tidyverse.org/reference/ggtheme.html)
You can also use the ggthemes package for additional themes. Check out the [ggtheme](https://github.com/jrnold/ggthemes) github for info on more pre-built themes.


###2. Look at the spread of GDP per capita within the continents
Within spread, I chose to look at mean, standard deviation, range and median. I create a kable to display my plots by continents below.



```r
spread <- gapminder %>%
  group_by(continent) %>% 
  summarize(mean_gdp = mean(gdpPercap), 
            sd_gdp = sd(gdpPercap), 
            median_gdp = median(gdpPercap),
            max_gdp = max(gdpPercap),
            min_gdp = min(gdpPercap))

spread %>%
  kable(format = "html", 
  col.names = c("Continent", "Mean GDP", "Standard Deviation", "Median", "Maximum", "Minimum"), 
  caption = "Spread of GDP within continents of the Gapminder dataset") %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;"><caption>Spread of GDP within continents of the Gapminder dataset</caption>
 <thead><tr><th style="text-align:left;"> Continent </th>
   <th style="text-align:right;"> Mean GDP </th>
   <th style="text-align:right;"> Standard Deviation </th>
   <th style="text-align:right;"> Median </th>
   <th style="text-align:right;"> Maximum </th>
   <th style="text-align:right;"> Minimum </th>
  </tr></thead><tbody><tr><td style="text-align:left;"> Africa </td>
   <td style="text-align:right;"> 2193.755 </td>
   <td style="text-align:right;"> 2827.930 </td>
   <td style="text-align:right;"> 1192.138 </td>
   <td style="text-align:right;"> 21951.21 </td>
   <td style="text-align:right;"> 241.1659 </td>
  </tr><tr><td style="text-align:left;"> Americas </td>
   <td style="text-align:right;"> 7136.110 </td>
   <td style="text-align:right;"> 6396.764 </td>
   <td style="text-align:right;"> 5465.510 </td>
   <td style="text-align:right;"> 42951.65 </td>
   <td style="text-align:right;"> 1201.6372 </td>
  </tr><tr><td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 7902.150 </td>
   <td style="text-align:right;"> 14045.373 </td>
   <td style="text-align:right;"> 2646.787 </td>
   <td style="text-align:right;"> 113523.13 </td>
   <td style="text-align:right;"> 331.0000 </td>
  </tr><tr><td style="text-align:left;"> Europe </td>
   <td style="text-align:right;"> 14469.476 </td>
   <td style="text-align:right;"> 9355.213 </td>
   <td style="text-align:right;"> 12081.749 </td>
   <td style="text-align:right;"> 49357.19 </td>
   <td style="text-align:right;"> 973.5332 </td>
  </tr><tr><td style="text-align:left;"> Oceania </td>
   <td style="text-align:right;"> 18621.609 </td>
   <td style="text-align:right;"> 6358.983 </td>
   <td style="text-align:right;"> 17983.304 </td>
   <td style="text-align:right;"> 34435.37 </td>
   <td style="text-align:right;"> 10039.5956 </td>
  </tr></tbody></table>

```r
# not sure I'm a fan of the captioning font..
```

Next we plot it! I do it through 3 methods: points, boxplot and violin


```r
# Option 1 - display specifically mean, SD and median
gapminder %>%
  group_by(continent) %>% 
  summarize(mean_gdp = mean(gdpPercap), 
            sd_gdp = sd(gdpPercap), 
            median_gdp = median(gdpPercap)) %>% 
  ggplot(aes(x = continent)) +                  # setting continent as base x axis
  geom_point(aes(y = mean_gdp), colour = "purple") + # change colour
  geom_point(aes(y = sd_gdp), colour = "turquoise", shape = 3) + # change shape
  geom_point(aes(y = median_gdp), colour = "orange", size = 5) + # change size
  ggtitle("Mean, Standard Deviation and Median \nof GDP per Capita on Different Continents") +
  labs(y = "GDP per cap") 
```

![](homework-03_files/figure-html/question 2 plot-1.png)<!-- -->

```r
# Option 2 - boxplot spread
gapminder %>%
  ggplot(aes(x = continent, y = gdpPercap)) +
  geom_boxplot(aes(fill = continent), show.legend = FALSE) +
  labs(x = "Continent", y = "GDP per capita", title = "Spread of GDP in each continent") +
  theme_minimal()
```

![](homework-03_files/figure-html/question 2 plot-2.png)<!-- -->

###3. How is life expectancy changing over time on different continents?

To answer this question, I look at the life expectancy by continent and year, and compare 1952 to 2007 (first and last year).


```r
# summarize and calculate difference between 2007 and 1952
# plot in kable

gapminder %>% 
  group_by(continent, year) %>% 
  summarize(mean_lifeExp = mean(lifeExp)) %>%
  summarize(change_mean_lifeExp = mean_lifeExp[year == 2007] - mean_lifeExp[year ==1952]) %>%
  kable(col.names = c("Continent", "Mean Life Expectancy"), format = "html", caption = "Increases in life expectancy from 1952-2007") 
```

<table>
<caption>Increases in life expectancy from 1952-2007</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Continent </th>
   <th style="text-align:right;"> Mean Life Expectancy </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:right;"> 15.67054 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Americas </td>
   <td style="text-align:right;"> 20.32828 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:right;"> 24.41409 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Europe </td>
   <td style="text-align:right;"> 13.24010 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oceania </td>
   <td style="text-align:right;"> 11.46450 </td>
  </tr>
</tbody>
</table>

Lets plot it! Using a line plot!


```r
gapminder %>% 
  group_by(continent, year) %>%
  summarize(mean_lifeExp = mean(lifeExp)) %>%
  ggplot(aes(x=year, y=mean_lifeExp)) +
  geom_line(aes(colour = continent)) +
  scale_color_discrete("Continent") +
  labs(x = "Year",
       y = "Mean Life Expectancy (years)",
       title = "Changes in life expectancy on each continent") +
  theme_bw(base_size = 10) # change font size
```

![](homework-03_files/figure-html/question 3 plot-1.png)<!-- -->

###4. Find countries with interesting stories. Open-ended and, therefore, hard. Promising but unsuccessful attempts are encouraged. This will generate interesting questions to follow up on in class.

I'm originally from Singapore, this island in SE Asia that got its independence from Malaysia in the late 1950s. I'm curious about the statistics of these countries, how did the population and GDP per capita change following independence.

```r
# filtering for only Singapore and Malaysia
Malaysia_SGP <- gapminder %>%
  filter(country %in% c("Singapore", "Malaysia")) 

# plotting using 2 different y axis in order to compare pop and GDP
Malaysia_SGP %>%
  ggplot(aes(x = year))+
  geom_line(aes(y = pop), colour = "blue") +
  geom_line(aes(y = gdpPercap), colour = "red") +
  scale_y_continuous(sec.axis = sec_axis(~.*5, name = "GDP per Capita")) +
  facet_wrap(~country)
```

![](homework-03_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

**Question: If I add 2 separate y axis, I'm confused how to get the legend to show specifying that blue line shows population and red line shows GDP per capita. Is there a way to label this manually? I'll seek help in office hour, will update this with answer, or if anyone knows, feel free to post. Thanks! **


The GDP per capita growth is dwarf by population, so I plot it in its own separate graph (below).

```r
Malaysia_SGP %>%
  ggplot(aes(x = year))+
  geom_line(aes(y = gdpPercap, colour = country))
```

![](homework-03_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

Conclusion: Population of Malaysia is alot larger than Singapore. GDP per capita of the 2 countries were similar in the 1950s, but began diverging through the 1960s onwards.


## Report on my progress

I found this assignment more time consuming than others, perhaps because of the open ended nature. I'm realizing all the customizability of ggplots, the sky is really the limit. I'm loving the [ggplot cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf). Also, as my thesis is largely focused on mappings, I'm interested in learning more about using geom_map() and coord_map() functions - I've been largely using rasters to plot my results. Not sure if we will do any mapping in this course. If anyone has any resources they've found useful for maps, please feel free to leave them in the [issues](https://github.com/angmelanie/STAT545-hw-Ang-Melanie/issues) tab. I'll love you forever.

![](https://media.giphy.com/media/uYffljMqX1EHe/giphy.gif)


