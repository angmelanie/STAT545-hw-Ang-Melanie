# Homework 4: Join Cheatsheet
Melanie Ang  
October 5, 2017  

## Melanie's cheatsheet for dplyr::join 
## Featuring FISH!

![](https://lph5i1b6c053kq7us26bdk75-wpengine.netdna-ssl.com/wp-content/uploads/2015/05/kissing-gourami-fish.jpg)

Honestly, I'm a bit sick of working with the gapminder dataset, so I will take this opportunity to create my own cheatsheet to illustrate the different types of joins. Plus also make it (somewhat) relevant to my thesis about fish!


```r
library(tidyverse)
```


We are working with 2 data frames:  
- Canada_fishes: 7 common marine fishes found in Canadian waters  
- Fish_habitat: Some fun facts about where you can find these fishes. Courtesy of [FishBase](www.fishbase.ca)  

Let's begin by loading these dataframes. These csv files are found within this github folder.


```r
Canada_fishes <- "
Common name, Scientific name
Pink salmon, Oncorhynchus gorbuscha
Chum salmon, Oncorhynchus keta
Sockeye salmon, Oncorhynchus nerka
Chinook salmon, Oncorhynchus tshawytscha
Coho salmon, Oncorhynchus kisutch
Pacific halibut, Hippoglossus stenolepis
Pacific herring, Clupea pallasii pallasii
"
Canada_fishes <- read.csv("~/STAT545-hw-Ang-Melanie/hw-04/Canada_fishes.csv")

Fish_habitat <- "
Scientific name, Northern Latitude, Southern Latitude, Maximum depth
Oncorhynchus gorbuscha, 79N, 29N, 250m
Oncorhynchus keta, 67N, 24N, 250m
Oncorhynchus nerka, 72N, 42N, 250m
Oncorhynchus tshawytscha, 72N, 27N, 375m
Oncorhynchus kisutch, 72N, 22N, 250m
Hippoglossus stenolepis, 73N, 42N, 1200m
Gadus chalcogrammus, 68N, 34N, 400m
"

Fish_habitat <- read.csv("~/STAT545-hw-Ang-Melanie/hw-04/Fish_habitat.csv")
```

Lets take a look at these dataframes more closely:



<table border = 1>
<tr>
<td valign="top">
  Canada Fishes
  

|Common.name     |Scientific.name          |
|:---------------|:------------------------|
|Pink salmon     |Oncorhynchus gorbuscha   |
|Chum salmon     |Oncorhynchus keta        |
|Sockeye salmon  |Oncorhynchus nerka       |
|Chinook salmon  |Oncorhynchus tshawytscha |
|Coho salmon     |Oncorhynchus kisutch     |
|Pacific halibut |Hippoglossus stenolepis  |
|Pacific herring |Clupea pallasii pallasii |


</td>
<td valign="top">
  Fish Habitat
  

|Scientific.name          |Northern.Latitude |Southern.Latitude |Maximum.Depth |
|:------------------------|:-----------------|:-----------------|:-------------|
|Oncorhynchus gorbuscha   |79N               |29N               |250m          |
|Oncorhynchus keta        |67N               |24N               |250m          |
|Oncorhynchus nerka       |72N               |42N               |250m          |
|Oncorhynchus tshawytscha |72N               |27N               |375m          |
|Oncorhynchus kisutch     |72N               |22N               |250m          |
|Hippoglossus stenolepis  |73N               |42N               |1200m         |
|Gadus chalcogrammus      |68N               |34N               |400m          |


</td>
</tr>
</table>

As you can see they are both linked through the common column, Scientific name. As you can see Canada fishes contain Pacific herring that is not in the Fish Habitat data frame. While Fish Habitat contains Gadus chalcogrammus (which is Alaska Pollock for those who are curious), that is not in the Canada Fishes data table.

I will now demonstrate the following joins:
1. [left_join()](#left_join)
2. [inner_join()](#inner_join)
3. [full_join()](#full_join)
4. [semi_join()](#semi_join)
5. [anti_join()](#anti_join)


### left_join
left_join(Canada_fishes, Fish_habitat)  
> Join matching rows from Fish_habitat to Canada_fishes
It inputs everything from Canada_fishes and matches NA from Fish_habitat if that value is unavailable. Notice that the extra row in Fish_habitat is not present here


```r
left_join(Canada_fishes, Fish_habitat, by = "Scientific.name") %>% 
  knitr::kable(format = "markdown")
```



|Common.name     |Scientific.name          |Northern.Latitude |Southern.Latitude |Maximum.Depth |
|:---------------|:------------------------|:-----------------|:-----------------|:-------------|
|Pink salmon     |Oncorhynchus gorbuscha   |79N               |29N               |250m          |
|Chum salmon     |Oncorhynchus keta        |67N               |24N               |250m          |
|Sockeye salmon  |Oncorhynchus nerka       |72N               |42N               |250m          |
|Chinook salmon  |Oncorhynchus tshawytscha |72N               |27N               |375m          |
|Coho salmon     |Oncorhynchus kisutch     |72N               |22N               |250m          |
|Pacific halibut |Hippoglossus stenolepis  |73N               |42N               |1200m         |
|Pacific herring |Clupea pallasii pallasii |NA                |NA                |NA            |

### inner_join
inner_join(Canada_fishes, Fish_habitat)  
> Join data and only retain rows in both sets.
It inputs only data that matches in both. Notice that the extra row in Canada_fishes and the extra row in Fish_habitat are not present here!


```r
inner_join(Canada_fishes, Fish_habitat, by = "Scientific.name") %>% 
  knitr::kable(format = "markdown")
```



|Common.name     |Scientific.name          |Northern.Latitude |Southern.Latitude |Maximum.Depth |
|:---------------|:------------------------|:-----------------|:-----------------|:-------------|
|Pink salmon     |Oncorhynchus gorbuscha   |79N               |29N               |250m          |
|Chum salmon     |Oncorhynchus keta        |67N               |24N               |250m          |
|Sockeye salmon  |Oncorhynchus nerka       |72N               |42N               |250m          |
|Chinook salmon  |Oncorhynchus tshawytscha |72N               |27N               |375m          |
|Coho salmon     |Oncorhynchus kisutch     |72N               |22N               |250m          |
|Pacific halibut |Hippoglossus stenolepis  |73N               |42N               |1200m         |

### full_join
full_join(Canada_fishes, Fish_habitat)  
> Join data and retains all values and all rows
It inputs all data from both datasets. Notice that the extra row in Canada_fishes and the extra row in Fish_habitat are both present here with NAs if the value is unavailable!


```r
full_join(Canada_fishes, Fish_habitat, by = "Scientific.name") %>% 
  knitr::kable(format = "markdown")
```

```
## Warning in full_join_impl(x, y, by$x, by$y, suffix$x, suffix$y): joining
## factors with different levels, coercing to character vector
```



|Common.name     |Scientific.name          |Northern.Latitude |Southern.Latitude |Maximum.Depth |
|:---------------|:------------------------|:-----------------|:-----------------|:-------------|
|Pink salmon     |Oncorhynchus gorbuscha   |79N               |29N               |250m          |
|Chum salmon     |Oncorhynchus keta        |67N               |24N               |250m          |
|Sockeye salmon  |Oncorhynchus nerka       |72N               |42N               |250m          |
|Chinook salmon  |Oncorhynchus tshawytscha |72N               |27N               |375m          |
|Coho salmon     |Oncorhynchus kisutch     |72N               |22N               |250m          |
|Pacific halibut |Hippoglossus stenolepis  |73N               |42N               |1200m         |
|Pacific herring |Clupea pallasii pallasii |NA                |NA                |NA            |
|NA              |Gadus chalcogrammus      |68N               |34N               |400m          |

### semi_join
semi_join(Canada_fishes, Fish_habitat)  
> All rows in Canada_fishes that have a match in Fish_habitat
This is a filtering join. Basically, it's looking through the first dataset (Canada_fishes) and keeping those (all columns) that have matches in the second data set (Fish_habitat). It returns no columns from Fish_habitat, just uses it as a filter to the first dataset.


```r
# structure/column looks exactly like the Canada_fishes data set except semi join only keeps rows from x that matches rows in y.
semi_join(Canada_fishes, Fish_habitat, by = "Scientific.name") %>% 
  knitr::kable(format = "markdown")
```



|Common.name     |Scientific.name          |
|:---------------|:------------------------|
|Pink salmon     |Oncorhynchus gorbuscha   |
|Chum salmon     |Oncorhynchus keta        |
|Sockeye salmon  |Oncorhynchus nerka       |
|Chinook salmon  |Oncorhynchus tshawytscha |
|Coho salmon     |Oncorhynchus kisutch     |
|Pacific halibut |Hippoglossus stenolepis  |

### anti_join
anti_join(Canada_fishes, Fish_habitat)  
> All rows in Canada_fishes that do not have a match in Fish_habitat
This is a filtering join. Basically, it's looking through the first dataset (Canada_fishes) and keeping those (all columns) that DO NOT have matches in the second data set (Fish_habitat). It returns no columns from Fish_habitat, just uses it as a filter to the first dataset.


```r
# structure/column looks exactly like the Canada_fishes data set except semi join only keeps rows from x that matches rows in y.
anti_join(Canada_fishes, Fish_habitat, by = "Scientific.name") %>% 
  knitr::kable(format = "markdown")
```



|Common.name     |Scientific.name          |
|:---------------|:------------------------|
|Pacific herring |Clupea pallasii pallasii |
