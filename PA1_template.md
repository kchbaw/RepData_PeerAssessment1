---
output: 
  html_document: 
    keep_md: yes
---

#Project # 1

##Load required packages.  Note, not all libraries will be used, but this is my standard library load


```
## Loading required package: Matrix
```

```
## Loaded glmnet 3.0-1
```

```
## Loading required package: greybox
```

```
## Registered S3 method overwritten by 'xts':
##   method     from
##   as.zoo.xts zoo
```

```
## Registered S3 method overwritten by 'quantmod':
##   method            from
##   as.zoo.data.frame zoo
```

```
## Registered S3 methods overwritten by 'forecast':
##   method             from    
##   fitted.fracdiff    fracdiff
##   residuals.fracdiff fracdiff
```

```
## Package "greybox", v0.5.6 loaded.
```

```
## This is package "smooth", v2.5.4
```

```
## Loading required package: lattice
```

```
## Warning: package 'lattice' was built under R version 3.6.2
```

```
## 
## Attaching package: 'caret'
```

```
## The following object is masked from 'package:greybox':
## 
##     MAE
```

```
## -- Attaching packages --------------------------------------------------------------- tidyverse 1.3.0 --
```

```
## v tibble  2.1.3     v dplyr   0.8.3
## v tidyr   1.0.0     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.4.0
## v purrr   0.3.3
```

```
## -- Conflicts ------------------------------------------------------------------ tidyverse_conflicts() --
## x tidyr::expand() masks Matrix::expand()
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
## x purrr::lift()   masks caret::lift()
## x tidyr::pack()   masks Matrix::pack()
## x tidyr::spread() masks greybox::spread()
## x tidyr::unpack() masks Matrix::unpack()
```

```
## 
## Attaching package: 'tensorflow'
```

```
## The following object is masked from 'package:caret':
## 
##     train
```

```
## 
## Attaching package: 'matrixStats'
```

```
## The following object is masked from 'package:dplyr':
## 
##     count
```

```
## Registered S3 methods overwritten by 'TSA':
##   method       from    
##   fitted.Arima forecast
##   plot.Arima   forecast
```

```
## 
## Attaching package: 'TSA'
```

```
## The following object is masked from 'package:readr':
## 
##     spec
```

```
## The following objects are masked from 'package:stats':
## 
##     acf, arima
```

```
## The following object is masked from 'package:utils':
## 
##     tar
```

```
## 
## Attaching package: 'Metrics'
```

```
## The following objects are masked from 'package:caret':
## 
##     precision, recall
```

```
## 
## Attaching package: 'TTR'
```

```
## The following object is masked from 'package:smooth':
## 
##     lags
```

```
## 
## Attaching package: 'forecast'
```

```
## The following object is masked from 'package:Metrics':
## 
##     accuracy
```

```
## ------------------------------------------------------------------------------
```

```
## You have loaded plyr after dplyr - this is likely to cause problems.
## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
## library(plyr); library(dplyr)
```

```
## ------------------------------------------------------------------------------
```

```
## 
## Attaching package: 'plyr'
```

```
## The following object is masked from 'package:matrixStats':
## 
##     count
```

```
## The following objects are masked from 'package:dplyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
```

```
## The following object is masked from 'package:purrr':
## 
##     compact
```

```
## Registered S3 method overwritten by 'GGally':
##   method from   
##   +.gg   ggplot2
```

```
## Loading required package: bitops
```

```
## 
## Attaching package: 'RCurl'
```

```
## The following object is masked from 'package:tidyr':
## 
##     complete
```

```
## Loading required package: DBI
```

```
## Loading required package: gsubfn
```

```
## Loading required package: proto
```

```
## Loading required package: RSQLite
```

```
## 
## Attaching package: 'RSQLite'
```

```
## The following object is masked from 'package:RMySQL':
## 
##     isIdCurrent
```

```
## sqldf will default to using MySQL
```

```
## 
## Attaching package: 'httr'
```

```
## The following object is masked from 'package:caret':
## 
##     progress
```

```
## 
## Attaching package: 'data.table'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last
```

```
## The following object is masked from 'package:purrr':
## 
##     transpose
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:data.table':
## 
##     hour, isoweek, mday, minute, month, quarter, second, wday, week,
##     yday, year
```

```
## The following object is masked from 'package:plyr':
## 
##     here
```

