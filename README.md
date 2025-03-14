Case Study: How Can a Wellness Technology Company Play It Smart? Using R
================
Douwe John Horsthuis
2/15/2022

# Bellabeat

![](https://github.com/DouweHorsthuis/A-Bellabeat-Case-Study-Google-Data-Analytics/blob/main/images/main_bellabeat.PNG)

## About the Company

Bellabeat is a high-tech company that manufactures health-focused smart
products and develops beautifully designed technology that informs and
inspires women around the world. Collecting data on activity, sleep,
stress, and reproductive health has allowed Bellabeat to empower women
with knowledge about their own health and habits. Since it was founded
in 2013, Bellabeat has grown rapidly and quickly positioned itself as a
tech-driven wellness company for women. By 2016, Bellabeat had opened
offices around the world and launched multiple products. Bellabeat
products became available through a growing number of online retailers
in addition to their own e-commerce channel on their website. The
company has invested in traditional advertising media, such as radio,
out-of-home billboards, print, and television, but focuses on digital
marketing extensively. Bellabeat invests year-round in Google Search,
maintaining active Facebook and Instagram pages, and consistently
engages consumers on Twitter. Additionally, Bellabeat runs video ads on
Youtube and display ads on the Google Display Network to support
campaigns around key marketing dates.

## Deliverables and Business task

**Deliverable:**

1.  A clear summary of the business task
2.  A description of all data sources used
3.  Documentation of any cleaning or manipulation of data
4.  A summary of your analysis
5.  Supporting visualizations and key findings
6.  Your top high-level content recommendations based on your analysis

**Business task:**

-   What are trends in smart devices (using Fitbit data)?

    -   How can they apply to our customers?
    -   How can these trends influence our marketing strategy?

## The data

The data used for this case study is publicly available at
<https://www.kaggle.com/arashnic/fitbit>. It contains the data generated
by 30 eligible Fitbit user that have consented to this. The data
includes minute-level output for physical activity, heart rate, and
sleep monitoring.

To access the data we downloaded it and unzipped the file containing 18
excel cvs files.

To access the data we first load the libraries we are using.

``` r
library(tidyverse)
library(dplyr)
library(ggplot2)
library(tidyr)
library(knitr)
library(magrittr)
library(gridExtra)
```

And then we load the data and take a look to get some idea of how the
data looks

``` r
d_activity    <- read_csv("data/dailyActivity_merged.csv")
d_calories    <- read.csv("data/dailyCalories_merged.csv")
d_intensities <- read.csv("data/dailyIntensities_merged.csv")
d_steps       <- read.csv("data/dailySteps_merged.csv")
s_heartrate   <- read.csv("data/heartrate_seconds_merged.csv")
h_calories    <- read.csv("data/hourlyCalories_merged.csv")
h_intensitie  <- read.csv("data/hourlyIntensities_merged.csv")
h_steps       <- read.csv("data/hourlySteps_merged.csv")
```

### The day-to-day data

First we are focusing on the daily data.

To do this we first plot the tables so we can look into the data

``` r
kable(head(d_activity))
```

|         Id | ActivityDate | TotalSteps | TotalDistance | TrackerDistance | LoggedActivitiesDistance | VeryActiveDistance | ModeratelyActiveDistance | LightActiveDistance | SedentaryActiveDistance | VeryActiveMinutes | FairlyActiveMinutes | LightlyActiveMinutes | SedentaryMinutes | Calories |
|-----------:|:-------------|-----------:|--------------:|----------------:|-------------------------:|-------------------:|-------------------------:|--------------------:|------------------------:|------------------:|--------------------:|---------------------:|-----------------:|---------:|
| 1503960366 | 4/12/2016    |      13162 |          8.50 |            8.50 |                        0 |               1.88 |                     0.55 |                6.06 |                       0 |                25 |                  13 |                  328 |              728 |     1985 |
| 1503960366 | 4/13/2016    |      10735 |          6.97 |            6.97 |                        0 |               1.57 |                     0.69 |                4.71 |                       0 |                21 |                  19 |                  217 |              776 |     1797 |
| 1503960366 | 4/14/2016    |      10460 |          6.74 |            6.74 |                        0 |               2.44 |                     0.40 |                3.91 |                       0 |                30 |                  11 |                  181 |             1218 |     1776 |
| 1503960366 | 4/15/2016    |       9762 |          6.28 |            6.28 |                        0 |               2.14 |                     1.26 |                2.83 |                       0 |                29 |                  34 |                  209 |              726 |     1745 |
| 1503960366 | 4/16/2016    |      12669 |          8.16 |            8.16 |                        0 |               2.71 |                     0.41 |                5.04 |                       0 |                36 |                  10 |                  221 |              773 |     1863 |
| 1503960366 | 4/17/2016    |       9705 |          6.48 |            6.48 |                        0 |               3.19 |                     0.78 |                2.51 |                       0 |                38 |                  20 |                  164 |              539 |     1728 |

``` r
kable(head(d_calories))
```

|         Id | ActivityDay | Calories |
|-----------:|:------------|---------:|
| 1503960366 | 4/12/2016   |     1985 |
| 1503960366 | 4/13/2016   |     1797 |
| 1503960366 | 4/14/2016   |     1776 |
| 1503960366 | 4/15/2016   |     1745 |
| 1503960366 | 4/16/2016   |     1863 |
| 1503960366 | 4/17/2016   |     1728 |

``` r
kable(head(d_steps))
```

|         Id | ActivityDay | StepTotal |
|-----------:|:------------|----------:|
| 1503960366 | 4/12/2016   |     13162 |
| 1503960366 | 4/13/2016   |     10735 |
| 1503960366 | 4/14/2016   |     10460 |
| 1503960366 | 4/15/2016   |      9762 |
| 1503960366 | 4/16/2016   |     12669 |
| 1503960366 | 4/17/2016   |      9705 |

``` r
kable(head(d_intensities))
```

|         Id | ActivityDay | SedentaryMinutes | LightlyActiveMinutes | FairlyActiveMinutes | VeryActiveMinutes | SedentaryActiveDistance | LightActiveDistance | ModeratelyActiveDistance | VeryActiveDistance |
|-----------:|:------------|-----------------:|---------------------:|--------------------:|------------------:|------------------------:|--------------------:|-------------------------:|-------------------:|
| 1503960366 | 4/12/2016   |              728 |                  328 |                  13 |                25 |                       0 |                6.06 |                     0.55 |               1.88 |
| 1503960366 | 4/13/2016   |              776 |                  217 |                  19 |                21 |                       0 |                4.71 |                     0.69 |               1.57 |
| 1503960366 | 4/14/2016   |             1218 |                  181 |                  11 |                30 |                       0 |                3.91 |                     0.40 |               2.44 |
| 1503960366 | 4/15/2016   |              726 |                  209 |                  34 |                29 |                       0 |                2.83 |                     1.26 |               2.14 |
| 1503960366 | 4/16/2016   |              773 |                  221 |                  10 |                36 |                       0 |                5.04 |                     0.41 |               2.71 |
| 1503960366 | 4/17/2016   |              539 |                  164 |                  20 |                38 |                       0 |                2.51 |                     0.78 |               3.19 |

It seems like the d\_calories and the d\_steps data frames are part of
the d\_activity data frame. We will test this by using the identical
function:

``` r
identical(as.integer(d_activity[['Calories']]),d_calories[['Calories']])
```

    ## [1] TRUE

``` r
identical(as.integer(d_activity[['TotalSteps']]),d_steps[['StepTotal']])
```

    ## [1] TRUE

After finding out we don’t need to use the smaller data frames, we
compare all the columns in the d\_intensities data frame:

``` r
identical(as.integer(d_activity[['SedentaryMinutes']]),d_intensities[['SedentaryMinutes']])
```

    ## [1] TRUE

``` r
identical(as.integer(d_activity[['LightlyActiveMinutes']]),d_intensities[['LightlyActiveMinutes']])
```

    ## [1] TRUE

``` r
identical(as.integer(d_activity[['FairlyActiveMinutes']]),d_intensities[['FairlyActiveMinutes']])
```

    ## [1] TRUE

``` r
identical(as.integer(d_activity[['VeryActiveMinutes']]),d_intensities[['VeryActiveMinutes']])
```

    ## [1] TRUE

``` r
identical(d_activity[['SedentaryActiveDistance']],d_intensities[['SedentaryActiveDistance']])
```

    ## [1] TRUE

``` r
identical(d_activity[['LightActiveDistance']],d_intensities[['LightActiveDistance']])
```

    ## [1] TRUE

``` r
identical(d_activity[['ModeratelyActiveDistance']],d_intensities[['ModeratelyActiveDistance']])
```

    ## [1] TRUE

``` r
identical(d_activity[['VeryActiveDistance']],d_intensities[['VeryActiveDistance']])
```

    ## [1] TRUE

This shows that all the data from all the other excel files are
summarized in the d\_activity files. So instead of focusing on multiple
data frames we only need to look at this one. This might be worth
bringing up later, because saving extra versions/files of the same data
costs extra storage space and it would be better to only keep what is
needed, unless there is a reason for needing the smaller data frames.

Looking at the ActivityDate variable, it is clear that R won’t see this
as a date, but instead as a character. We need to fix this by using the
`as.Date(d_activity$ActivityDate, "%m/%d/%Y")` The same is the case for
Id. Id is now of the category num and should be switch to for example
string.  
Later in the case we’ll need to do the same for the h\_intensitie data
so we do that too here.

``` r
d_activity$ActivityDate<-as.Date(d_activity$ActivityDate, "%m/%d/%Y")
d_activity$Id<-as.character(d_activity$Id)
h_intensitie$ActivityHour<-as.POSIXct(h_intensitie$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
h_intensitie$Time<-format(h_intensitie$ActivityHour, format = "%H:%M:%S")
h_intensitie$Date<-as.Date(h_intensitie$ActivityHour, format = "%m/%d/%y")
h_intensitie <- subset(h_intensitie, select = -c(ActivityHour) )
h_intensitie$Id<-as.character(h_intensitie$Id)
n_distinct(d_activity$Id)
```

    ## [1] 33

``` r
n_distinct(d_activity$ActivityDate)
```

    ## [1] 31

``` r
d_activity %>%  
  select(TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()
```

    ##    TotalSteps    TotalDistance    TrackerDistance  LoggedActivitiesDistance
    ##  Min.   :    0   Min.   : 0.000   Min.   : 0.000   Min.   :0.0000          
    ##  1st Qu.: 3790   1st Qu.: 2.620   1st Qu.: 2.620   1st Qu.:0.0000          
    ##  Median : 7406   Median : 5.245   Median : 5.245   Median :0.0000          
    ##  Mean   : 7638   Mean   : 5.490   Mean   : 5.475   Mean   :0.1082          
    ##  3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.: 7.710   3rd Qu.:0.0000          
    ##  Max.   :36019   Max.   :28.030   Max.   :28.030   Max.   :4.9421          
    ##  VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes
    ##  Min.   :  0.00    Min.   :  0.00      Min.   :  0.0       
    ##  1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:127.0       
    ##  Median :  4.00    Median :  6.00      Median :199.0       
    ##  Mean   : 21.16    Mean   : 13.56      Mean   :192.8       
    ##  3rd Qu.: 32.00    3rd Qu.: 19.00      3rd Qu.:264.0       
    ##  Max.   :210.00    Max.   :143.00      Max.   :518.0

This tells us that there are 33 unique people in this database and that
there are 31 days of data collected. When looking at the summary, we see
that total distance and Tracker Distance are almost the same. We see
that almost no activities are logged. When comparing the 3 intensities,
it is clear that by far most activity falls withing the
LigthlyActiveMinutes.

### Total distance by date

Now that we have some idea of the state of the data, we plot the data.
To see if this gives us some insights

``` r
ggplot(data=d_activity)+
  geom_point(mapping=aes(x=ActivityDate, y=TotalDistance)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=TotalDistance)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20)) +
  ggtitle("Total distance by date")+
  xlab("Time")+
  ylab("Distance")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Plotting%20total%20distance%20vs%20date-1.png)<!-- -->

This plot shows us that there is a very small decrease in distance after
a while. Which can mean that after a while the novelty wears off and
people’s enthusiasms decreases, which results in less actives. It can
also be the case that people changed to an different exercise that
requires staying in one space. To test this we create the same plot but
using calories instead of Total distance

### Potential decrease in calories over time

``` r
ggplot(data=d_activity)+
  geom_point(mapping=aes(x=ActivityDate, y=Calories)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=Calories)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  ggtitle("Potential decrease in calories over time")+
  xlab("Time")+
  ylab("Total calories")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Plotting%20total%20calories%20vs%20date-1.png)<!-- -->

