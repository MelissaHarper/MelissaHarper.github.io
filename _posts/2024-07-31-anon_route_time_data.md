---
layout: post
title: "Service Route Eficiency: Data Cleaning with R"
date: 31/07/2024
categories: r, data cleaning
# image: /assests/atricle_images/[post_folder]/[image] -this is the desktop image
# image2: /assests/atricle_images/[post_folder]/[image] -this is the mobile image
---



I’m working with a service company that is looking to improve their
technician’s service routes. They supply a service that provides
repeated visits at regular intervals. My goal with this project is to
help the company provide their customers with more precise scheduling
times, and give their technicians more balance in their day to day
routes. I’ll take the data they have, and provide them with an estimated
time for service at each property.

While the company does have records of the time it has previously taken
a technician to complete the service, their scheduling software does not
have the functionality to take that information and supply them with an
average service time for each property. This makes scheduling difficult
because they have no way to know how long a technician’s route will be
or when they might be arriving at properties throughout the day. Some
days a technician will have 8 properties on their route and spend 10
hours to completing them. Other days, they’ll finish 13 properties in 6
hours.

There are some challenges with the way their data is gathered as well as
retrieved from their system that make this task a bit more difficult
than running a simple code to find the average time for each property.
Here, I’ll go over the initial cleaning and address some of those
challenges encountered during the process.

## Retrieving, Importing, and Examining the Data  

The first challenge comes with accessing the data itself. While the
system appears to collect a lot of data, their options for retrieving
the data are limited. The system won’t allow for an integrated script to
automatically access the data and update their time estimates so, to
keep their information up-to-date, the reports must be run manually.
Additionally, after running several report configurations, I found many
glitches in the retrieval process as well as data that was missing
altogether. In the interest of keeping the process as simple as possible
for the company, I chose to use their payroll report as the data source.
I chose this method for several reasons. They already manually run the
report bi-monthly, it is easily accessible from the front page of their
system, and it contains the relevant data for this project.

As this is a public post, rather than importing the data as a whole,
I’ve examined the information beforehand and will exclude columns
containing identifying information during the import.

We’ll start by importing the data.

``` r
raw = fread("data/payroll_report_all.csv", drop = 2:3) # Import file, excluding columns 2 and 3.
```

We’ll take a quick look at the structure of our data.

``` r
str(raw)
```

    ## Classes 'data.table' and 'data.frame':   27388 obs. of  14 variables:
    ##  $ CustomerNumber                  : int  13700 13772 13977 14073 14079 13800 14123 14189 13963 14148 ...
    ##  $ PropertySize                    : num  0.36 0.38 0.41 0.34 0.24 0.26 0.13 0.15 0.47 0.45 ...
    ##  $ ServiceDoneDate                 : chr  "4/22/2019 12:00:00 AM" "4/22/2019 12:00:00 AM" "4/26/2019 12:00:00 AM" "4/26/2019 12:00:00 AM" ...
    ##  $ ActualManHours                  : num  0.333 0.5 0.783 0.45 0.683 ...
    ##  $ DoneByTechnicianId              : chr  "BMUDD" "BMUDD" "DMUDD" "DMUDD" ...
    ##  $ ServiceRoundCode                : chr  "S01" "S01" "S01" "N01" ...
    ##  $ ServiceRoundProductionValue     : num  69 69 69 69 69 69 69 69 69 69 ...
    ##  $ ServiceDiscountCode             : chr  "" "" "" "" ...
    ##  $ ServiceRoundDiscountAmount      : num  0 0 29 29 29 ...
    ##  $ ServiceRoundPrepayDiscountAmount: num  0 0 0 0 0 0 0 0 0 13.8 ...
    ##  $ ServiceTotalDiscount            : num  0 0 29 29 29 ...
    ##  $ ProgramStandardPrice            : num  69 69 69 69 69 69 69 69 69 69 ...
    ##  $ V15                             : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ V16                             : num  69 69 40 40 40 ...
    ##  - attr(*, ".internal.selfref")=<externalptr>