```
## The following object is masked from 'package:greybox':
## 
##     hm
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```
## Warning: package 'chron' was built under R version 3.6.2
```

```
## 
## Attaching package: 'chron'
```

```
## The following objects are masked from 'package:lubridate':
## 
##     days, hours, minutes, seconds, years
```

#Loading and preprocessing the data


```r
activity<-read.csv(file = 'C:/Users/Kevin/Desktop/Data Analytics/JHU R/Course 5/Project 1/activity.CSV',header = TRUE)
summary(activity)
```

```
##      steps                date          interval     
##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
##  Median :  0.00   2012-10-03:  288   Median :1177.5  
##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2  
##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
##  NA's   :2304     (Other)   :15840
```
Note:  there are 2304 missing values for steps  

##remove observations with missing values


```r
activity_narm<-activity %>% dplyr::filter(!is.na(activity$steps))
summary(activity_narm)
```

```
##      steps                date          interval     
##  Min.   :  0.00   2012-10-02:  288   Min.   :   0.0  
##  1st Qu.:  0.00   2012-10-03:  288   1st Qu.: 588.8  
##  Median :  0.00   2012-10-04:  288   Median :1177.5  
##  Mean   : 37.38   2012-10-05:  288   Mean   :1177.5  
##  3rd Qu.: 12.00   2012-10-06:  288   3rd Qu.:1766.2  
##  Max.   :806.00   2012-10-07:  288   Max.   :2355.0  
##                   (Other)   :13536
```
Note:  all missing values have been removed

Note: By removing the days with missing step values, there are 15264 observations in the data set activity_narm, which is equivalent of the original data set's 17568 values minus the 2304 missing values

##What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.  Therefore I will use the activity_narm data I built with no "NA's"

1.  Calculate the total number of steps taken per day  


```r
steps<-activity_narm %>% group_by(date) %>% 
        dplyr::summarise(total_steps=sum(steps))
steps
```

```
## # A tibble: 53 x 2
##    date       total_steps
##    <fct>            <int>
##  1 2012-10-02         126
##  2 2012-10-03       11352
##  3 2012-10-04       12116
##  4 2012-10-05       13294
##  5 2012-10-06       15420
##  6 2012-10-07       11015
##  7 2012-10-09       12811
##  8 2012-10-10        9900
##  9 2012-10-11       10304
## 10 2012-10-12       17382
## # ... with 43 more rows
```


2.  Make a histogram of the total number of steps taken each day  


```r
step_plot<-ggplot(steps, aes(x=total_steps))+
        geom_histogram(bins = 30)
step_plot
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->



3.  Calculate and report the mean and median of the total number of steps taken per day  


```r
steps_mean<-mean(steps$total_steps)
steps_median<-median(steps$total_steps)
steps_mean
```

```
## [1] 10766.19
```

```r
steps_median
```

```
## [1] 10765
```
##What is the average daily activity pattern?

1.  Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
activity_time<-activity %>% group_by(interval) %>% 
        dplyr::summarise(average_steps=mean(steps, na.rm=TRUE))

plot(x=activity_time$interval, y=activity_time$average_steps, type='l', ylab='average steps per 5 minute interval', xlab='5 Minute Interval')
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


2.  Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
colMax <- function(data) sapply(data, max, na.rm = TRUE)
max_col<-colMax(activity_time)
max_step<-max_col[2]
max_interval<-activity_time %>% filter (average_steps==max_step)
max_interval
```

```
## # A tibble: 1 x 2
##   interval average_steps
##      <int>         <dbl>
## 1      835          206.
```

##Imputing missing values

Note that there are a number of days/intervals where there are missing values (NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1.  Calculate and report the total number of missing values in the dataset (i.e. the total number of NAs)  

To determine the number to NAs for each variable, use summary

```r
summary(activity)
```

```
##      steps                date          interval     
##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
##  Median :  0.00   2012-10-03:  288   Median :1177.5  
##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2  
##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
##  NA's   :2304     (Other)   :15840
```

The output shows there are no NA's for date or interval, but 2304 for steps. I also determined this earlier in the assignment

2.  Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.  

I will fill in the missing values with the average steps calculated for the particular 5 minute interval. This was determined previously and the averagel valuse are stored in activity_time.


3.  Create a new dataset that is equal to the original dataset but with the missing data filled in.



```r
#removes all rows that have no 'NAs', i.e. missing values
NAR<-activity %>% dplyr::filter(is.na(activity$steps))
#NAR has 2304 observations and matches number of NAs
#replace the NA with average steps for the pariticular 5 minute interval 
for(i in 1:2304) {
     for (n in 1:288) {
           if (NAR$interval[i]==activity_time$interval[n]){
             NAR$steps[i]<-activity_time$average_steps[n]}
     }   
        
}
```

The new data set with missing values added is activity_impute created combining activity_narm (previously create, is the original data set with NAs removed) NAR (dataset with impute steps)


```r
activity_impute<-rbind(activity_narm, NAR)
summary(activity_impute)
```

```
##      steps                date          interval     
##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
##  Median :  0.00   2012-10-03:  288   Median :1177.5  
##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
##  3rd Qu.: 27.00   2012-10-05:  288   3rd Qu.:1766.2  
##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
##                   (Other)   :15840
```
Note:  acitivity_impute has 17568 values equal to the original data set. 

4.  Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?  

First, recalculate total steps


```r
steps2<-activity_impute %>% group_by(date) %>% 
        dplyr::summarise(total_steps=sum(steps))