If we would only look at the average, than this plot confirms that
people are also burning less calories. And since Calories would be
burned for all types of actives, **this might indicate that people
simply stop doing as much exercise after a while**. This might be an
important measure for the company to know. **However, when we look at
the individual data points, we see something odd.** It seems like the
last day, everyone is doing less. This could be cause by several things
when collecting data, including having the same time frame (instead of
24h maybe only data until 12Pm was used) or maybe it was a holiday
(people might move different). To be clear we will plot the same data
without the last day.

### Excluding last dates

``` r
c_plot1<-ggplot(data=d_activity)+
  geom_point(mapping=aes(x=ActivityDate, y=Calories)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=Calories)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  ggtitle("Total calories by all dates\n")+
  xlab("Time")+
  ylab("Total calories")

c_plot2<-ggplot(data=d_activity)+
  geom_point(mapping=aes(x=ActivityDate, y=Calories)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=Calories)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  ggtitle("Total calories by date \nexcept for the last")+
  xlab("Time")+
  ylab("Total calories")

grid.arrange(c_plot1, c_plot2, ncol=2)
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Plotting%20total%20calories%20vs%20date%20but%20restricking%20date-1.png)<!-- -->

This shows us that actually, there is no drop towards the end. This is
only the case by the last day’s data and are not representative of a
trend. To make sure that we are not looking at the last day we will use
a limit for the x-axis
`xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))`

Another point of interest might be to compare what intensity of activity
is done more often. This can be looked at by comparing the
Lightly/fairly/VeryActiveMinutes variables. This could give some insight
in what group of athletes is most likely to buy a fitbit.

### Different intensities of excersise

``` r
diff_int_plot <- ggplot(data=d_activity)+
  geom_smooth(mapping=aes(x=ActivityDate, y=LightlyActiveMinutes, colour="Light Activity")) +
  geom_point(mapping=aes(x=ActivityDate, y=LightlyActiveMinutes, colour="Light Activity"))+
  geom_smooth(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, colour="Medium Activity")) +
  geom_point(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, colour="Medium Activity")) +
  geom_smooth(mapping=aes(x=ActivityDate, y=VeryActiveMinutes, colour="Heavy Activty")) +
  geom_point(mapping=aes(x=ActivityDate, y=VeryActiveMinutes, colour="Heavy Activty")) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  theme_light()+
  ggtitle("Different intensities of excersise")+
  xlab("Time")+
  ylab("Minutes of activity")