### General Observations  

We can see the the column names are quite long. The data types, overall,
look accurate, but we’ll need to change the ServiceDoneDate to a date
type. The ActualManHours, which is the column we’re most interested in
as it contains the length of time spent at each property, is listed as a
decimal of one hour.

Before making any changes to the data, we’ll make a copy and retain the
original data in its raw form.

``` r
df = raw
```

Let’s select only the columns we’ll need for this specific project and
rename them for clarity.

``` r
df = df %>%
  # Rename columns
  rename(cust_id = CustomerNumber,
         service_date = ServiceDoneDate,
         tech_id = DoneByTechnicianId,
         duration = ActualManHours,
         service_code = ServiceRoundCode
         ) %>%
  # Select only relevant columns
 select(cust_id, # Identifies the property
         service_date, # The date the service was performed on the property
         tech_id, # The tech that completed the service
         duration, # How long the tech spent performing the service
         service_code # The type of service performed
        )
```

Then change the service_date column to a date type. When examining the
structure of the data, we can see that time is included, but every
record shows a time of 12:00am, so we’ll keep only the date and get rid
of the time.

``` r
df$service_date <- as.Date(df$service_date, format = "%m/%d/%Y") # Specify the date is listed as month/day/Year excluding unnecessary time stamp.

class(df$service_date) # Returns data type
```

    ## [1] "Date"

## Cleaning and Grouping the data  

### Missing Values  

Using a combination of is.na and colSums, we’ll check how many missing
values exist in each column.

``` r
colSums(is.na(df)) # Count NA values for each column
```

    ##      cust_id service_date      tech_id     duration service_code 
    ##            0        12750            0        12750            0

We can see there are quite a lot of missing values relative to our data.
We can start to address this by continuing to narrow down our data to
only what we need.

#### Service Dates  

If a record does not have a service date, it means no services were
performed for that record. Because we are looking for the average time
it takes to complete a service, we’ll remove any records that do not
have a service date.

``` r
df = subset(df, !is.na(service_date)) # Return only rows that do not have missing values in the service_date column
```

    ## This removes the 12750 records that do not have a service date.

That is exactly the number of missing rows we saw previously in our
service_date and duration columns. Just to confirm, lets check for
missing values again.

``` r
colSums(is.na(df)) # Count NA values for each column
```

    ##      cust_id service_date      tech_id     duration service_code 
    ##            0            0            0            0            0

### Relevant Data  

This is a franchise company that changed owners within the time period
of the data we have. As some of the properties did not continue service
with the new owner, that data is not relevant to our purposes.

Filtering out those properties is made easier by the fact that all but
one tech_id from the previous company was listed with the word “MUDD” in
their tech_id’s and the other is “DM”. With this information, we can
filter out properties using the tech_id column. We’ll make a list of
employees of the previous owner, “prev_empl”, and a list of employees of
the new owner, “new_empl”. Then we’ll use these values to discard any
properties that have not been serviced by employees of the new owner
(“new_empl”).

``` r
prev_empl = c("DM", unique(keep(df$tech_id, (grepl("MUDD", df$tech_id))))) # Create a list of previous employees specifying a specific employee, DM, and extrapolating from the data table any unique tech_id that contains the word "MUDD"

new_empl <- unique(filter(df, !(tech_id %in% prev_empl))$tech_id) # Create a list of current employees by extrapolating from the data any unique tech_id that is not in the list of prev_empl


retained <- df %>%
  group_by(cust_id) %>% # Create groups on customer_id
  filter(
    any(tech_id %in% new_empl) # Keep any property serviced by new employees
  )
```

Now we have a data table containing only properties that were retained
by the new owner. We’ve also kept records of services performed on the
retained properties by previous employees so we can compare service
times across all technicians.

### Duplicate Records  

