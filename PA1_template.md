# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
# Extract the file if it doesn't exist already
if(!file.exists("activity.csv"))
  unzip("activity.zip")
#read the .csv file and store the data in activity_data variable
activity_data <- read.csv("activity.csv")
```


## What is mean total number of steps taken per day?

```r
# 1.Calculate the total number of steps taken per day
# Use function "aggregate" to total the steps for each date
Datewise_Total_Steps <- aggregate(steps~date, activity_data, sum, 
                                  na.action = na.omit)
# 2.Make a histogram of the total number of steps taken each day
# for x-axis tick marks from 0 to 26000 at intervals of 2000
x_axis_ticks <- seq(from = 0, to = 26000, by = 2000)
hist(Datewise_Total_Steps$steps,
     main="Histogram - Total Steps", col="gray", border="blue",
     ylim = c(0,35), xaxt = "n", xlab="")
# create x-axis using "at" for the tick marks, labels vertical
axis(side=1, at=x_axis_ticks, las=2, hadj = 0.9)
# create the x-axis label
mtext(text="Total Steps Per Day", side=1, line=4)
```

![](PA1_template_files/figure-html/histogram_total_steps-1.png)<!-- -->


```
## [1] "Mean Total Steps/Day   =  10766"
```

```
## [1] "Median Total Steps/Day =  10765"
```

## What is the average daily activity pattern?

```r
# 1.Calculate the average number of steps taken per 5-minute interval,
#   where the steps taken are averaged across all days
# Use function "aggregate" to compute this average with steps~interval formula
Avg_Steps_Per_Interval <- aggregate(steps~interval, activity_data, mean, na.action = na.omit)

# 2.Make a histogram of the total number of steps taken each day
# x-axis tick marks from 0 to 2400 (higher than max 5-minuter interval value),
# with each interval of 200 points
x_axis_ticks <- seq(from = 0, to = 2400, by = 200)
# y-axis tick marks from 0 to 200 with tick-interval of 20
y_axis_ticks <- seq(from=0, to=200, by=20)

# plot the time series with the 5-minute interval on x-axis and
# average number of steps on y-axis
plot(Avg_Steps_Per_Interval$interval, # 5-minute interval on x-axis 
     Avg_Steps_Per_Interval$steps,    # Avg no of steps on y-axis
     type="l", xaxt="n", yaxt="n",    # type="l" for line graph
     xlab="", ylab="", col="blue", lwd=2,
     main="Average Number of Steps for 5-minute intervals")

# create x and y-axis labels using the variables we created
axis(side=1, at=x_axis_ticks, las=2, hadj = 0.9)
axis(side=2, at=y_axis_ticks)

# create the appropriate axis lables
mtext(text="5-minute intervals", side=1, line=4)
mtext(text="Avg No of Steps", side=2, line=3)
```

![](PA1_template_files/figure-html/plot_daily_activity-1.png)<!-- -->


```
## [1] "5-minute interval having max no of average steps taken =  835"
```

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?