diff_int_plot
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Comparing%20different%20intensity%20levels-1.png)<!-- -->

**This plot shows that the majority of the people do Light activity**.
It also shows that, because the majority of the minutes logged are light
activities, medium and heavy activities do not influence the main data.
This would mean that when we talk about average data, we are actually
almost ignoring the impact of these to types.

We also want to see how the data looks when we look at individuals. For
this we can use the Id variable

### Activities logged per ID number

``` r
  ggplot(data=d_activity)+
  geom_point(mapping=aes(x=ActivityDate, y=Id, color=Id)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  ggtitle("Activities logged per ID number")+
  xlab("Time")+
  ylab("ID numbers")+
  theme(legend.position = "none")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/results%20by%20Id-1.png)<!-- -->

This is informative because it shows that out of the 33 people 12 people
have not been wearing their device until the end. This is also a
potential indicator that people lose interested after some time.

This might make it worth to look at the only the data of the people that
are left over. For this we need to create a new data frame. We start by
figuring out who still has data during the last day (2016-5-12)

### Splitting the data

``` r
n_distinct(d_activity$Id)
```

    ## [1] 33

``` r
#creating an empty data frame for the active people
mat = matrix(ncol = 0, nrow = 1)
active_people=data.frame(mat)
last_date<- as.Date('2016-05-11')
ii=1
for(i in 1:nrow(d_activity)) { 
  if(d_activity[i,2] > last_date){
  active_people[ii]<- d_activity[i,1]
  ii<-ii+1}
}

