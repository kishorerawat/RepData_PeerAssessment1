# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

#### Extract the .CSV file from the .ZIP file, first checking if they don't already exist

```r
if(!file.exists("activity.csv"))
  unzip("activity.zip")
#read the .csv file and store the data in activity_data variable
if(!exists("activity_data"))
  activity_data <- read.csv("activity.csv")
```


## What is mean total number of steps taken per day?

#### Using <i>aggregate</i> function to calculate the total number of steps for each date 

```r
Datewise_Total_Steps <- aggregate(steps~date, activity_data, sum, 
                                  na.action = na.omit)
```
#### Now, here is the Histogram showing the total steps taken each day
![](PA1_template_files/figure-html/histogram_total_steps-1.png)<!-- -->

#### The mean and median of the total steps taken are, 

```
## [1] "Mean Total Steps / Day   =  10766"
```

```
## [1] "Median Total Steps / Day =  10765"
```

## What is the average daily activity pattern?

#### Calculate the average number of steps taken per 5-minute interval, where the steps taken are averaged across all days.

#### Using function <i>aggregate</i> to compute this average with <i>steps~interval</i> formula,

```r
Avg_Steps_Per_Interval <- aggregate(steps~interval, activity_data, mean, na.action = na.omit)
```
#### ...and here is the XY-plot showing average number of steps(y-axis) for each 5-minute interval(x-axis)
![](PA1_template_files/figure-html/xy_plot_avg_steps-1.png)<!-- -->

#### In this plot, the 5-minute interval which which has the highest average steps taken is,

```
## [1] "5-minute interval with max steps =  835"
```

## Imputing missing values

#### We will replace the NA values in the </i>steps</i> column with the average value of steps taken, 

```r
# find the mean of steps column for all rows, round it off
mean_steps <- round(mean(activity_data$steps, na.rm=TRUE))

# keep the original data in another variable
activity_data_original <- activity_data
# replace all instances of NA in the "steps" column with this 
# average steps value
activity_data[["steps"]][is.na(activity_data[["steps"]])] <- mean_steps
```

#### Now, after "imputing the missing values", we will recalculate the steps taken for each date,

```r
Datewise_Total_Steps <- aggregate(steps~date, activity_data, sum, 
                                  na.action = na.omit)
```

#### ...and here is the new histogram after imputing the missing values,
![](PA1_template_files/figure-html/histogram_total_steps_new-1.png)<!-- -->

#### The new mean and median of the total steps taken are,

```
## [1] "Mean Total Steps / Day   =  10752"
```

```
## [1] "Median Total Steps / Day =  10656"
```

#### We can observe the differences in the original and new values after replacing the missing values.

## Are there differences in activity patterns between weekdays and weekends?
