Peer Assessment 1
=================

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>. When the **Knit** button is clicked, a document is generated that includes both content as well as the output of any embedded R code chunks within it.

This R Markdown document is the first of two course projects, or Peer Assessment 1, of Coursera's Reproducible Research class, the fifth (5th) of nine (9) courses (plus a Capstone Project) in Johns Hopkins University's Data Science Specialization.

All the work has been done locally prior to making any commits to GitHub.

Introduction
------------

It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a [Fitbit](http://www.fitbit.com/), [Nike Fuelband](http://www.nike.com/us/en_us/c/nikeplus-fuel), or [Jawbone Up](https://jawbone.com/up). These type of devices are part of the "quantified self" movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.
Data

The data for this assignment can be downloaded from the forked [GitHub Repository created for this assignment](https://github.com/rdpeng/RepData_PeerAssessment1):

- *Dataset*: [Activity monitoring data](https://github.com/NeryCastillo-McIntyre/RepData_PeerAssessment1/blob/master/activity.zip) [52K]

The variables included in this dataset are:

- **steps**: Number of steps taking in a 5-minute interval (missing values are coded as NA)

- **date**: The date on which the measurement was taken in YYYY-MM-DD format

- **interval**: Identifier for the 5-minute interval in which measurement was taken

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

### Loading and Pre-Processing the Data

Since all the work has been done locally, the first step after opening RStudio is to set the proper working directory. The selection varies according to individual idiosyncrasies, but for purposes of Peer Assessment 1, a subdirectory has been created, named "Project 1". The data file called "Activity.csv" has been downloaded and saved the "Project 1" subdirectory.

1. Load the data and briefly explore it


```r
Activity <- read.csv ("Activity.csv")
    head(Activity)
```

```
##   steps      date interval
## 1    NA 10/1/2012        0
## 2    NA 10/1/2012        5
## 3    NA 10/1/2012       10
## 4    NA 10/1/2012       15
## 5    NA 10/1/2012       20
## 6    NA 10/1/2012       25
```

```r
    tail(Activity)
```

```
##       steps       date interval
## 17563    NA 11/30/2012     2330
## 17564    NA 11/30/2012     2335
## 17565    NA 11/30/2012     2340
## 17566    NA 11/30/2012     2345
## 17567    NA 11/30/2012     2350
## 17568    NA 11/30/2012     2355
```

```r
    str(Activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "10/1/2012","10/10/2012",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

```r
    summary(Activity)
```

```
##      steps                date          interval     
##  Min.   :  0.00   10/1/2012 :  288   Min.   :   0.0  
##  1st Qu.:  0.00   10/10/2012:  288   1st Qu.: 588.8  
##  Median :  0.00   10/11/2012:  288   Median :1177.5  
##  Mean   : 37.38   10/12/2012:  288   Mean   :1177.5  
##  3rd Qu.: 12.00   10/13/2012:  288   3rd Qu.:1766.2  
##  Max.   :806.00   10/14/2012:  288   Max.   :2355.0  
##  NA's   :2304     (Other)   :15840
```

2. Process/transform the data into a format suitable for analysis, and start by loading the necessary packages, including ```plyr```, ```dplyr```, ```tidyr```, and ```lubridate``` from library.




```r
    StepsTaken <- tbl_df(Activity)
        StepsTaken$date <- mdy(StepsTaken$date)
            day <- wday(StepsTaken$date, label = TRUE, abbr = FALSE)
                StepsTaken <- cbind(StepsTaken, day)
                head(StepsTaken)
```

```
##   steps       date interval    day
## 1    NA 2012-10-01        0 Monday
## 2    NA 2012-10-01        5 Monday
## 3    NA 2012-10-01       10 Monday
## 4    NA 2012-10-01       15 Monday
## 5    NA 2012-10-01       20 Monday
## 6    NA 2012-10-01       25 Monday
```

```r
               tail(StepsTaken)
```

```
##       steps       date interval    day
## 17563    NA 2012-11-30     2330 Friday
## 17564    NA 2012-11-30     2335 Friday
## 17565    NA 2012-11-30     2340 Friday
## 17566    NA 2012-11-30     2345 Friday
## 17567    NA 2012-11-30     2350 Friday
## 17568    NA 2012-11-30     2355 Friday
```

```r
               str(StepsTaken)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : POSIXct, format: "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ day     : Ord.factor w/ 7 levels "Sunday"<"Monday"<..: 2 2 2 2 2 2 2 2 2 2 ...
```

```r
               summary(StepsTaken)
```

```
##      steps             date               interval             day      
##  Min.   :  0.00   Min.   :2012-10-01   Min.   :   0.0   Sunday   :2304  
##  1st Qu.:  0.00   1st Qu.:2012-10-16   1st Qu.: 588.8   Monday   :2592  
##  Median :  0.00   Median :2012-10-31   Median :1177.5   Tuesday  :2592  
##  Mean   : 37.38   Mean   :2012-10-31   Mean   :1177.5   Wednesday:2592  
##  3rd Qu.: 12.00   3rd Qu.:2012-11-15   3rd Qu.:1766.2   Thursday :2592  
##  Max.   :806.00   Max.   :2012-11-30   Max.   :2355.0   Friday   :2592  
##  NA's   :2304                                           Saturday :2304
```

### What Is the Mean Total Number of Steps Taken Per Day

For this part of the assignment, missing values in the dataset may be ignored.

1. Calculate the total number of steps taken per day


```r
    Clean <- filter(StepsTaken, !is.na(steps))
        Daily <- group_by(Clean, date)
            DailyTotal <- summarize(Daily, sum(steps))
                DailyTotal
```

```
## Source: local data frame [53 x 2]
## 
##          date sum(steps)
## 1  2012-10-02        126
## 2  2012-10-03      11352
## 3  2012-10-04      12116
## 4  2012-10-05      13294
## 5  2012-10-06      15420
## 6  2012-10-07      11015
## 7  2012-10-09      12811
## 8  2012-10-10       9900
## 9  2012-10-11      10304
## 10 2012-10-12      17382
## ..        ...        ...
```

2. Make a histogram of the total number of steps taken each day


```r
   hist(DailyTotal$`sum(steps)`, col = "red", breaks = 30, main = "Histogram",
         xlab = "Total Number of Steps Taken Each Day")
    rug(DailyTotal$`sum(steps)`)
    abline(v = median(DailyTotal$`sum(steps)`), col = "orange", lwd = 3)             
```

![plot of chunk Histogram of Total Number of Steps Taken Each Day](figure/Histogram of Total Number of Steps Taken Each Day-1.png) 

3. Calculate and report the mean and median of the total number of steps taken per day


```r
    DailyMedian <- summarize(Daily, median(steps))
        DailyMedian
```

```
## Source: local data frame [53 x 2]
## 
##          date median(steps)
## 1  2012-10-02             0
## 2  2012-10-03             0
## 3  2012-10-04             0
## 4  2012-10-05             0
## 5  2012-10-06             0
## 6  2012-10-07             0
## 7  2012-10-09             0
## 8  2012-10-10             0
## 9  2012-10-11             0
## 10 2012-10-12             0
## ..        ...           ...
```

```r
    DailyAverage <- summarize(Daily, mean(steps))
        DailyAverage
```

```
## Source: local data frame [53 x 2]
## 
##          date mean(steps)
## 1  2012-10-02     0.43750
## 2  2012-10-03    39.41667
## 3  2012-10-04    42.06944
## 4  2012-10-05    46.15972
## 5  2012-10-06    53.54167
## 6  2012-10-07    38.24653
## 7  2012-10-09    44.48264
## 8  2012-10-10    34.37500
## 9  2012-10-11    35.77778
## 10 2012-10-12    60.35417
## ..        ...         ...
```

### What is the average daily activity pattern?

1. Make a time series plot (i.e. ```type = "l"```) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
   FiveMinute <- group_by(Clean, interval)
        DailyPattern <- summarize(FiveMinute, mean(steps))
            DailyPattern
```

```
## Source: local data frame [288 x 2]
## 
##    interval mean(steps)
## 1         0   1.7169811
## 2         5   0.3396226
## 3        10   0.1320755
## 4        15   0.1509434
## 5        20   0.0754717
## 6        25   2.0943396
## 7        30   0.5283019
## 8        35   0.8679245
## 9        40   0.0000000
## 10       45   1.4716981
## ..      ...         ...
```

```r
    plot(DailyPattern$interval, DailyPattern$`mean(steps)`, type = "l", 
         col = "black", main = "Average Daily Activity Pattern", 
         xlab = "5-Minute Interval", ylab = "Average Number of Steps")
```

![plot of chunk Number of Steps Taken per 5-Minute Interval](figure/Number of Steps Taken per 5-Minute Interval-1.png) 

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
   arrange(DailyPattern, desc(`mean(steps)`)) 
```

```
## Source: local data frame [288 x 2]
## 
##    interval mean(steps)
## 1       835    206.1698
## 2       840    195.9245
## 3       850    183.3962
## 4       845    179.5660
## 5       830    177.3019
## 6       820    171.1509
## 7       855    167.0189
## 8       815    157.5283
## 9       825    155.3962
## 10      900    143.4528
## ..      ...         ...
```

### Imputing missing values

Note that there are a number of day/intervals where there are missing values (coded as ```NA```). The presence of missing days may introduce bias into some calculations or summaries of the data.

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with ```NA```s)


```r
   MissingValues <- is.na(StepsTaken)
        TotalMissingValues <- sum(MissingValues)
            TotalMissingValues 
```

```
## [1] 2304
```

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, one could use the mean/median for that day, or the mean for that 5-minute interval, etc.

One of the contributors on Stack Overflow has referred to the idea of replacing missing values with the mean of a column as "statistical malpractice," on the basis that such action "seriously inflate[s] the significance of the results." On the other hand, replacing all the missing values with zeroes would accomplish the opposite effect, to "seriously" deflate "the signifance of the results;" moreover, it would ignore the likelihood that the subject simply did not wear the tracking devise on the given day/date/time, and operate instead as if the subject failed to take any steps at all.

Thus, for the purposes of this assignment, the missing values are being replaced with the median for that 5-minute interval, since such value is neither as high as the mean nor as low as zero.

3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

After working on this item for far too long without success, credit belongs to one of the [discussion forum contributors](https://class.coursera.org/repdata-036/forum/thread?thread_id=64) for the following code.


```r
    FullInterval <- StepsTaken
        NAs <- is.na(FullInterval$steps)
            InterMedian <- tapply(FullInterval$steps, FullInterval$interval, median, na.rm = TRUE, simplify = TRUE)
                FullInterval$steps[NAs] <- InterMedian[as.character(FullInterval$interval[NAs])]
                    head(FullInterval)
```

```
##   steps       date interval    day
## 1     0 2012-10-01        0 Monday
## 2     0 2012-10-01        5 Monday
## 3     0 2012-10-01       10 Monday
## 4     0 2012-10-01       15 Monday
## 5     0 2012-10-01       20 Monday
## 6     0 2012-10-01       25 Monday
```

```r
                    tail(FullInterval)
```

```
##       steps       date interval    day
## 17563     0 2012-11-30     2330 Friday
## 17564     0 2012-11-30     2335 Friday
## 17565     0 2012-11-30     2340 Friday
## 17566     0 2012-11-30     2345 Friday
## 17567     0 2012-11-30     2350 Friday
## 17568     0 2012-11-30     2355 Friday
```

```r
                    str(FullInterval)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ steps   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ date    : POSIXct, format: "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ day     : Ord.factor w/ 7 levels "Sunday"<"Monday"<..: 2 2 2 2 2 2 2 2 2 2 ...
```

```r
                    summary(FullInterval)
```

```
##      steps          date               interval             day      
##  Min.   :  0   Min.   :2012-10-01   Min.   :   0.0   Sunday   :2304  
##  1st Qu.:  0   1st Qu.:2012-10-16   1st Qu.: 588.8   Monday   :2592  
##  Median :  0   Median :2012-10-31   Median :1177.5   Tuesday  :2592  
##  Mean   : 33   Mean   :2012-10-31   Mean   :1177.5   Wednesday:2592  
##  3rd Qu.:  8   3rd Qu.:2012-11-15   3rd Qu.:1766.2   Thursday :2592  
##  Max.   :806   Max.   :2012-11-30   Max.   :2355.0   Friday   :2592  
##                                                      Saturday :2304
```

```r
                        MissingNAValues <- is.na(FullInterval)
                            TotalMissingNAValues <- sum(MissingNAValues)
                                TotalMissingNAValues
```

```
## [1] 0
```

4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

a. Calculate the total number of steps taken per day


```r
    Dately <- group_by(FullInterval, date)
            DatelyTotal <- summarize(Dately, sum(steps))
                DatelyTotal
```

```
## Source: local data frame [61 x 2]
## 
##          date sum(steps)
## 1  2012-10-01       1141
## 2  2012-10-02        126
## 3  2012-10-03      11352
## 4  2012-10-04      12116
## 5  2012-10-05      13294
## 6  2012-10-06      15420
## 7  2012-10-07      11015
## 8  2012-10-08       1141
## 9  2012-10-09      12811
## 10 2012-10-10       9900
## ..        ...        ...
```

b. Make a histogram of the total number of steps taken each day


```r
   hist(DatelyTotal$`sum(steps)`, col = "red", breaks = 30, main = "Histogram",
         xlab = "Total Number of Steps Taken Each Day")
    rug(DatelyTotal$`sum(steps)`)
    abline(v = median(DatelyTotal$`sum(steps)`), col = "orange", lwd = 3)             
```

![plot of chunk New Histogram of Total Number of Steps Taken Each Day](figure/New Histogram of Total Number of Steps Taken Each Day-1.png) 

c. Calculate and report the mean and median of the total number of steps taken per day


```r
    DatelyMedian <- summarize(Dately, median(steps))
        DatelyMedian
```

```
## Source: local data frame [61 x 2]
## 
##          date median(steps)
## 1  2012-10-01             0
## 2  2012-10-02             0
## 3  2012-10-03             0
## 4  2012-10-04             0
## 5  2012-10-05             0
## 6  2012-10-06             0
## 7  2012-10-07             0
## 8  2012-10-08             0
## 9  2012-10-09             0
## 10 2012-10-10             0
## ..        ...           ...
```

```r
    DatelyAverage <- summarize(Dately, mean(steps))
        DatelyAverage
```

```
## Source: local data frame [61 x 2]
## 
##          date mean(steps)
## 1  2012-10-01    3.961806
## 2  2012-10-02    0.437500
## 3  2012-10-03   39.416667
## 4  2012-10-04   42.069444
## 5  2012-10-05   46.159722
## 6  2012-10-06   53.541667
## 7  2012-10-07   38.246528
## 8  2012-10-08    3.961806
## 9  2012-10-09   44.482639
## 10 2012-10-10   34.375000
## ..        ...         ...
```

d. Compare and contrast


```r
DailyMedian
```

```
## Source: local data frame [53 x 2]
## 
##          date median(steps)
## 1  2012-10-02             0
## 2  2012-10-03             0
## 3  2012-10-04             0
## 4  2012-10-05             0
## 5  2012-10-06             0
## 6  2012-10-07             0
## 7  2012-10-09             0
## 8  2012-10-10             0
## 9  2012-10-11             0
## 10 2012-10-12             0
## ..        ...           ...
```

```r
    DatelyMedian
```

```
## Source: local data frame [61 x 2]
## 
##          date median(steps)
## 1  2012-10-01             0
## 2  2012-10-02             0
## 3  2012-10-03             0
## 4  2012-10-04             0
## 5  2012-10-05             0
## 6  2012-10-06             0
## 7  2012-10-07             0
## 8  2012-10-08             0
## 9  2012-10-09             0
## 10 2012-10-10             0
## ..        ...           ...
```

```r
DailyAverage
```

```
## Source: local data frame [53 x 2]
## 
##          date mean(steps)
## 1  2012-10-02     0.43750
## 2  2012-10-03    39.41667
## 3  2012-10-04    42.06944
## 4  2012-10-05    46.15972
## 5  2012-10-06    53.54167
## 6  2012-10-07    38.24653
## 7  2012-10-09    44.48264
## 8  2012-10-10    34.37500
## 9  2012-10-11    35.77778
## 10 2012-10-12    60.35417
## ..        ...         ...
```

```r
    DatelyAverage
```

```
## Source: local data frame [61 x 2]
## 
##          date mean(steps)
## 1  2012-10-01    3.961806
## 2  2012-10-02    0.437500
## 3  2012-10-03   39.416667
## 4  2012-10-04   42.069444
## 5  2012-10-05   46.159722
## 6  2012-10-06   53.541667
## 7  2012-10-07   38.246528
## 8  2012-10-08    3.961806
## 9  2012-10-09   44.482639
## 10 2012-10-10   34.375000
## ..        ...         ...
```

e. Impact of Imputing Missing Data

The primary (or, even, sole) impact of imputing missing data on the estimates of the total daily number of steps - at least when comparing and contrasting the earlier median and mean calculations with the later ones - is that there are additional observations: 61, to be exact, as compared to 53. The other values appear to remain constant.

### Are there differences in activity patterns between weekdays and weekends?

For this part the ```weekdays()``` function may be of some help here. Use the dataset with the filled-in missing values for this part.

1. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

Same attribution as [above](https://class.coursera.org/repdata-036/forum/thread?thread_id=64).


```r
FullInterval <- mutate(FullInterval, weekenday = ifelse(day == "Monday" | day == "Tuesday" | day == "Wednesday" | day == "Thursday" | day == "Friday", "weekday", "weekend"))
    FullInterval$weekenday <- as.factor(FullInterval$weekenday)
        head(FullInterval)
```

```
##   steps       date interval    day weekenday
## 1     0 2012-10-01        0 Monday   weekday
## 2     0 2012-10-01        5 Monday   weekday
## 3     0 2012-10-01       10 Monday   weekday
## 4     0 2012-10-01       15 Monday   weekday
## 5     0 2012-10-01       20 Monday   weekday
## 6     0 2012-10-01       25 Monday   weekday
```

```r
        tail(FullInterval)
```

```
##       steps       date interval    day weekenday
## 17563     0 2012-11-30     2330 Friday   weekday
## 17564     0 2012-11-30     2335 Friday   weekday
## 17565     0 2012-11-30     2340 Friday   weekday
## 17566     0 2012-11-30     2345 Friday   weekday
## 17567     0 2012-11-30     2350 Friday   weekday
## 17568     0 2012-11-30     2355 Friday   weekday
```

```r
        str(FullInterval)
```

```
## 'data.frame':	17568 obs. of  5 variables:
##  $ steps    : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ date     : POSIXct, format: "2012-10-01" "2012-10-01" ...
##  $ interval : int  0 5 10 15 20 25 30 35 40 45 ...
##  $ day      : Ord.factor w/ 7 levels "Sunday"<"Monday"<..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ weekenday: Factor w/ 2 levels "weekday","weekend": 1 1 1 1 1 1 1 1 1 1 ...
```

```r
        summary(FullInterval)
```

```
##      steps          date               interval             day      
##  Min.   :  0   Min.   :2012-10-01   Min.   :   0.0   Sunday   :2304  
##  1st Qu.:  0   1st Qu.:2012-10-16   1st Qu.: 588.8   Monday   :2592  
##  Median :  0   Median :2012-10-31   Median :1177.5   Tuesday  :2592  
##  Mean   : 33   Mean   :2012-10-31   Mean   :1177.5   Wednesday:2592  
##  3rd Qu.:  8   3rd Qu.:2012-11-15   3rd Qu.:1766.2   Thursday :2592  
##  Max.   :806   Max.   :2012-11-30   Max.   :2355.0   Friday   :2592  
##                                                      Saturday :2304  
##    weekenday    
##  weekday:12960  
##  weekend: 4608  
##                 
##                 
##                 
##                 
## 
```

2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
   EnDay <- group_by(FullInterval, interval, weekenday)
        EnDayAverage <- summarize(EnDay, mean(steps))
            EnDayAverage
```

```
## Source: local data frame [576 x 3]
## Groups: interval
## 
##    interval weekenday mean(steps)
## 1         0   weekday  2.02222222
## 2         0   weekend  0.00000000
## 3         5   weekday  0.40000000
## 4         5   weekend  0.00000000
## 5        10   weekday  0.15555556
## 6        10   weekend  0.00000000
## 7        15   weekday  0.17777778
## 8        15   weekend  0.00000000
## 9        20   weekday  0.08888889
## 10       20   weekend  0.00000000
## ..      ...       ...         ...
```

```r
library(lattice)
    xyplot(`mean(steps)` ~ interval | weekenday, data = EnDayAverage, layout = c(1,2),
                type = "l", xlab = "Interval", ylab = "Number of Steps")     
```

![plot of chunk Time Series Panel Plot for WeekEnDay Averages](figure/Time Series Panel Plot for WeekEnDay Averages-1.png) 