length(active_people)
```

    ## [1] 21

``` r
#using the ID of the active people to create new variables
activity_in_use<- filter(d_activity, Id %in% active_people)
activity_not_in_use<- filter(d_activity, !(Id %in% active_people))
n_distinct(activity_not_in_use$Id)
```

    ## [1] 12

The outcome here says that in total we have 33 people, of those 33 21
have collected data until the end and only 12 have not completed the
whole collection. Now that we have all the people that are still active,
we can look at their data to see if it’s different from everyone grouped
together.

### Comparing 2 groups on group level

``` r
plot1<-ggplot(data=activity_in_use)+
  geom_point(mapping=aes(x=ActivityDate, y=Calories))+
  geom_smooth(mapping=aes(x=ActivityDate, y=Calories))+
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  coord_cartesian(ylim = c(0, 4500))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10')) +
  ggtitle("Active people \nCalories over time")+
  xlab("Time")+
  ylab("Calories")

plot2<-  ggplot(data=activity_not_in_use)+
  geom_point(mapping=aes(x=ActivityDate, y=Calories))+
  geom_smooth(mapping=aes(x=ActivityDate, y=Calories))+
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  coord_cartesian(ylim = c(0, 4500))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Non-active people \nCalories over time")+
  xlab("Time")+
  ylab("Calories")

