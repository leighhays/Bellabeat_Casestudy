
![Unknown-2](https://user-images.githubusercontent.com/68507597/170092857-305d880f-ea2e-4774-bc8d-e6bee2b2bc5b.png)

# Bellabeat Casestudy
<h2>Google Data Analytics Capstone Project</h2>
<h3>By:  Leigh Hays</h3>

Bellabeat Case Study
How Can a Wellness Technology Company Play It Smart?

R - <a href = "https://kaggle.com/arashnic/fitbit">FitBit Tracker Data</a>


<h2> Summary</h2>

<h3>Company Background</h3>

Bellabeat is a tech-driven wellness company for women founded in 2013, that manufactures health-focused smart products that are beautifully designed to inform and inspire women around the world. Bellabeat technology is developed to Collect data on various health activities, sleep, stress, and reproductive health, which has allowed women to be empowered with the knowledge about their own health and habits.

# Ask

<h3> Business Task</h3>

As a junior data analyst working on the marketing analyst team at Bellabeat, I have been asked to analyze data from non-Bellabeat(FitBit) smart devices usage, in order to gain insight into how consumers are using these smart devices. These insights will help guide the marketing strategy for the company.

<h3> Key Stakeholders</h3>
    
* Urška Sršen:  Bellabeat’s co-founder and Chief Creative Officer
* Sando Mur: Mathematician and Bellabeat’s co-founder; key member of the Bellabeat executive team
* Marketing analytics team: A team of data analysts responsible for guiding Bellabeat’s marketing strategy.
    
# Prepare
    
<h3>Data Source</h3>
    
The data provided to us by Bellabeat's cofounder is the FitBit Fitness Tracker Dataset, which is a public domain made available through Mobius. The dataset is also available on Kaggle: https://kaggle.com/arashnic/fitbit

<h3>Is the Data ROCCC(Reliable, Original, Comprehensive, Current, and Cited)?</h3>

* Reliable- - The dataset only has 30 participants with no additional information on gender, age, or lifestyle.
* Original- - The data was collected from Amazon Mechanical Turk.
* Comprehensive-There is some missing information due to all users not wearing FitBit for 30 days
* Current-The data was collected in 2016. Since there have been many changes since then, it might be useful to gather more recent data for future projects
* Cited- Unknown since this was 3rd party data

# Process

Due to the size of the data sets, I will be using R Programming language to create data visualizations to share with stakeholders

<h3>Installing R Packages</h3>

In order to process the data in R, we will need to install the necessary packages

```
library(tidyverse) 
library(here) 
library(skimr)
library(dplyr)
library(ggplot2)
library(janitor)
library(lubridate)
library(corrplot)
library(tidyr)
library(stats)
library(ggplot2)
```
── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

✔ ggplot2 3.3.5     ✔ purrr   0.3.4
✔ tibble  3.1.6     ✔ dplyr   1.0.8
✔ tidyr   1.2.0     ✔ stringr 1.4.0
✔ readr   2.1.2     ✔ forcats 0.5.1

── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()

here() starts at /kaggle/working


Attaching package: ‘janitor’


The following objects are masked from ‘package:stats’:

    chisq.test, fisher.test



Attaching package: ‘lubridate’


The following objects are masked from ‘package:base’:

    date, intersect, setdiff, union


corrplot 0.92 loaded

<h3>Importing Datasets</h3>

I will be focusing solely on Activity, Sleep, and Weight for this analysis. Activity data includes information about the number of steps taken and calories burned.

```
daily_activity <- read_csv(file= "../input/fitbit/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv") 
sleep_day <- read.csv(file= "../input/fitbit/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
weight_data <- read.csv(file= "../input/fitbit/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
```

Rows: 940 Columns: 15
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr  (1): ActivityDate
dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

<h3>Data Exploration</h3>

I will use the glimpse() and head() function to examine the data more closely.

```
glimpse(daily_activity)

Rows: 940
Columns: 15
$ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
$ ActivityDate             <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/…
$ TotalSteps               <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 13019…
$ TotalDistance            <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
$ TrackerDistance          <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
$ LoggedActivitiesDistance <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
$ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…
$ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
$ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
$ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
$ VeryActiveMinutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
$ FairlyActiveMinutes      <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
$ LightlyActiveMinutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
$ SedentaryMinutes         <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
$ Calories                 <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 203…


head(daily_activity)
A tibble: 6 × 15
Id	ActivityDate	TotalSteps	TotalDistance	TrackerDistance	LoggedActivitiesDistance	VeryActiveDistance	ModeratelyActiveDistance	LightActiveDistance	SedentaryActiveDistance	VeryActiveMinutes	FairlyActiveMinutes	LightlyActiveMinutes	SedentaryMinutes	Calories
<dbl>	<chr>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>
1503960366	4/12/2016	13162	8.50	8.50	0	1.88	0.55	6.06	0	25	13	328	728	1985
1503960366	4/13/2016	10735	6.97	6.97	0	1.57	0.69	4.71	0	21	19	217	776	1797
1503960366	4/14/2016	10460	6.74	6.74	0	2.44	0.40	3.91	0	30	11	181	1218	1776
1503960366	4/15/2016	9762	6.28	6.28	0	2.14	1.26	2.83	0	29	34	209	726	1745
1503960366	4/16/2016	12669	8.16	8.16	0	2.71	0.41	5.04	0	36	10	221	773	1863
1503960366	4/17/2016	9705	6.48	6.48	0	3.19	0.78	2.51	0	38	20	164	539	1728
#Take a glimpse at the daily sleep data
glimpse(sleep_day)
Rows: 413
Columns: 5
$ Id                 <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150…
$ SleepDay           <chr> "4/12/2016 12:00:00 AM", "4/13/2016 12:00:00 AM", "…
$ TotalSleepRecords  <int> 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
$ TotalMinutesAsleep <int> 327, 384, 412, 340, 700, 304, 360, 325, 361, 430, 2…
$ TotalTimeInBed     <int> 346, 407, 442, 367, 712, 320, 377, 364, 384, 449, 3…
head(sleep_day)
A data.frame: 6 × 5
Id	SleepDay	TotalSleepRecords	TotalMinutesAsleep	TotalTimeInBed
<dbl>	<chr>	<int>	<int>	<int>
1	1503960366	4/12/2016 12:00:00 AM	1	327	346
2	1503960366	4/13/2016 12:00:00 AM	2	384	407
3	1503960366	4/15/2016 12:00:00 AM	1	412	442
4	1503960366	4/16/2016 12:00:00 AM	2	340	367
5	1503960366	4/17/2016 12:00:00 AM	1	700	712
6	1503960366	4/19/2016 12:00:00 AM	1	304	320
glimpse(weight_data)
Rows: 67
Columns: 8
$ Id             <dbl> 1503960366, 1503960366, 1927972279, 2873212765, 2873212…
$ Date           <chr> "5/2/2016 11:59:59 PM", "5/3/2016 11:59:59 PM", "4/13/2…
$ WeightKg       <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70.3, …
$ WeightPounds   <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 159.6…
$ Fat            <int> 22, NA, NA, NA, NA, 25, NA, NA, NA, NA, NA, NA, NA, NA,…
$ BMI            <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27.25,…
$ IsManualReport <chr> "True", "True", "False", "True", "True", "True", "True"…
$ LogId          <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e+12,…
head(weight_data)
A data.frame: 6 × 8
Id	Date	WeightKg	WeightPounds	Fat	BMI	IsManualReport	LogId
<dbl>	<chr>	<dbl>	<dbl>	<int>	<dbl>	<chr>	<dbl>
1	1503960366	5/2/2016 11:59:59 PM	52.6	115.9631	22	22.65	True	1.462234e+12
2	1503960366	5/3/2016 11:59:59 PM	52.6	115.9631	NA	22.65	True	1.462320e+12
3	1927972279	4/13/2016 1:08:52 AM	133.5	294.3171	NA	47.54	False	1.460510e+12
4	2873212765	4/21/2016 11:59:59 PM	56.7	125.0021	NA	21.45	True	1.461283e+12
5	2873212765	5/12/2016 11:59:59 PM	57.3	126.3249	NA	21.69	True	1.463098e+12
6	4319703577	4/17/2016 11:59:59 PM	72.4	159.6147	25	27.45	True	1.460938e+12

```
# Analyze
I will analyze the data further to make sure it is consistent and formatted correctly.

<h3> Data Summary</h3>

Next I will check to see the number of rows in each dataset

```
nrow(daily_activity)
nrow(sleep_day)
nrow(weight_data)
940
413
67
n_distinct(sleep_day$Id)
n_distinct(weight_data$Id)
n_distinct(daily_activity$Id)
24
8
33
Next I will check for any duplicates

sum(duplicated(daily_activity))
sum(duplicated(sleep_day))
sum(duplicated(weight_data))
0
3
0
```
<h3> Observations</h3>

From a quick scan of the loaded datasets the following quick observation were made

* The id column is common in all 3 datasets, and can be used to merge the datasets
* The data type of the Date variable in the 3 datasets are currently character variables and needs to be converted to Date format.
* The sleep_data and the weight_data have both date and time merged in one column and need to be be separated, as only the date variable will be used for the analysis.
* There were 33 unique users who logged in their daily activities, however, only 24 and 8 unique users logged sleep data and weight data respectively. This implies that most of these users used the device to log their daily activities, but not all of the users track their weight and sleeping habits with the device. 
* There appears to be no duplicate data in the daily_activity and weight_data, however the sleep_data has 3 duplicates, which need to be removed

<h3>Data Cleaning Steps</h3>

Will format the column names to lowercase for consistency, and change some of the column names as well

```
# Clean column names to lower case
daily_activity <- clean_names(daily_activity)
sleep_day <- clean_names(sleep_day)
weight_data <- clean_names(weight_data)

# Change the activity_date column name to 'date' in the daily_activity dataset
daily_activity <- daily_activity %>% 
  dplyr::rename(date = activity_date)

# Examine the column names
colnames(daily_activity)
'id''date''total_steps''total_distance''tracker_distance''logged_activities_distance''very_active_distance''moderately_active_distance''light_active_distance''sedentary_active_distance''very_active_minutes''fairly_active_minutes''lightly_active_minutes''sedentary_minutes''calories'
colnames(sleep_day)
'id''sleep_day''total_sleep_records''total_minutes_asleep''total_time_in_bed'
colnames(weight_data)
'id''date''weight_kg''weight_pounds''fat''bmi''is_manual_report''log_id'
# Removing duplicate date from the sleep_day
sleep_day <- distinct(sleep_day)

# Confirm that the duplicate was removed
sum(duplicated(sleep_day))
0
sum(is.na(weight_data))
65
Since there are 65 missing values from the column below, I will remove this due to insufficient data.

# remove the 'fat' column with the missing data in the weight_data and the log_id column
weight_data <- select(weight_data, -fat)
weight_data <- select(weight_data, -log_id)
# view columns
head(weight_data)
A data.frame: 6 × 6
id	date	weight_kg	weight_pounds	bmi	is_manual_report
<dbl>	<chr>	<dbl>	<dbl>	<dbl>	<chr>
1	1503960366	5/2/2016 11:59:59 PM	52.6	115.9631	22.65	True
2	1503960366	5/3/2016 11:59:59 PM	52.6	115.9631	22.65	True
3	1927972279	4/13/2016 1:08:52 AM	133.5	294.3171	47.54	False
4	2873212765	4/21/2016 11:59:59 PM	56.7	125.0021	21.45	True
5	2873212765	5/12/2016 11:59:59 PM	57.3	126.3249	21.69	True
6	4319703577	4/17/2016 11:59:59 PM	72.4	159.6147	27.45	True
# Convert the data type of the date column from character variable to date variable 
daily_activity$date <- lubridate::mdy(daily_activity$date)
daily_activity <-  mutate(daily_activity, weekday = weekdays(date))
# confirm that the data type is changed from character to date
head(daily_activity)
A tibble: 6 × 16
id	date	total_steps	total_distance	tracker_distance	logged_activities_distance	very_active_distance	moderately_active_distance	light_active_distance	sedentary_active_distance	very_active_minutes	fairly_active_minutes	lightly_active_minutes	sedentary_minutes	calories	weekday
<dbl>	<date>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<chr>
1503960366	2016-04-12	13162	8.50	8.50	0	1.88	0.55	6.06	0	25	13	328	728	1985	Tuesday
1503960366	2016-04-13	10735	6.97	6.97	0	1.57	0.69	4.71	0	21	19	217	776	1797	Wednesday
1503960366	2016-04-14	10460	6.74	6.74	0	2.44	0.40	3.91	0	30	11	181	1218	1776	Thursday
1503960366	2016-04-15	9762	6.28	6.28	0	2.14	1.26	2.83	0	29	34	209	726	1745	Friday
1503960366	2016-04-16	12669	8.16	8.16	0	2.71	0.41	5.04	0	36	10	221	773	1863	Saturday
1503960366	2016-04-17	9705	6.48	6.48	0	3.19	0.78	2.51	0	38	20	164	539	1728	Sunday
# sleep_data cleaning: separate sleep_day column to date and time column, convert the date from character variable to date format, and add weekdays column
sleep_day <- sleep_day %>%
    separate(sleep_day,c("date","time"), sep=" ") %>%
    mutate(date = mdy(date), weekday = weekdays(date)) %>%
    select(-"time")
Warning message:
“Expected 2 pieces. Additional pieces discarded in 410 rows [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].”

# 

sleep_day$weekday <- factor(sleep_day$weekday, 
                                 levels = c("Monday", "Tuesday", "Wednesday", 
                                            "Thursday", "Friday", "Saturday", 
                                            "Sunday"))

# confirm that the data type is changed from character to date format
head(sleep_day)
A data.frame: 6 × 6
id	date	total_sleep_records	total_minutes_asleep	total_time_in_bed	weekday
<dbl>	<date>	<int>	<int>	<int>	<fct>
1	1503960366	2016-04-12	1	327	346	Tuesday
2	1503960366	2016-04-13	2	384	407	Wednesday
3	1503960366	2016-04-15	1	412	442	Friday
4	1503960366	2016-04-16	2	340	367	Saturday
5	1503960366	2016-04-17	1	700	712	Sunday
6	1503960366	2016-04-19	1	304	320	Tuesday
# weight_data cleaning: separate date column to date and time column, convert the date from character variable to date format
weight_data <- weight_data %>%
  separate(date, c("date", "time"), sep = " ")%>%
  select(-"time")%>%
  mutate(date = mdy(date), weekday = weekdays(date))
Warning message:
“Expected 2 pieces. Additional pieces discarded in 67 rows [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].”
# confirm that the data type is changed from character to date
head(weight_data)
A data.frame: 6 × 7
id	date	weight_kg	weight_pounds	bmi	is_manual_report	weekday
<dbl>	<date>	<dbl>	<dbl>	<dbl>	<chr>	<chr>
1	1503960366	2016-05-02	52.6	115.9631	22.65	True	Monday
2	1503960366	2016-05-03	52.6	115.9631	22.65	True	Tuesday
3	1927972279	2016-04-13	133.5	294.3171	47.54	False	Wednesday
4	2873212765	2016-04-21	56.7	125.0021	21.45	True	Thursday
5	2873212765	2016-05-12	57.3	126.3249	21.69	True	Thursday
6	4319703577	2016-04-17	72.4	159.6147	27.45	True	Sunday
Merge Data
combined_data <- merge(daily_activity, sleep_day, by=c ('id', 'date'), all = TRUE)
head(combined_data)
A data.frame: 6 × 20
id	date	total_steps	total_distance	tracker_distance	logged_activities_distance	very_active_distance	moderately_active_distance	light_active_distance	sedentary_active_distance	very_active_minutes	fairly_active_minutes	lightly_active_minutes	sedentary_minutes	calories	weekday.x	total_sleep_records	total_minutes_asleep	total_time_in_bed	weekday.y
<dbl>	<date>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<chr>	<int>	<int>	<int>	<fct>
1	1503960366	2016-04-12	13162	8.50	8.50	0	1.88	0.55	6.06	0	25	13	328	728	1985	Tuesday	1	327	346	Tuesday
2	1503960366	2016-04-13	10735	6.97	6.97	0	1.57	0.69	4.71	0	21	19	217	776	1797	Wednesday	2	384	407	Wednesday
3	1503960366	2016-04-14	10460	6.74	6.74	0	2.44	0.40	3.91	0	30	11	181	1218	1776	Thursday	NA	NA	NA	NA
4	1503960366	2016-04-15	9762	6.28	6.28	0	2.14	1.26	2.83	0	29	34	209	726	1745	Friday	1	412	442	Friday
5	1503960366	2016-04-16	12669	8.16	8.16	0	2.71	0.41	5.04	0	36	10	221	773	1863	Saturday	2	340	367	Saturday
6	1503960366	2016-04-17	9705	6.48	6.48	0	3.19	0.78	2.51	0	38	20	164	539	1728	Sunday	1	700	712	Sunday

```


# Share

Now I will create some visualizations to share some key insights.

<h2>Visualizations</h2>

```
head(combined_data)
A data.frame: 6 × 20
id	date	total_steps	total_distance	tracker_distance	logged_activities_distance	very_active_distance	moderately_active_distance	light_active_distance	sedentary_active_distance	very_active_minutes	fairly_active_minutes	lightly_active_minutes	sedentary_minutes	calories	weekday.x	total_sleep_records	total_minutes_asleep	total_time_in_bed	weekday.y
<dbl>	<date>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<dbl>	<chr>	<int>	<int>	<int>	<fct>
1	1503960366	2016-04-12	13162	8.50	8.50	0	1.88	0.55	6.06	0	25	13	328	728	1985	Tuesday	1	327	346	Tuesday
2	1503960366	2016-04-13	10735	6.97	6.97	0	1.57	0.69	4.71	0	21	19	217	776	1797	Wednesday	2	384	407	Wednesday
3	1503960366	2016-04-14	10460	6.74	6.74	0	2.44	0.40	3.91	0	30	11	181	1218	1776	Thursday	NA	NA	NA	NA
4	1503960366	2016-04-15	9762	6.28	6.28	0	2.14	1.26	2.83	0	29	34	209	726	1745	Friday	1	412	442	Friday
5	1503960366	2016-04-16	12669	8.16	8.16	0	2.71	0.41	5.04	0	36	10	221	773	1863	Saturday	2	340	367	Saturday
6	1503960366	2016-04-17	9705	6.48	6.48	0	3.19	0.78	2.51	0	38	20	164	539	1728	Sunday	1	700	712	Sunday
```
Scatter plot to show the time in bed vs time asleep

```
ggplot(data=sleep_day, aes(x=total_minutes_asleep, y=total_time_in_bed)) + geom_point(aes(color=date))
```
![image](https://user-images.githubusercontent.com/68507597/170605595-71382ba8-29e4-47ca-abe7-ee8afe2a092d.png)


Scatter plot to show the sedentary minutes vs total steps

```
ggplot(data=daily_activity, aes(x=total_steps, y=sedentary_minutes, color = calories)) + geom_point()
```
![image](https://user-images.githubusercontent.com/68507597/170605687-6f3b1701-3550-4235-a645-40a0a626e1d0.png)

Graph to show the tracker distance usage for each day of the week

```
ggplot(data = combined_data, aes( x = weekday.x, y = tracker_distance, fill = weekday.x)) + 
  geom_bar(stat = "identity")
  ```
  ![image](https://user-images.githubusercontent.com/68507597/170605716-cb01683b-8c2a-4da6-a22d-b996cdfdf71a.png)


The dataset did not include any demographic information about the users. We can classify the users by activity considering the daily amount of steps. We can categorize users as follows: I used this article as a reference to determine how to categorize them. https://www.livestrong.com/article/401892-what-are-sedentary-moderate-high-activity-exercise-levels/

* Sedentary - Less than 5000 steps a day.
* Lightly active - Between 5000 and 7499 steps a day.
* Fairly active - Between 7500 and 9999 steps a day.
* Very active - More than 10000 steps a day.

```
daily_average <- combined_data %>%
  group_by(id) %>%
  summarise (mean_daily_steps = mean(total_steps), mean_daily_calories = mean(calories), mean_daily_sleep = mean(total_minutes_asleep))

head(daily_average)
A tibble: 6 × 4
id	mean_daily_steps	mean_daily_calories	mean_daily_sleep
<dbl>	<dbl>	<dbl>	<dbl>
1503960366	12116.742	1816.419	NA
1624580081	5743.903	1483.355	NA
1644430081	7282.967	2811.300	NA
1844505072	2580.065	1573.484	NA
1927972279	916.129	2172.806	NA
2022484408	11370.645	2509.968	NA
# Creating user categories based on daily steps
user_category <- daily_average %>%
  mutate(user_category = case_when(
    mean_daily_steps < 5000 ~ "sedentary",
    mean_daily_steps >= 5000 & mean_daily_steps < 7499 ~ "lightly active", 
    mean_daily_steps >= 7500 & mean_daily_steps < 9999 ~ "fairly active", 
    mean_daily_steps >= 10000 ~ "very active"
  ))

head(user_category)
A tibble: 6 × 5
id	mean_daily_steps	mean_daily_calories	mean_daily_sleep	user_category
<dbl>	<dbl>	<dbl>	<dbl>	<chr>
1503960366	12116.742	1816.419	NA	very active
1624580081	5743.903	1483.355	NA	lightly active
1644430081	7282.967	2811.300	NA	lightly active
1844505072	2580.065	1573.484	NA	sedentary
1927972279	916.129	2172.806	NA	sedentary
2022484408	11370.645	2509.968	NA	very active
user_category_percent <- user_category %>%
  group_by(user_category) %>%
  summarise(total = n()) %>%
  mutate(totals = sum(total)) %>%
  group_by(user_category) %>%
  summarise(total_percent = total / totals) %>%
  mutate(labels = scales::percent(total_percent))

user_category_percent$user_type <- factor(user_category_percent$user_category , levels = c("very active", "fairly active", "lightly active", "sedentary"))


head(user_category_percent)
A tibble: 4 × 4
user_category	total_percent	labels	user_type
<chr>	<dbl>	<chr>	<fct>
fairly active	0.2727273	27.3%	fairly active
lightly active	0.2727273	27.3%	lightly active
sedentary	0.2424242	24.2%	sedentary
very active	0.2121212	21.2%	very active
user_category_percent %>%
  ggplot(aes(x="",y=total_percent, fill=user_category)) +
  geom_bar(stat = "identity", width = 1)+
  coord_polar("y", start=0)+
  theme_minimal()+
  theme(axis.title.x= element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(), 
        panel.grid = element_blank(), 
        axis.ticks = element_blank(),
        axis.text.x = element_blank(),
        plot.title = element_text(hjust = 0.5, size=14, face = "bold")) +
  scale_fill_manual(values = c("#5b9aa0","#d6d4e0", "#b8a9c9", "#622569")) +
  geom_text(aes(label = labels),
            position = position_stack(vjust = 0.5))+
  labs(title="User Category distribution")
  
  ```
![image](https://user-images.githubusercontent.com/68507597/170605758-90b93a5d-974d-4563-9b51-8f0a5969b61b.png)

# Act
After reviewing the data, I will present my recommendations on how Bellabeat can use these insights to improve their Marketing stategies.

<h2>Observations</h2>

* The data we were working with is from 2016, from an Amazon Mechanical Turk survey. It might be best to gather more recent data to make sure our findings are current.
* It appears that many FitBit users do not wear their device consistently, so having a reminder might help users to remmember to wear them more often.
* They could have rewards similar to the Apple watch, that helps motivate you to reach milestones.
* There was a lot of sedentary time in this group, The CDC recommends 30 minutes of activity each day. That is why having a reminder on the device would help motivate users to stay more active.
* It appears that many users were wearing the device in bed, but not asleep. This is most likely due to using their phone before going to bed. So, they might want to set screen time limitations, so their sleep is not negatively effected by blue light before going to sleep.

<h2>Marketing Suggestions for Bellabeat</h2>

* Bellabeat can create a podcast or blog that talks about healthy lifestyle and the importance of daily exercise.
* They could have monthly incentives for meeting milestones for their users, such as free swag, or a discount towards one of their products.
* They could send a monthly newsletter with tips on how to become more active and even include healthy recipes to encourage a healthier lifestyle.

<h3> References </h3>

* Data Source:  https://www.kaggle.com/code/leighhays/bellabeat-capstone-project/data?scriptVersionId=96701939
* Articles: https://www.livestrong.com/article/401892-what-are-sedentary-moderate-high-activity-exercise-levels/
* CDC Physical Activity Guidelines - https://www.cdc.gov/physicalactivity/basics/adults/index.htm
