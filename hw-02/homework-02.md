# hw-02
Melanie Ang  
September 24, 2017  

## Homework 2:
Where we explore the gapminder dataset and use dplyr and ggplot2!
Please see my **tips** sprinkled through the document for fun things I've learned in the process of completing this homework assignment such as colour customization, adding graph titles, axis etc. - read on! :)

### Install gapminder and tidyverse packages

```r
install.packages("gapminder")
library(gapminder)
```


```r
install.packages("tidyverse")
library(tidyverse)
```

**Tip 1: To keep neat and tidy rmarkdown, add {r Install gapminder, results='hide', message = FALSE, warning = FALSE} to run the code but surpress the results and warning messages!**

### Exploring the gapminder dataset


```r
typeof(gapminder) # its a list
class(gapminder) # its a data frame
summary(gapminder) # it has 6 variables/columns
ncol(gapminder) # this also gives number of coumn
dim(gapminder) # it has 1704 rows
str(gapminder) # country: factor, continent: factor, year: integer, lifeExp: number, pop: integer, gdppercap: number

# In the above code chunk, I have turn results = 'hide' to keep the code neat and not display the results. See comments and below for answers.
```

#### Answers to questions:
* Is it a data.frame, a matrix, a vector, a list?
    + list
* What’s its class?
    + data frame
* How many variables/columns?
    + 6 variables/columns  
* How many rows/observations?
    + 1704 rows/observations
* Can you get these facts about “extent” or “size” in more than one way? Can you imagine different functions being useful in different contexts?
    + Yes, you can get information about size or extent from dim() or str()
    + dim() would give you information about size of dataframes
    + str() in addition to extent information, could also inform of the class of object and the data type of each column
* What data type is each variable?
    + country: factor
    + continent: factor
    + year: integer
    + lifeExp: number
    + pop: integer
    + gdppercap: number

## Exploring individual variables

### Categorical variables: country and continent
I have selected 2 categorical variables to evaluate! Country and continent

```r
# Country
# I'm using the head function just to keep things neat and tidy, to not list all 142 countries!
head(unique(gapminder$country)) # there are 142 different unique countries
```

```
## [1] Afghanistan Albania     Algeria     Angola      Argentina   Australia  
## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe
```

```r
gapminder %>%
  group_by(continent) %>% 
  summarize(n = n(), #n() is the number of observations
            ncountries = n_distinct(country))
```

```
## # A tibble: 5 x 3
##   continent     n ncountries
##      <fctr> <int>      <int>
## 1    Africa   624         52
## 2  Americas   300         25
## 3      Asia   396         33
## 4    Europe   360         30
## 5   Oceania    24          2
```

```r
# Continent
unique(gapminder$continent) # 5 continents (Asia, Europe, Africa, America, Oceania)
```

```
## [1] Asia     Europe   Africa   Americas Oceania 
## Levels: Africa Americas Asia Europe Oceania
```

### Quantitative variable : pop and year
I have selected 2 quantitative variables to evaluate! Population and year. Again, I have hidden the output of these code below (eval = FALSE) because it is very long, please see comments within for answers.


```r
# POPULATION
unique(gapminder$pop) # displays the possible values
median(gapminder$pop) #7,023,596
range(gapminder$pop) # returns the range 60,011 to 1,318,683,096
```

Summary function returns the same results as running median and range etc separately and it displays it nicely in summary table!

```r
# YEAR
summary(gapminder$year)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    1952    1966    1980    1980    1993    2007
```

## Explore plot types

It's plotting time! I love graphing, mostly for the customization features of ggplot. First up, let's do a scatterplot using year and population!

### Scatterplot with 2 quantitative



```r
ggplot(gapminder, 
      aes(x=year, y=pop)) +
      geom_point(size = 1.5, color = "cornflowerblue") + # customization
      ggtitle("Gapminder dataset - \nPopulation vs. Year")
```