plot3<-ggplot(data=activity_in_use)+
  geom_point(mapping=aes(x=ActivityDate, y=TotalSteps))+
  geom_smooth(mapping=aes(x=ActivityDate, y=TotalSteps))+
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  coord_cartesian(ylim = c(0, 25000))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Active people \nTotal steps over time")+
  xlab("Time")+
  ylab("Steps")


plot4<-  ggplot(data=activity_not_in_use)+
  geom_point(mapping=aes(x=ActivityDate, y=TotalSteps))+
  geom_smooth(mapping=aes(x=ActivityDate, y=TotalSteps))+
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  coord_cartesian(ylim = c(0, 25000))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Non-active people \nTotal steps over time")+
  xlab("Time")+
  ylab("Steps") 

plot5<- ggplot(data=activity_in_use)+
  geom_point(mapping=aes(x=ActivityDate, y=VeryActiveMinutes, colour="Heavy Activity")) +
  geom_point(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, colour="Medium Activity")) +
  geom_smooth(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, colour="Medium Activity")) +
  geom_smooth(mapping=aes(x=ActivityDate, y=VeryActiveMinutes, colour="Heavy Activity")) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  theme(legend.position = c(0.78, 0.86) ,legend.background = element_blank(), legend.box.background = element_rect(colour = "black"))+
  coord_cartesian(ylim = c(0, 150))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Active people \nMedium and Heavy \nactivity")+
  xlab("Time")+
  ylab("Minutes of activity") 

plot6<- ggplot(data=activity_not_in_use)+
  geom_point(mapping=aes(x=ActivityDate, y=VeryActiveMinutes, colour="Heavy Activity")) +
  geom_point(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, colour="Medium Activity")) +
  geom_smooth(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, colour="Medium Activity")) +
  geom_smooth(mapping=aes(x=ActivityDate, y=VeryActiveMinutes, colour="Heavy Activity")) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  theme(legend.position = c(0.78, 0.86),legend.background = element_blank(), legend.box.background = element_rect(colour = "black") )+
  coord_cartesian(ylim = c(0, 150))+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Non-active people \nMedium and Heavy \nactivity")+
  xlab("Time")+
  ylab("Minutes of activity") 

grid.arrange(plot1, plot2, ncol=2)
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Comparing%20different%20intensity%20levels%20of%20active%20people-1.png)<!-- -->

``` r
grid.arrange(plot3, plot4, ncol=2)
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Comparing%20different%20intensity%20levels%20of%20active%20people-2.png)<!-- -->

``` r
grid.arrange(plot5, plot6, ncol=2)
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/Comparing%20different%20intensity%20levels%20of%20active%20people-3.png)<!-- -->

It seems in general that both groups behave similar in most aspects, the
re-occurring trend is that the group that stops recording also seem to
drop a little more. Since the line we base this one is the average, it’s
not caused by a change in the total of the group, but rather by the
individuals actually doing less.

The thing to look into, for this data set, might be how individual
people behave over time. For this we plot calories over time, this
seemed to be a sensitive measure. But instead of plotting it as we did
before, we now want to look at the individual:

### Comparing both groups on individual level as one