For this company, a technician performs a specific service on a property
only once per visit. However, it is common for two different services to
be performed by a technician in a single visit. When two services are
performed, the visit is recorded in the database as two records, one for
each service. Unfortunately, all of the data is duplicated for each
record, including duration. Only the column referencing the service_code
differs.

This means, if the technician spends 30 minutes at a property and
performs two services, a separate record will be created for each
service and both records will show 30 minutes in the duration column.
This makes it appear as if the technician spent 1 hour at the property
on that date when, in reality, they spent only 30 minutes performing
both services.

Because we need to know the actual time spent at the property each
visit, but also need to keep records of all services performed, we’ll
keep the first duration record for a single visit. The duration of
additional services performed on a single visit will be changed to NA.

Additionally, when familiarizing myself with the data, I found several
records showing the same service performed multiple times in one visit.
While multiple services can be performed in one visit, each unique
service can only be performed once per visit. Any record of a single
service being performed multiple times on a property on the same day is
an error. To correct that, we’ll keep only the first record of each
unique service performed in a single visit.

Finally, in future steps we’ll need make some changes to some duration
records, so I’ve chosen to duplicate that column here for future
comparisons.

``` r
retained <- retained %>%
  mutate(dup_duration = duration) %>% # Duplicate duration row for future reference
  group_by(service_date, cust_id) %>% # Group records on service date and cust_id
  mutate(duplicate = row_number() > 1) %>% # Creates a column that marks duplicate rows with the same service date and cust_id
  mutate(duration = if_else(duplicate == TRUE, NA_real_, duration)) %>% # Changes duration for any rows marked as TRUE in the previous step to NA
  ungroup() %>% # Remove groups
  distinct(service_date, cust_id, service_code, .keep_all = TRUE) %>% # Group by service date, cust_id, and service_code and remove any duplicate groups, while retaining all variables
  ungroup() %>%  # Remove groups
  select(-duplicate) # Remove duplicate column marking duplicates
```


    ## Now, our data table that started with 27388 records, has been pared down to only 8772 records that are relevant to our goals

### Correcting Technician Records  

The company informed me there were some records that were assigned to
the incorrect tech. They provided me with the following information to
correct the records.

Any records with the tech_id DTHOMPSO that occurred on a Tuesday,
Wednesday, or Thursday between 2023-07-10 and 2023-08-14 need to be
reassigned to MHMH except when the cust_id is any of the following:
105589, 57202, 58982, 139760, or 59149. However the company said there
are specific dates within those parameters in which both techs techs
provided services. As there is no pattern to reference for those days
we’ll need to manually find the specific records identified by the
company and take note of the row numbers in which the tech_id needs to
be changed. We’ll add an index column based on the row numbers, and
reference the specific numbers we took note of previously.

``` r
retained <- retained %>%
  mutate(
    is_wrong_tech = tech_id == "DTHOMPSO" & # Create new column to specify true or false conditions identified in the following
                    (weekdays(service_date) %in% c("Tuesday", "Wednesday", "Thursday")) & # Extract and specify weekdays to be corrected
                    service_date > as.Date("2023-07-10") &
                    service_date < as.Date("2023-08-14") & # Specify date range
                    !(cust_id %in% c(105589, 57202, 58982, 139760, 59149)), # Identify records that should not be corrected based on customer_id
    row_index = row_number(), # Create an index column of row numbers
    tech_id = case_when( # Identify tech_id to be changed based on the following conditions
      is_wrong_tech  ~ "MHMH", # If is_wrong_tech column is TRUE change to MHMH
      row_index %in% c(6272:6274, 6478:6481) ~ "DTHOMPSO", # These rows should be DTHOMPSO
      row_index %in% c(6701:6706) ~ "MHMH", # These rows should be assigned to MHMH
      TRUE ~ tech_id # If none of the previous conditions are met, return original tech_id
    )
  ) %>%
  select(-is_wrong_tech, -row_index) # Remove rows created for this operation
```

Now that we’ve cleaned up the data in general, we can start exploring
our duration column.

## Dealing with Outliers  