steps2
```

```
## # A tibble: 61 x 2
##    date       total_steps
##    <fct>            <dbl>
##  1 2012-10-01      10766.
##  2 2012-10-02        126 
##  3 2012-10-03      11352 
##  4 2012-10-04      12116 
##  5 2012-10-05      13294 
##  6 2012-10-06      15420 
##  7 2012-10-07      11015 
##  8 2012-10-08      10766.
##  9 2012-10-09      12811 
## 10 2012-10-10       9900 
## # ... with 51 more rows
```
Histogram follows  



```r
step_plot2<-ggplot(steps2, aes(x=total_steps))+
        geom_histogram(bins = 30)
step_plot2
```

![](PA1_template_files/figure-html/unnamed-chunk-13-1.png)<!-- -->




```r
steps_mean2<-mean(steps2$total_steps)
steps_median2<-median(steps2$total_steps)
steps_mean2
```

```
## [1] 10766.19
```

```r
steps_median2
```

```
## [1] 10766.19
```

The mean has not changed and the median changed very slightly. The histogram appears to have barely changed (the imputed histogram max level is slightly higher).  It appears there is little difference between removing missing value and imputing values.

##Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

1.  Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.


```r
#I will change date from a character to a date format
activity_impute$date<-as.Date(activity_impute$date)

#add an variable indicating day of the week
activity_impute<-activity_impute %>% mutate(day=weekdays(date))

#change day of week to either weekend or weekday

for(i in 1:17568) {
     
           if (activity_impute$day[i]== ('Saturday')) {
            activity_impute$day[i]<-'weekend' }
  
      if (activity_impute$day[i]== ('Sunday')) {
            activity_impute$day[i]<-'weekend' }
  
    if (activity_impute$day[i]== ('Monday')) {
            activity_impute$day[i]<-'weekday' }
  
    if (activity_impute$day[i]== ('Tuesday')) {
            activity_impute$day[i]<-'weekday' }
  
    if (activity_impute$day[i]== ('Wednesday')) {
            activity_impute$day[i]<-'weekday' }
  
    if (activity_impute$day[i]== ('Thursday')) {
            activity_impute$day[i]<-'weekday' }
  
    if (activity_impute$day[i]== ('Friday')) {
            activity_impute$day[i]<-'weekday' }
  
  
        
} 


#change day to a factor
activity_impute$day<-as.factor(activity_impute$day)

summary(activity_impute)
```

```
##      steps             date               interval           day       
##  Min.   :  0.00   Min.   :2012-10-01   Min.   :   0.0   weekday:12960  
##  1st Qu.:  0.00   1st Qu.:2012-10-16   1st Qu.: 588.8   weekend: 4608  
##  Median :  0.00   Median :2012-10-31   Median :1177.5                  
##  Mean   : 37.38   Mean   :2012-10-31   Mean   :1177.5                  
##  3rd Qu.: 27.00   3rd Qu.:2012-11-15   3rd Qu.:1766.2                  
##  Max.   :806.00   Max.   :2012-11-30   Max.   :2355.0
```

2.  Make a panel plot containing a time series plot (i.e. type="l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.
 

```r
activity_impute_day<-activity_impute %>% filter(day=='weekday')
activity_impute_day<-activity_impute_day %>% group_by(interval) %>% 
        dplyr::summarise(average_steps=mean(steps))
activity_impute_day<-activity_impute_day %>% mutate(day='weekday')

activity_impute_end<-activity_impute %>% filter(day=='weekend')
activity_impute_end<-activity_impute_end %>% group_by(interval) %>% 
        dplyr::summarise(average_steps=mean(steps))
activity_impute_end<-activity_impute_end %>% mutate(day='weekend')


par(mfrow = c(2,1))
 
 plot(x=activity_impute_day$interval, y=activity_impute_day$average_steps, col='blue',type='l', main = "weekday", xlab='5-minute interval', ylab='average steps')
 
 plot(x=activity_impute_end$interval, y=activity_impute_end$average_steps,col='green',type='l', main = "weekend", xlab='5-minute interval', ylab='average steps')
```

![](PA1_template_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
 
I conclude that the weekdays have a larger peak than weekend day but overall it looks as if people are more active throughout the day on weekends


