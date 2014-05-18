Reproducible Research: Peer Assignment 1
========================================================
## Loading and preprocessing the data

```r
actdata <- read.csv("./data/activity.csv")
```

```
## Warning: cannot open file './data/activity.csv': No such file or directory
```

```
## Error: cannot open the connection
```

```r

```


## What is mean total number of steps taken per day?
1.Make a histogram of the total number of steps taken each day

```r
library(plyr)
```

```
## Warning: package 'plyr' was built under R version 3.0.3
```

```r
sumsteps <- ddply(actdata, ~date, summarize, total = sum(steps, na.rm = TRUE))
```

```
## Error: object 'actdata' not found
```

```r
timeconvert <- strptime(sumsteps$date, format = "%Y-%m-%d")
```

```
## Error: object 'sumsteps' not found
```

```r
plot(x <- timeconvert, y <- sumsteps$total, type = "h", lwd = 3, col = "green", 
    main = "Histogram of the total number of steps taken each day", xlab = "Date", 
    ylab = "Total number of steps")
```

```
## Error: object 'timeconvert' not found
```

2.Calculate and report the mean and median total number of steps taken per day


```r
mean(sumsteps$total)
```

```
## Error: object 'sumsteps' not found
```

```r
median(sumsteps$total)
```

```
## Error: object 'sumsteps' not found
```


## What is the average daily activity pattern?

1.Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
avgsteps <- ddply(actdata, ~interval, summarize, avg = mean(steps, na.rm = TRUE))
```

```
## Error: object 'actdata' not found
```

```r
plot(x <- avgsteps$interval, y <- avgsteps$avg, type = "l", lwd = 3, col = "red", 
    main = "Average daily activity pattern", xlab = "Interval", ylab = "Average number of steps")
```

```
## Error: object 'avgsteps' not found
```

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
avgsteps$interval[which.max(avgsteps$avg)]
```

```
## Error: object 'avgsteps' not found
```


## Imputing missing values

1.Total number of missing values in the dataset 

```r
sum(is.na(actdata))
```

```
## Error: object 'actdata' not found
```


2.Filling in all of the missing values in the dataset by using the mean for that day.
3.Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
daymean <- ddply(actdata, ~interval, summarize, med = median(steps, na.rm = TRUE))
```

```
## Error: object 'actdata' not found
```

```r

data2 <- actdata
```

```
## Error: object 'actdata' not found
```

```r

for (i in 1:nrow(data2)) {
    
    if (is.na(data2$steps[i])) {
        
        asd <- daymean$interval == data2$interval[i]
        
        data2$steps[i] <- daymean$med[asd]
    }
}
```

```
## Error: object 'data2' not found
```

4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day.

```r

sumsteps2 <- ddply(data2, ~date, summarize, total = sum(steps, na.rm = TRUE))
```

```
## Error: object 'data2' not found
```

```r

timeconvert2 <- strptime(sumsteps2$date, format = "%Y-%m-%d")
```

```
## Error: object 'sumsteps2' not found
```

```r

plot(x <- timeconvert2, y <- sumsteps2$total, type = "h", lwd = 3, col = "blue", 
    main = "Histogram\nof the total number of steps taken each day", xlab = "Date", 
    ylab = "Total number of steps")
```

```
## Error: object 'timeconvert2' not found
```



```r
mean(sumsteps2$total)
```

```
## Error: object 'sumsteps2' not found
```

```r
median(sumsteps2$total)
```

```
## Error: object 'sumsteps2' not found
```


```
Do these values differ from the estimates from the first part of the assignment?
Yes
What is the impact of imputing missing data on the estimates of the total daily number of steps?
mean and is different, and meadian the same


## Are there differences in activity patterns between weekdays and weekends?


```r
arg1 <- strptime(sumsteps2$date, format = "%Y-%m-%d")
```

```
## Error: object 'sumsteps2' not found
```

```r
data2$isWeekDay <- "weekday"
```

```
## Error: object 'data2' not found
```

```r
Wknd <- weekdays(arg1) == "Saturday" | weekdays(arg1) == "Sunday"
```

```
## Error: object 'arg1' not found
```

```r
data2$isWeekDay[Wknd] = "weekend"
```

```
## Error: object 'data2' not found
```

```r

dWeekend <- ddply(data2[data2$isWeekDay == "weekend", ], ~date, summarize, avg = mean(steps, 
    na.rm = TRUE))
```

```
## Error: object 'data2' not found
```

```r
tst <- ddply(data2, .(isWeekDay, interval), summarize, avg = mean(steps, na.rm = TRUE))
```

```
## Error: object 'data2' not found
```

```r


par(mfrow = c(2, 1))
plot(x <- tst[tst$isWeekDay == "weekday", ]$interval, y <- tst[tst$isWeekDay == 
    "weekday", ]$avg, type = "l", xlab = "interval", ylab = "avg number of steps", 
    main = "Weekday")
```

```
## Error: object 'tst' not found
```

```r
plot(x <- tst[tst$isWeekDay == "weekend", ]$interval, y <- tst[tst$isWeekDay == 
    "weekend", ]$avg, type = "l", xlab = "interval", ylab = "avg number of steps", 
    main = "Weekend")
```

```
## Error: object 'tst' not found
```