![](homework-02_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

**Tip 2: To add title, layer this onto your ggplot: "+ ggtitle("Gapminder dataset - Population vs. Year")"**

### Plot quantitative variable - continent (histogram)



```r
ggplot(data = gapminder, aes(gapminder$continent)) + 
  geom_histogram(stat = "count", colour = "black", fill = "coral2", alpha = 0.2) +
  labs(x = "Continent", y = "Count") + # relabel axis 
  ggtitle("Histogram of Continents in Gapminder dataset")
```

![](homework-02_files/figure-html/Histogram-1.png)<!-- -->
  
**Tip 3: Do you like pretty colours like I do? Want to customize your graphs? Not just regular, boring blue but meet cornflower blue. To find a list of preset colours in R click [here](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf). You can also specify HEX colours by including them with a hashtag "#FFFFFF"**


### Plot of pop vs. country (without subset)

```r
ggplot(gapminder,
      aes(x = country, y = pop)) +
      geom_boxplot()
```

![](homework-02_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
This displays all the countries in the gapminder dataset, which is obviously too many to effectively display in boxplots. In the following step, I use the filter function to subset the data to plot

### Use filter() to subset the above dataset


```r
NAmericas <- filter(gapminder, country %in% c("Canada", "Mexico", "United States"))
ggplot(NAmericas,
      aes(x=country, y=pop)) +
      geom_boxplot(colour = "burlywood4", fill = "cadetblue", alpha = 0.5) + #making pretty graphs via fill, line and transparency customization
      ggtitle("North America's populations by countries - Gapminder dataset") +
      labs(x = "Country", y = "Population")
```

![](homework-02_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

**Tip 3: Relabel axis by adding "labs(x = "insert_text_here", y = "insert_text_here"), insert your own x and y labels**

### Adding it all together: filter, select, piping into ggplot

```r
gapminder %>%
  filter(country %in% c("Canada", "Mexico", "United States")) %>%
  select(country, lifeExp) %>% 
  ggplot(., aes(x=country, y=lifeExp)) +
  geom_boxplot(fill = "#0f738e", colour = "black", alpha = "0.8") +
  ggtitle("Life Expectancies in USA, Canada and Mexico") +
  labs(x = "Countries", y = "Life Expectancies (years)")
```

![](homework-02_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**Tip 4: I've also learned that to display plots from RMarkdown on GitHub you need to save RMarkdown file as a raw file (.md). Then also commit and push these images to GitHub. [click here](https://github.com/STAT540-UBC/Discussion/issues/11)**


## But I want to do more!


```r
filter(gapminder, country == c("Rwanda", "Afghanistan"))
```

```
## # A tibble: 12 x 6
##        country continent  year lifeExp      pop gdpPercap
##         <fctr>    <fctr> <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan      Asia  1957  30.332  9240934  820.8530
##  2 Afghanistan      Asia  1967  34.020 11537966  836.1971
##  3 Afghanistan      Asia  1977  38.438 14880372  786.1134
##  4 Afghanistan      Asia  1987  40.822 13867957  852.3959
##  5 Afghanistan      Asia  1997  41.763 22227415  635.3414
##  6 Afghanistan      Asia  2007  43.828 31889923  974.5803
##  7      Rwanda    Africa  1952  40.000  2534927  493.3239
##  8      Rwanda    Africa  1962  43.000  3051242  597.4731
##  9      Rwanda    Africa  1972  44.600  3992121  590.5807
## 10      Rwanda    Africa  1982  46.218  5507565  881.5706
## 11      Rwanda    Africa  1992  23.599  7290203  737.0686
## 12      Rwanda    Africa  2002  43.413  7852401  785.6538
```

```r
# The above is incorrect! Instead below are the two solutions I can think of to filter out rows containing Rwanda and Afghanistan!

gapminder %>%
  filter(country %in% c("Rwanda", "Afghanistan"))
```

```
## # A tibble: 24 x 6
##        country continent  year lifeExp      pop gdpPercap
##         <fctr>    <fctr> <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan      Asia  1952  28.801  8425333  779.4453
##  2 Afghanistan      Asia  1957  30.332  9240934  820.8530
##  3 Afghanistan      Asia  1962  31.997 10267083  853.1007
##  4 Afghanistan      Asia  1967  34.020 11537966  836.1971
##  5 Afghanistan      Asia  1972  36.088 13079460  739.9811
##  6 Afghanistan      Asia  1977  38.438 14880372  786.1134
##  7 Afghanistan      Asia  1982  39.854 12881816  978.0114
##  8 Afghanistan      Asia  1987  40.822 13867957  852.3959
##  9 Afghanistan      Asia  1992  41.674 16317921  649.3414
## 10 Afghanistan      Asia  1997  41.763 22227415  635.3414
## # ... with 14 more rows
```

```r
gapminder %>%
  filter(country == "Canada" | country == "Afghanistan")
```

```
## # A tibble: 24 x 6
##        country continent  year lifeExp      pop gdpPercap
##         <fctr>    <fctr> <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan      Asia  1952  28.801  8425333  779.4453
##  2 Afghanistan      Asia  1957  30.332  9240934  820.8530
##  3 Afghanistan      Asia  1962  31.997 10267083  853.1007
##  4 Afghanistan      Asia  1967  34.020 11537966  836.1971
##  5 Afghanistan      Asia  1972  36.088 13079460  739.9811
##  6 Afghanistan      Asia  1977  38.438 14880372  786.1134
##  7 Afghanistan      Asia  1982  39.854 12881816  978.0114
##  8 Afghanistan      Asia  1987  40.822 13867957  852.3959
##  9 Afghanistan      Asia  1992  41.674 16317921  649.3414
## 10 Afghanistan      Asia  1997  41.763 22227415  635.3414
## # ... with 14 more rows
```

###Playing around with other dplyr functions

```r
gapminder %>% 
  rename(LifeExpectancy = lifeExp,
         `GDP per capita` = gdpPercap)
```

```
## # A tibble: 1,704 x 6
##        country continent  year LifeExpectancy      pop `GDP per capita`
##         <fctr>    <fctr> <int>          <dbl>    <int>            <dbl>
##  1 Afghanistan      Asia  1952         28.801  8425333         779.4453
##  2 Afghanistan      Asia  1957         30.332  9240934         820.8530
##  3 Afghanistan      Asia  1962         31.997 10267083         853.1007
##  4 Afghanistan      Asia  1967         34.020 11537966         836.1971
##  5 Afghanistan      Asia  1972         36.088 13079460         739.9811
##  6 Afghanistan      Asia  1977         38.438 14880372         786.1134
##  7 Afghanistan      Asia  1982         39.854 12881816         978.0114
##  8 Afghanistan      Asia  1987         40.822 13867957         852.3959
##  9 Afghanistan      Asia  1992         41.674 16317921         649.3414
## 10 Afghanistan      Asia  1997         41.763 22227415         635.3414
## # ... with 1,694 more rows
```

```r
# this is new to me! As I usually rename my columns via the following:

colnames(gapminder) <- c("country", "continent", "year", "LifeExp", "pop", "GDP per capita")

# but my original method requires me to list all the fields, I like that this way I can specify certain columns to change. I will use this one from now on!
```

### Trying out dplyr::group_by() and count()


```r
# grouping by continent and counting how many within
gapminder %>%
  group_by(continent) %>%
  tally()
```

```
## # A tibble: 5 x 2
##   continent     n
##      <fctr> <int>
## 1    Africa   624
## 2  Americas   300
## 3      Asia   396
## 4    Europe   360
## 5   Oceania    24
```

```r
# another way of doing it:
gapminder %>%
  count(continent)
```

```
## # A tibble: 5 x 2
##   continent     n
##      <fctr> <int>
## 1    Africa   624
## 2  Americas   300
## 3      Asia   396
## 4    Europe   360
## 5   Oceania    24
```


### Testing out knitr::kable
A simple (prettier) table generator!


```r
library(knitr)
gapminder %>%
  count(continent) %>% 
  knitr::kable(align = "c")
```



 continent     n  
-----------  -----
  Africa      624 
 Americas     300 
   Asia       396 
  Europe      360 
  Oceania     24  

```r
# Looks like more custom options available with rownames, colnames, table title etc
```


## My reflections aka report on my progress

Testing out some dplyr functions. For my research, I'm currently writing for loops to go through my data (for some reason I keep gravitating towards for loops) but I'm find it very very slow. I'm really liking these dplyr functions in hopes that some functions will be able to more efficiently replace my need for for loops!

There's ALOT of functions within R and we barely scratch the surface in class or in the assignments. I feel like I'm not quite proficient in applying all these functions nor do I feel that I have the time to go through each of these functions in my spare time (I do still have my thesis). On the positive side - I feel like one of the hardest things is being aware of the existence of this functions in the first place! So hopefully with this new found knowledge, I can always Stack Overflow when I do need to implement it.

Overall, good second assignment. I'm finding the ?help within R the most useful resource in discovering more functionalities of each package. And my two lifesavers: the [R Markdown cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf) and the [R Studio data wrangling cheat sheets](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)!   

That's all for now!  

![](https://media.giphy.com/media/uYffljMqX1EHe/giphy.gif)