``` r
ggplot(data=d_activity)+
  #geom_point(mapping=aes(x=ActivityDate, y=Calories, color=Id)) +
  geom_smooth( se=FALSE,mapping=aes(x=ActivityDate, y=TotalSteps, color=Id)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20),legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Total steps by date")+
  xlab("Time")+
  ylab("Total steps")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/individual%20Steps-1.png)<!-- -->

I think this might shows a very interesting pattern. **It seems that
everyone that is dropping out first has a couple of days where the steps
drop**. Maybe because they exercise less, or maybe just because they
aren’t wearing the device all the time. **This could be a good moment to
alert the customer**. To be sure about this, we plot the still active
and people that dropped out separately.

### Comparing both groups on individual level separated

``` r
plot7<-ggplot(data=activity_in_use)+
  #geom_point(mapping=aes(x=ActivityDate, y=Calories, color=Id)) +
  geom_smooth( se=FALSE,mapping=aes(x=ActivityDate, y=TotalSteps, group=Id, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=TotalSteps)) +
  coord_cartesian(ylim = c(0, 20000))+
  theme_light()+
  theme(axis.text.x = element_text(angle = 20),legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Active group")+
  xlab("Time")+
  ylab("Total steps")

plot8<-ggplot(data=activity_not_in_use)+
  #geom_point(mapping=aes(x=ActivityDate, y=Calories, color=Id)) +
  geom_smooth( se=FALSE,mapping=aes(x=ActivityDate, y=TotalSteps, group=Id, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=TotalSteps)) +
  coord_cartesian(ylim = c(0, 20000))+
  theme_light()+
  theme(axis.text.x = element_text(angle = 20),legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-10'))+
  ggtitle("Non-active group")+
  xlab("Time")+
  ylab("Total steps")

grid.arrange(plot7, plot8, ncol=2, top="Individual steps per ID")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/comparing%20both%20groups%20on%20individual%20level%20separated-1.png)<!-- -->

These plots confirms that often before people drop out, there is a drop
off of activity. However, it’s also not a 100% given that everyone who
drops will stop wearing it, or that people who don’t drop won’t stop
wearing it. **It also shows that people’s patterns aren’t super
stable.**

### Activity per individual per activity level

``` r
plot9<-ggplot(data=activity_in_use)+
  geom_smooth(se=FALSE,mapping=aes(x=ActivityDate, y=LightlyActiveMinutes, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=LightlyActiveMinutes)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20),legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  coord_cartesian(ylim = c(0, 400))+
  ggtitle("Active people")+
  xlab("Time")+
  ylab("Minutes of activity")

plot10<-ggplot(data=activity_not_in_use)+
  geom_smooth(se=FALSE,mapping=aes(x=ActivityDate, y=LightlyActiveMinutes, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=LightlyActiveMinutes)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  theme(legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  coord_cartesian(ylim = c(0, 400))+
  ggtitle("Non-active people")+
  xlab("Time")+
  ylab("Minutes of activity")

plot11<-ggplot(data=activity_in_use)+
  geom_smooth(se=FALSE,mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20),legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  coord_cartesian(ylim = c(0, 90))+
  ggtitle("Active people")+
  xlab("Time")+
  ylab("Minutes of activity")

plot12<-ggplot(data=activity_not_in_use)+
  geom_smooth(se=FALSE,mapping=aes(x=ActivityDate, y=FairlyActiveMinutes, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=FairlyActiveMinutes)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  theme(legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  coord_cartesian(ylim = c(0, 90))+
  ggtitle("Non-active people")+
  xlab("Time")+
  ylab("Minutes of activity")

plot13<-ggplot(data=activity_in_use)+
  geom_smooth(se=FALSE,mapping=aes(x=ActivityDate, y=VeryActiveMinutes, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=VeryActiveMinutes)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20),legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  coord_cartesian(ylim = c(0, 110))+
  ggtitle("Active people")+
  xlab("Time")+
  ylab("Minutes of activity")

plot14<-ggplot(data=activity_not_in_use)+
  geom_smooth(se=FALSE,mapping=aes(x=ActivityDate, y=VeryActiveMinutes, color=Id)) +
  geom_smooth(mapping=aes(x=ActivityDate, y=VeryActiveMinutes)) +
  theme_light()+
  theme(axis.text.x = element_text(angle = 20))+
  theme(legend.position = "none")+
  xlim(as.Date('2016-04-10'), as.Date('2016-05-11'))+
  coord_cartesian(ylim = c(0, 110))+
  ggtitle("Non-active people")+
  xlab("Time")+
  ylab("Minutes of activity")

grid.arrange(plot9, plot10, ncol=2, top="Light activity")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/activity%20per%20individual%20per%20activity%20level-1.png)<!-- -->

``` r
grid.arrange(plot11, plot12, ncol=2, top="Medium activity")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/activity%20per%20individual%20per%20activity%20level-2.png)<!-- -->