We’ll start by looking at some duration statistics.

``` r
summary(retained['duration'])
```

    ##     duration      
    ##  Min.   :-7.5667  
    ##  1st Qu.: 0.3167  
    ##  Median : 0.4500  
    ##  Mean   : 0.5078  
    ##  3rd Qu.: 0.6333  
    ##  Max.   :12.6833  
    ##  NA's   :607

We can see some pretty extreme outliers. It seems safe to say no one
spent negative 7.5667 hours at a property, so we’ll see what might be
going on.

We’ll create a dataframe showing us descriptive statistics for duration
of each property. This will give us an original reference point before
we look to change anything.

``` r
retained_duration_stats <- retained %>%
  group_by(cust_id) %>% # group by customer_id so all statistics are for individual properties
  summarize(
    count = n(), # Count how many services have been performed for each property
    min_duration = min(duration, na.rm = TRUE), # Find the min duration
    q1_duration = quantile(duration, 0.25, na.rm = TRUE), # First quartile duration
    median_duration = median(duration, na.rm = TRUE), # Find the median duration
    mean_duration = mean(duration, na.rm = TRUE), # Find the average duration
    q3_duration = quantile(duration, 0.75, na.rm = TRUE), # Third quartile duration
    max_duration = max(duration, na.rm = TRUE), # Find the max duration
    sd_duration = sd(duration, na.rm = TRUE) # Find the standard deviation
  )
```

The company confirmed that negative and 0 service times are an error and
should be removed.

``` r
duration_retained <- retained %>%
  mutate(duration = replace(duration, duration <= 0, NA)) # Remove any records less than or equal to 0
```

Sometimes a technician will forget to turn on the timer when they
arrive, or forget to turn off the timer when finished at a property.
This leaves us with incorrect durations recorded for that service date.
We’ll take the next few steps to try and weed out those values from some
known parameters.

Each property should take at least ten minutes to complete, so we’ll
change any duration that is less than 10 minutes to NA. The data is
stored as decimals of hours, so 10 minutes is represented as 0.1666

``` r
duration_retained = duration_retained %>% 
  mutate(duration = if_else(duration < 0.16666, NA_real_, duration)) # Remove any records less than 0.16666 (10 minutes)
```

Conversely, the company advised that no property should take more than 4
hours, so we’ll change any duration that exceeds 4 hours to NA.

``` r
duration_retained = duration_retained %>% 
  mutate(duration = if_else(duration > 4, NA_real_, duration)) # Remove any records greater than 4 hours
```

Now let’s see what our new descriptive statistics look like.

``` r
summary(duration_retained['duration'])
```

    ##     duration     
    ##  Min.   :0.1667  
    ##  1st Qu.:0.3333  
    ##  Median :0.4667  
    ##  Mean   :0.5298  
    ##  3rd Qu.:0.6500  
    ##  Max.   :3.9000  
    ##  NA's   :1062

We’ll create another dataframe of descriptive statistics for our new
data and take a look at the first 6 rows.

``` r
retained_mod_duration_stats <- duration_retained %>%
  group_by(cust_id) %>% # group by customer_id so all statistics are for individual properties
  summarize(
    count = n(), # Count how many services have been performed for each property
    min_duration = min(duration, na.rm = TRUE), # Find the min duration
    q1_duration = quantile(duration, 0.25, na.rm = TRUE), # First quartile duration
    median_duration = median(duration, na.rm = TRUE), # Find the median duration
    mean_duration = mean(duration, na.rm = TRUE), # Find the average duration
    q3_duration = quantile(duration, 0.75, na.rm = TRUE), # Third quartile duration
    max_duration = max(duration, na.rm = TRUE), # Find the max duration
    sd_duration = sd(duration, na.rm = TRUE) # Find the standard deviation
  )

head(retained_mod_duration_stats)
```

    ## # A tibble: 6 × 9
    ##   cust_id count min_duration q1_duration median_duration mean_duration
    ##     <int> <int>        <dbl>       <dbl>           <dbl>         <dbl>
    ## 1   13772    70        0.283       0.412           0.517         0.540
    ## 2   13800    53        0.167       0.283           0.333         0.397
    ## 3   13963    41        0.233       0.333           0.483         0.575
    ## 4   13977    47        0.233       0.333           0.517         0.576
    ## 5   14073    27        0.267       0.337           0.383         0.454
    ## 6   14079    33        0.267       0.367           0.483         0.519
    ## # ℹ 3 more variables: q3_duration <dbl>, max_duration <dbl>, sd_duration <dbl>

