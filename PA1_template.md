Loading the data
================

    data <- read.csv("C:/Users/jthomson/Documents/activity.csv")

What is mean total number of steps taken per day?
=================================================

    stepsdate <- aggregate(steps ~ date, data, sum, na.rm = TRUE)

#### Mean and median number of steps taken each day:

    mean(stepsdate$steps)

    ## [1] 10766.19

    median(stepsdate$steps)

    ## [1] 10765

#### Histogram of the total number of steps taken each day

    plot(stepsdate$steps,
            type = "h",
            main = "Total Number of Steps Taken Each Day",
            xlab = "Date", 
            ylab = "Steps",
            col = "red")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-4-1.png)

What is the average daily activity pattern?
===========================================

    stepsint <- aggregate(steps ~ interval, data, mean, na.rm = TRUE)

    plot(stepsint,
            type = "l",
            main = "Average Daily Activity Pattern",
            xlab = "Interval (5-minute)",
            ylab = "Average Number of Steps",
            col = "red")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-5-1.png)

#### The 5-minute interval that, on average, contains the maximum number of steps

    stepsint$interval[which.max(stepsint$steps)]

    ## [1] 835

Imputing missing values
=======================

#### Count of missing (NA) data

    sum(is.na(data))

    ## [1] 2304

#### Replace missing data with the mean

    library(imputeTS)
    imputed.data <- data
    imputed.data <- na.mean(imputed.data)
    imputed.data.stepsdate <- aggregate(steps ~ date, imputed.data, sum)

#### Mean and median number of steps taken each day:

    mean(imputed.data.stepsdate$steps)

    ## [1] 10766.19

    median(imputed.data.stepsdate$steps)

    ## [1] 10766.19

#### Histogram of the total number of steps taken each day

    plot(imputed.data$steps, 
            type = "h",
            main = "Total Number of Steps Taken Each Day",
            xlab = "Date", 
            ylab = "Steps",
            col = "red") 

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-10-1.png)

Are there differences in activity patterns between weekdays and weekends?
=========================================================================

#### Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

    weekdays <- c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday")

    imputed.data$day <- as.factor(ifelse(is.element(weekdays(as.Date(imputed.data$date)),weekdays), "Weekday", "Weekend"))

    steps.interval <- aggregate(steps ~ interval + day, imputed.data, mean)

    library(lattice)

    xyplot(steps.interval$steps ~ steps.interval$interval|steps.interval$day,
            type="l",
            main="Total Number of Steps Taken Each Day",
            xlab="Interval (5-minute)", 
            ylab="Steps",
            col = "red",
            layout=c(1,2))

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-11-1.png)