``` r
grid.arrange(plot13, plot14, ncol=2, top="Heavy activity")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/activity%20per%20individual%20per%20activity%20level-3.png)<!-- -->

Here we see that if we divide the 2 groups on the type of there is no
really obvious pattern between the groups. In all 3 the variations we in
both groups people around 0 and around the highest point. There is an
odd moment where someone goes in the minus minutes, but non of this
could help Bellabeat.

## The personal data

First we want to check the quality of the data. We noticed that all the
dates were in the wrong format. Here we fix that and look how many
people are in the data sets. In this case we will instantly also exclude
the faulty last day of data collection.

``` r
n_distinct(s_heartrate$Id) #will give us the amount of people
```

    ## [1] 14

``` r
n_distinct(h_calories$Id) #will give us the amount of people
```

    ## [1] 33

``` r
n_distinct(h_steps$Id) #will give us the amount of people
```

    ## [1] 33

``` r
n_distinct(h_intensitie$Id) #will give us the amount of people
```

    ## [1] 33

``` r
h_intensitie <- filter(h_intensitie, Date <= "2016-05-10")
```

Base on the amount of participants, we see that the hearth rate data
does not have everyone’s data. So it might be better to not focus on
that one. Instead lets look at the hourly calorie data. This data is
very similar to the data before, we just have more data points. So
instead let’s average and look at it on an hourly bases. While doing
this let’s try to compare the active and not active people.

### Hourly data

``` r
intensitie_in_use<- filter(h_intensitie, Id %in% active_people)
intensitie_not_in_use<- filter(h_intensitie, !(Id %in% active_people))

int_acti_hour <- intensitie_in_use %>%
  group_by(Time) %>%
  drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

int_not_acti_hour <- intensitie_not_in_use %>%
  group_by(Time) %>%
  drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

plot15 <-ggplot(data=int_acti_hour, aes(x=Time, y=mean_total_int)) + 
  geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Active people")

plot16 <- ggplot(data=int_not_acti_hour, aes(x=Time, y=mean_total_int)) + 
  geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Non-active people")

grid.arrange(plot15, plot16, ncol=2, top="Average Total Intensity vs. Time")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/intensitie%20on%20hourly%20bases%20grouped-1.png)<!-- -->
On a group level there doesn’t seem to be a clear difference per hour
between the groups.

But maybe if we look at an individual level we might find something. For
this we will choose randomly 3 people from each group and plot their
data.

### Individual hourly data

