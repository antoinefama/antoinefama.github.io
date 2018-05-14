---
title: "Project_V""
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


load usefull library

```{r }
library(tidyverse)
library(ggplot2)
library(ggmap)
library(lubridate) 
library(leaflet)

```



load merged data

```{r }

setwd("./data")


load("Merged_df.RDA")


```
fix the 2 wring localisation we have : 
```{r }
str_which(Merged_df$DBA,"ATRIUMDUMBO")
Merged_df$DBA[174]

str_which(Merged_df$DBA,"TRAIF")
Merged_df$DBA[193]


register_google(key = "AIzaSyA6LwK5aDH3JTsgpRrt3FQv4G7Jv5siz74")
Merged_df$location[[174]] <- geocode("15 Main St, Brooklyn, NY 11201, États-Unis")
Merged_df$location[[193]] <- geocode("229 S 4th St, between Havemeyer St & Roebling St, Brooklyn, NY 11211-5605")


```
Plot map with color for cuisijne type.

```{r }

unique(Merged_df$`CUISINE DESCRIPTION`)


location <- data_frame(lon = Merged_df$location)

location <- unnest(location)

location$cuisine <- Merged_df$`CUISINE DESCRIPTION`


x <- as.factor(Merged_df$`CUISINE DESCRIPTION`)

color_easy = colors()[x]



icons <- awesomeIcons(
  icon = 'ios-close',
  iconColor = 'black',
  library = 'ion',
  markerColor = color_easy
)

leaflet(location) %>% addTiles() %>%
  addAwesomeMarkers(~lon, ~lat, icon=icons, label=~Merged_df$DBA)

Merged_df %>% group_by(`CUISINE DESCRIPTION`) %>% count() 
```
