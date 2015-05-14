Reproducible Research: Peer Assessment 1
========================================
## Purpose of this document
This is the first peer-assessed assignment for the Coursera Reproducible Reseach course. It documents the steps taken to analyse data collected from a FitBit device. The steps in this analysis are:

- loading and preprocessing data
- imputing missing values
- intepreting data to answer the assignment questions

## Load Required Libraries
The following libraries will be required to answer the assignment questions:

```r
library(Hmisc)
```

```
## Loading required package: grid
## Loading required package: lattice
## Loading required package: survival
## Loading required package: Formula
## Loading required package: ggplot2
## 
## Attaching package: 'Hmisc'
## 
## The following objects are masked from 'package:base':
## 
##     format.pval, round.POSIXt, trunc.POSIXt, units
```

```r
library(ggplot2)
```

## Load and Pre-processing the Data
### Load the data
The following code with test for the existence of the unzipped data and load the data into a dataframe:

```r
if(!file.exists('activity.csv')){
    unzip('activity.zip')
}
activityData <- read.csv('activity.csv')
```
### Pre-process the data
The code below will set the class of the date column as Date:

```r
activityData$date <- as.Date(activityData$date, format="%Y-%m-%d")
```

## What is mean total number of steps taken per day?
The following code sums up the steps taken per day:

```r
stepsPerDay <- tapply(activityData$steps, activityData$date, sum, na.rm=TRUE)
```

### Histogram of total number of steps taken each day:

```r
qplot(stepsPerDay, xlab='Total steps per day', ylab='Frequency', binwidth=500)
```

![plot of chunk hist_total_steps_per_day](figure/hist_total_steps_per_day-1.png) 

### Calculate the mean and median of the total number of steps taken per day
The following code was used to calculate the mean and median of the steps taken per day:

```r
meanOrig <- mean(stepsPerDay)
medianOrig <- median(stepsPerDay)
```

The **mean** of the total steps taken per day is: 

```
## [1] 9354.23
```
The **median** of the total steps taken per day is: 

```
## [1] 10395
```

## What is the average daily activity pattern?
This code chunk preps the data for the time-series plot:

```r
avgStepsInt <- aggregate(x=list(meanSteps=activityData$steps), by=list(interval=activityData$interval), FUN=mean, na.rm=TRUE)
```

### Time-Series Plot of Average Steps per 5-minute Interval

```r
ggplot(data=avgStepsInt, aes(x=interval, y=meanSteps)) +
    geom_line() +
    xlab("5-minute interval") +
    ylab("average number of steps")
```

![plot of chunk ts_avg_steps_per_5_min_interval](figure/ts_avg_steps_per_5_min_interval-1.png) 

### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?
The following code determines which time interval has the highest average number of steps:

```r
maxSteps <- which.max(avgStepsInt$meanSteps)
timeMaxSteps <- gsub("([0-9]{1,2})([0-9]{2})", "\\1:\\2", avgStepsInt[maxSteps,'interval'])
```

The **time interval** at which the average maximum number of steps occurs is:

```
## [1] "8:35"
```

## Imputing missing values

### Calculate and report the total number of missing values in the dataset
This code chunk calculates the number of missing values in the steps column:

```r
numMissingValues <- length(which(is.na(activityData$steps)))
```

The total number of missing values is:

```
## [1] 2304
```

### Devise a strategy for filling in all of the missing values in the dataset
The missing values will be replaced by the mean of the steps, excluding NAs.

### Create a new dataset that is equal to the original dataset but with the missing data filled in
The following code creates a new data set with with missing data filled in:

```r
activityDataImputed <- activityData
activityDataImputed$steps <- impute(activityData$steps, fun=mean)
```

### Make a histogram of the total number of steps taken each day
This code chunk preps the imputed data for the histogram:

```r
stepsPerDayImputed <- tapply(activityDataImputed$steps, activityDataImputed$date, sum)
qplot(stepsPerDayImputed, xlab='Total steps per day (Imputed)', ylab='Frequency', binwidth=500)
```

![plot of chunk hist_total_steps_per_day_imputed](figure/hist_total_steps_per_day_imputed-1.png) 

### Calculate and report the mean and median total number of steps taken per day
The following code was used to calculate the mean and median of the imputed steps taken per day:

```r
meanImp <- mean(stepsPerDayImputed)
medianImp <- median(stepsPerDayImputed)
```
The **mean** of the total steps taken per day, WITH IMPUTED DATA, is: 

```
## [1] 10766.19
```
The **median** of the total steps taken per day, WITH IMPUTED DATA, is: 

```
## [1] 10766.19
```

## Are there differences in activity patterns between weekdays and weekends?
The following code calculates the difference between the original and imputed means and medians. This difference, as expected, is substantial, due to the large amount of missing data that needed to be imputed.

```r
meanDiff <- abs(meanOrig-meanImp)
medianDiff <- abs(medianOrig-medianImp)
```

The difference in the means is:

```
## [1] 1411.959
```

The difference in the medians is:

```
## [1] 371.1887
```
### Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day
The code below will add a factor column based on the weekday: 

```r
activityDataImputed$dateType <- ifelse(as.POSIXlt(activityDataImputed$date)$wday %in% c(0,6), 'weekend', 'weekday')
```

### Make a panel plot containing a time series plot
This code chunk preps the imputed data for the time-series plot:

```r
avgStepsIntImputed <- aggregate(steps ~ interval + dateType, data=activityDataImputed, mean)
ggplot(avgStepsIntImputed, aes(interval, steps)) + 
    geom_line() + 
    facet_grid(dateType ~ .) +
    xlab("5-minute interval") + 
    ylab("avarage number of steps")
```

![plot of chunk ts_avg_steps_per_5_min_interval_imputed](figure/ts_avg_steps_per_5_min_interval_imputed-1.png) 
