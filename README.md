# Rota-coordination

# R code for constructing weekend rota

# R code for constructing weekend rota
library(dplyr)
library(stringr)
library(timeDate)

# create date range
df <- data.frame(date = seq.Date(from = as.Date('2022-12-28'), to = as.Date('2023-04-30'), by = 'days')) # create date range

# add the days
df$day <- weekdays(as.Date(df$date)) 

# add the bank holidays
df$bankhol <- as.factor(df$date %in% (as.Date(holidayLONDON(2022:2023))))

# if weekend rota only then select just weekends and bank holidays
rota <- df %>% filter(day == 'Saturday'|day == 'Sunday'|bankhol == T) %>% select(date, day)

# add the names on the rota as a character vector. 
names <- c('a', 'b', 'c', 'd')

# z is how many times you have to replicate the names list. round up to the nearest interger
z <- (as.numeric(nrow(rota))/as.numeric(length(names))) %>% ceiling 
names <- rep(c(names), times=z)  # replicate names the correct number of times. 
# curtail the length of the names list to the number of rows in the rota df so that it can be joined 
length(names) <- nrow(rota)

# join the names to the rota
rota <- cbind(rota, names)

# annotate weekdays as being bank holidays
rota$day <- gsub("Monday", "BH Monday", rota$day) 
rota$day <- gsub("Tuesday", "BH Tuesday", rota$day) 
rota$day <- gsub("Friday", "BH Friday", rota$day) 

write.csv(rota, 'weekend_rota.csv', row.names =F)