``` r
random_person_1 <- sample(1:21, 3) #3 random numbers that are not repeated 
intensitie_act1<- filter(intensitie_in_use, Id==active_people[,random_person_1[1]])
intensitie_act2<- filter(intensitie_in_use, Id==active_people[,random_person_1[2]])
intensitie_act3<- filter(intensitie_in_use, Id==active_people[,random_person_1[3]])
#same for the not active people
non_active_people <- unique(activity_not_in_use$Id)
random_person_2 <- sample(1:12, 3) #3 random numbers that are not repeated 
intensitie_no_act1<- filter(intensitie_not_in_use, Id==non_active_people[random_person_2[1]])
intensitie_no_act2<- filter(intensitie_not_in_use, Id==non_active_people[random_person_2[2]])
intensitie_no_act3<- filter(intensitie_not_in_use, Id==non_active_people[random_person_2[3]])


active_1 <- intensitie_act1 %>%
  group_by(Time) %>%
  #drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

active_2 <- intensitie_act2 %>%
  group_by(Time) %>%
  #drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

active_3 <- intensitie_act3 %>%
  group_by(Time) %>%
  #drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

n_active_1 <- intensitie_no_act1 %>%
  group_by(Time) %>%
  #drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

n_active_2 <- intensitie_no_act2 %>%
  group_by(Time) %>%
  #drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

n_active_3 <- intensitie_no_act3 %>%
  group_by(Time) %>%
  #drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

plot17 <-ggplot(data=active_1, aes(x=Time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 45)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Active person 1")

plot18 <-ggplot(data=active_2, aes(x=Time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Active person 2")

plot19 <-ggplot(data=active_3, aes(x=Time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Active person 3")

plot20 <-ggplot(data=n_active_1, aes(x=Time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Non-active person 1")

plot21 <-ggplot(data=n_active_2, aes(x=Time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Non-active person 2")

plot22 <-ggplot(data=n_active_3, aes(x=Time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
  theme_light()+
  theme(axis.text.x = element_text(angle = 90)) +
  coord_cartesian(ylim = c(0, 25))+
  labs(title="Non-active person 3")

grid.arrange(plot17, plot18, plot19, ncol=3, top="Average Total Intensity vs. Time")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/intensitie%20on%20hourly%20bases%20individual-1.png)<!-- -->

``` r
grid.arrange(plot20, plot21, plot22, ncol=3, top="Average Total Intensity vs. Time")
```

![](Bellabeat-Case-Study-with-R_files/figure-gfm/intensitie%20on%20hourly%20bases%20individual-2.png)<!-- -->

I don’t see a pattern based on this data that can help distinguished
what person would be more likely to drop out. However it does show that
as the previous plots showed most people don’t exercise between 23:00
and 04:00, but this is not true for everyone.

# Case study results

## Analysis results

This case study has yielded a couple of results. There were 33 people in
the data set but not everyone completed it. This was interesting because
this let us to somewhat understand the pattern that showed before people
would drop out. When we looking at the plot below, it shows that before
most people don’t record, they have a drop of activity.
![](Bellabeat-Case-Study-with-R_files/figure-gfm/replotting%20plot%207%20and%208-1.png)<!-- -->

Another thing that this plot shows is that most people are not stable
when it comes to steps. The total amount goes up and down for most
people.

It is not clear if the “light activity” is actual exercise or if that is
just everything that doesn’t reach the minimum threshold for “Fairly
Active”. So it’s possible that every step counts towards light activity
and only if for example the hearth rate goes up it adds to one of the
other 2. This means that when we don’t separate this data, we might
mainly be talking about the average movement during the day and not
specifically about exercise. When looking at the average intensity per
hour, it seems like people (as expected) don’t exercise between 22:00
and 04:00. But when we look at individuals, it seems not entirely clear
if there is a pattern for everyone. To look into this better it would
maybe be better to filter out the low intensity data. Because it’s clear
that this drives the majority of results.
![](Bellabeat-Case-Study-with-R_files/figure-gfm/plotting%20different%20intensities%20again-1.png)<!-- -->

## Answers to the Busniness task and recommendations

**What are trends in smart devices (using Fitbit data)?**

People that stop recording seem to often have a steep drop in activity.
Based on the patterns that we see in the data, we see that people on
average have a pretty stable amount of steps/calories that get burned.
However, when we look on a personal level this seems to not be true at
all. Everyone fluctuates pretty strongly. More importantly, it seems
that people first have a drop in data before they completely stop
recording. It seems like people fall into different groups. Very few
people exercise sometimes a lot an sometimes not at all.

### First recommendation

It might be useful to come up with alarms that that encourage people to
resume activities before they drop so far that they might stop all
together. One could choose to follow the suggestions by [Fishbach A.,
Eyal T., Finkelstein S.
(2010)](http://www.communicationcache.com/uploads/1/0/8/8/10887248/how_positive_and_negative_feedback_motivate_goal_pursuit.pdf),
they say that novices prefer and are more sensitive to positive
feedback, where experts are more likely to look for negative feedback.
However when dealing with costumers negative feedback might not be
ideal.

### Second recommendation

To prevent fluctuation it might be worth it to try to set personal
goals. Instead of making everyone instantly go for the [recommend 10000
steps by the
CDC](https://www.cdc.gov/diabetes/prevention/pdf/postcurriculum_session8.pdf)
it might be a good idea to build up to this. And based on goals/step
reached by the person increase this. However, these goals need to be
clear, high enough that it requires effort as explained in by [Lock E.,
Latham G.
(2006)](https://journals.sagepub.com/doi/full/10.1111/j.1467-8721.2006.00449.x?casa_token=3YGVcoJa2yEAAAAA%3AVHucZcxZxkz1VUpfYQZAK0X1UFM6_i2XDGhNV2B68RT9GH-4EV1owHZOcRGURfV0bfs9erOOfQ9B).

### Third recommendation

Split people up in groups, it seems that when we look at the activity
plots below there are people that are way more active than others. These
people are maybe looking for different things. If people are able to
communicate they might be able to motivate each other.
![](Bellabeat-Case-Study-with-R_files/figure-gfm/plotting%20activity%20again-1.png)<!-- -->![](Bellabeat-Case-Study-with-R_files/figure-gfm/plotting%20activity%20again-2.png)<!-- -->![](Bellabeat-Case-Study-with-R_files/figure-gfm/plotting%20activity%20again-3.png)<!-- -->

## Marketing strategy suggestions

We see that most people do light activity. So most people will be
impacted by anything related to simple steps. If, as suggested in the
last recommendation, people are split up into groups, it will be easier
to target people with relevant ads and information.
