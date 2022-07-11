# Rota-coordination

# R code for constructing weekend rota

``` r{create rota}
library(dplyr)
library(stringr)
library(timeDate)

# create date range
df <- data.frame(date = seq.Date(from = as.Date('2022-08-30'), to = as.Date('2023-01-02'), by = 'days')) # create date range

# add the days
df$day <- weekdays(as.Date(df$date)) 

# add the bank holidays
df$bankhol <- as.factor(df$date %in% (as.Date(holidayLONDON(2022:2023))))

# if weekend rota only then select just weekends and bank holidays
rota <- df %>% filter(day == 'Saturday'|day == 'Sunday'|bankhol == T) %>% select(date, day)

# add the names on the rota as a character vector
rota$name <- rep(c('name A', 'name B', 'name C'), times=13)

# annotate weekdays as being bank holidays
rota$day <- gsub("Monday", "BH Monday", rota$day) 
rota$day <- gsub("Tuesday", "BH Tuesday", rota$day) 
```