Now our numbers look a bit closer to what would be expected. However,
when looking at our retained_mod_duration table, we can see there are
some large differences in the amount of time spent at some properties.

While some variation is expected, and hour and a half difference like we
see with customer 13977 is not consistent with expectations.

As we said before, user error is not uncommon with service timers, so I
got the go ahead to deal with these as anomalies rather than legitimate
data points.

I’ve chosen to use z-scores as a way to deal with the outliers. So we’ll
start by calculating a z-score column.

``` r
duration_retained <- duration_retained %>%
  group_by(cust_id) %>%
  mutate(z_score_duration = scale(duration)) %>%
  select(1:7, z_score_duration, everything())
```

Then, we’ll create a function that will replace the value in the
duration column with the nearest non-outlier value grouped by cust_id
for any row that has a z-score below -3, or above 3.

``` r
# Define the function to replace outliers
replace_outliers <- function(x) {
  median_x <- median(x, na.rm = TRUE)
  median_absolute_deviation <- median(abs(x - median_x), na.rm = TRUE)
  lower_bound <- median_x - 3 * median_absolute_deviation
  upper_bound <- median_x + 3 * median_absolute_deviation
  x[x < lower_bound] <- lower_bound
  x[x > upper_bound] <- upper_bound
  x
}

# Replace outliers in the duration column based on z_score_duration on properties with more than 3 records.
normalized_duration <- duration_retained %>%
  group_by(cust_id) %>%
  mutate(duration = ifelse(n() > 3 & (z_score_duration < -3 | z_score_duration > 3),
                           replace_outliers(duration), duration))
```

We’ll create one more dataframe of descriptive statistics to take a look
at our new values.

``` r
normalized_duration_stats <- normalized_duration %>%
  group_by(cust_id) %>%
  summarize(
    count = n(),
    mean_duration = mean(duration, na.rm = TRUE),
    median_duration = median(duration, na.rm = TRUE),
    min_duration = min(duration, na.rm = TRUE),
    max_duration = max(duration, na.rm = TRUE),
    sd_duration = sd(duration, na.rm = TRUE),
    q1_duration = quantile(duration, 0.25, na.rm = TRUE),
    q3_duration = quantile(duration, 0.75, na.rm = TRUE)
  )

head(normalized_duration_stats)
```

    ## # A tibble: 6 × 9
    ##   cust_id count mean_duration median_duration min_duration max_duration
    ##     <int> <int>         <dbl>           <dbl>        <dbl>        <dbl>
    ## 1   13772    70         0.540           0.517        0.283        0.867
    ## 2   13800    53         0.389           0.333        0.167        0.817
    ## 3   13963    41         0.575           0.483        0.233        1.22 
    ## 4   13977    47         0.549           0.517        0.233        1.07 
    ## 5   14073    27         0.454           0.383        0.267        0.967
    ## 6   14079    33         0.519           0.483        0.267        0.833
    ## # ℹ 3 more variables: sd_duration <dbl>, q1_duration <dbl>, q3_duration <dbl>

Now we’ll create a data frame containing only the columns we need for
our project, calculating route times.

``` r
route_planning <- as.data.frame(subset(normalized_duration, select = c("cust_id", "service_date", "tech_id", "service_code", "duration")))
```

There’s lots more cleaning and exploring to be done for other projects.
For now, we’ve got the data we need to build a route estimator tool for
our company.
