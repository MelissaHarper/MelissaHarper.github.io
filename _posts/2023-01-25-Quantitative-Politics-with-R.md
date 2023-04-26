---
layout: post
title: "2023-01-25 Quantitative Politics with R"
author: "Harper"
categories: practice
tag: R
---



## R Data Exercises

I've been looking to practice some of the basics of R programming. In this post, I'll be practicing creating data through some simple web scraping using "Quantitative Politics with R" by Erik Gahner Larsen and Zoltán Fazekas. The goal is to showcase the code that gets us to the results.

## Create data by webscraping online files  

Let's begin by testing a single election file from 1955 and inspecting the data.  


```r
el_1955 <- read_delim(
  "https://www.electoralcalculus.co.uk/electdata_1955.txt",
  delim = ";"
)
```

```
## Rows: 630 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(el_1955)
```

```
## # A tibble: 6 × 11
##   Name            MP     Area County Elect…¹   CON   LAB   LIB   NAT   MIN   OTH
##   <chr>           <chr> <dbl> <chr>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
## 1 Abingdon        AMS …    12 Berks…   58487 25613 16979  8634     0     0     0
## 2 Accrington      H Hy…     4 Lanca…   50938 21157 22502     0     0     0     0
## 3 Acton           JA S…    11 Ealing   49373 20120 20645     0     0     0     0
## 4 Aldershot       E Er…    12 Hamps…   54209 22701 13129  4232     0     0     0
## 5 Altrincham and… FJ E…     4 Centr…   61525 30586 12174  6436     0     0     0
## 6 Arundel and Sh… HB K…    12 West …   69034 35180 15188     0     0     0     0
## # … with abbreviated variable name ¹​Electorate
```
  
We can see that the above data loaded correctly, so now we can set up a function to scrape the files for the election years we are interested in.  

First, we'll make a list of the election years we'd like to collect data from.  


```r
election_years <- c("1955", "1959", "1964", "1966", "1970", "1974feb",
                    "1974oct", "1979", "1983", "1987", "1992ob", "1997",
                    "2001ob", "2005ob", "2010", "2015", "2017")
```
  
Next we'll put together the function to scrape data. We'll use past0() to concatenate our url, year, and file type and read_delim() for the year we specify.  


```r
read_election_data <- function(election) {
  url <- paste0("http://www.electoralcalculus.co.uk/electdata_",
                election, ".txt")
  read_delim(url, delim = ";") %>%
    mutate(year = election)
}
```
  
Now that we've created the function, we can use lapply() to run the function for all the years we previously specified and bind_rows() to put them all together. Then we'll inspect the results.  


```r
elections <- bind_rows(lapply(election_years, read_election_data))  
```

```
## Rows: 630 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 630 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 630 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 630 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 630 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 635 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 635 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 635 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 650 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 650 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 651 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 659 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 659 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 646 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 650 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr (3): Name, MP, County
## dbl (8): Area, Electorate, CON, LAB, LIB, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 650 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr  (3): Name, MP, County
## dbl (10): Area, Electorate, CON, LAB, LIB, UKIP, Green, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 650 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ";"
## chr  (3): Name, MP, County
## dbl (10): Area, Electorate, CON, LAB, LIB, UKIP, Green, NAT, MIN, OTH
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(elections)  
```

```
## # A tibble: 6 × 14
##   Name      MP     Area County Elect…¹   CON   LAB   LIB   NAT   MIN   OTH year 
##   <chr>     <chr> <dbl> <chr>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <chr>
## 1 Abingdon  AMS …    12 Berks…   58487 25613 16979  8634     0     0     0 1955 
## 2 Accringt… H Hy…     4 Lanca…   50938 21157 22502     0     0     0     0 1955 
## 3 Acton     JA S…    11 Ealing   49373 20120 20645     0     0     0     0 1955 
## 4 Aldershot E Er…    12 Hamps…   54209 22701 13129  4232     0     0     0 1955 
## 5 Altrinch… FJ E…     4 Centr…   61525 30586 12174  6436     0     0     0 1955 
## 6 Arundel … HB K…    12 West …   69034 35180 15188     0     0     0     0 1955 
## # … with 2 more variables: UKIP <dbl>, Green <dbl>, and abbreviated variable
## #   name ¹​Electorate
```
  
## Create data by webscraping online tables  

Next we'll practice scraping data from tables found on a webpage.  

First, specify the link and save it as an object.  


```r
url <- c(
  "https://en.wikipedia.org/wiki/2014_European_Parliament_election_in_the_United_Kingdom"
)  
```
  
Then, using read_html, we'll save the data from the page.  


```r
wikipage <- read_html(url)  
```
  
Then take a look at the type of data we saved.  


```r
class(wikipage)  
```

```
## [1] "xml_document" "xml_node"
```
  
We can see that we saved an xml_document and xml_node. Next we'll want to save the tables in that data and inspect.  


```r
data_table <- html_nodes(wikipage,"table")  

data_table  
```

```
## {xml_nodeset (25)}
##  [1] <table class="infobox vevent" style="line-height: 1.5em; width:500px">\n ...
##  [2] <table style="width:100%; margin:1px; display:inline-table;"><tbody><tr> ...
##  [3] <table style="background:transparent; width:100%;"><tbody>\n<tr style="d ...
##  [4] <table cellspacing="0" cellpadding="0" style="background:transparent; wi ...
##  [5] <table class="sidebar sidebar-collapse nomobile vcard"><tbody>\n<tr><td  ...
##  [6] <table style="width:100%;border-collapse:collapse;border-spacing:0px 0px ...
##  [7] <table style="width:100%;border-collapse:collapse;border-spacing:0px 0px ...
##  [8] <table class="wikitable sortable">\n<caption>Representation by region\n< ...
##  [9] <table class="wikitable" style="font-size:90%"><tbody>\n<tr><th colspan= ...
## [10] <table class="wikitable sortable" style="text-align:center; font-size:90 ...
## [11] <table class="wikitable sortable" style="text-align:center; font-size:90 ...
## [12] <table class="wikitable sortable" style="text-align:center; font-size:90 ...
## [13] <table class="wikitable sortable" style="text-align:center; font-size:90 ...
## [14] <table class="wikitable sortable" style="text-align:center; font-size:90 ...
## [15] <table class="wikitable sortable" style="text-align:right style=">\n<cap ...
## [16] <table class="wikitable"><tbody>\n<tr>\n<th>Constituency\n</th>\n<th col ...
## [17] <table class="nowraplinks mw-collapsible autocollapse navbox-inner" styl ...
## [18] <table class="nowraplinks hlist mw-collapsible autocollapse navbox-inner ...
## [19] <table class="nowraplinks hlist mw-collapsible autocollapse navbox-inner ...
## [20] <table class="nowraplinks hlist mw-collapsible mw-collapsed navbox-inner ...
## ...
```
  
Let's pick out a specific table. From the wiki page, we can pick out the data on the number of votes parties received and see that it is table 15 from our saved data.  

We'll use html_table() to get the tables and pipe in pluck() to get the specific table we're looking for. Because some of the cells in the table are empty, we'll use fill=TRUE in html_table(), saving the object as ep14_raw to indicate it is the raw, unchanged data. Then we'll use class() to confirm we've saved the data as a data_frame.  


```r
ep14_raw <- data_table %>%
  html_table(fill=TRUE) %>%
  purrr::pluck(15)  

class(ep14_raw)  
```

```
## [1] "tbl_df"     "tbl"        "data.frame"
```
  
Now we can begin to clean up our data.  

We'll start by looking at the last of the data to see any issues there.  


```r
tail(ep14_raw)
```

```
## # A tibble: 6 × 10
##   Party             Party        Votes Votes Votes Seats Seats Seats ``    ``   
##   <chr>             <chr>        <chr> <chr> <chr> <chr> <chr> <chr> <lgl> <lgl>
## 1 ""                NI21         10,5… 0.1   "New" "0"   ""    ""    NA    NA   
## 2 ""                Peace Party  10,1… 0.1   ""    "0"   ""    ""    NA    NA   
## 3 ""                Others       55,0… 0.3   "3.4" "0"   ""    ""    NA    NA   
## 4 "Valid Votes"     Valid Votes  16,4… 99.5  ""    "73"  "1"   ""    NA    NA   
## 5 "Rejected Votes"  Rejected Vo… 90,8… 0.6   ""    ""    ""    ""    NA    NA   
## 6 "Overall turnout" Overall tur… 16,5… 35.6  "0.9" ""    ""    ""    NA    NA
```

We see that the last three rows are aggregated data. Let's remove those.  


```r
ep14_raw <- ep14_raw[-c(32:34), ]  
```
  
Now we'll move to the front end of our data for cleaning.  


```r
head(ep14_raw)  
```

```
## # A tibble: 6 × 10
##   Party   Party                  Votes Votes Votes Seats Seats Seats ``    ``   
##   <chr>   <chr>                  <chr> <chr> <chr> <chr> <chr> <chr> <lgl> <lgl>
## 1 "Party" Party                  Numb… %     +/-   Seats +/-   %     NA    NA   
## 2 ""      UK Independence Party  4,37… 26.6  10.6  24    11    32.9  NA    NA   
## 3 ""      Labour Party           4,02… 24.4  9.2   20    7     27.4  NA    NA   
## 4 ""      Conservative Party     3,79… 23.1  3.8   19    7     26.0  NA    NA   
## 5 ""      Green Party of Englan… 1,13… 6.9   0.9   3     1     4.1   NA    NA   
## 6 ""      Liberal Democrats      1,08… 6.6   6.7   1     10    1.4   NA    NA
```

We can see that the variable names are not unique. We'll give specific names to variables we are interested in, and innocuous names to the ones we aren't.  


```r
names(ep14_raw) <- c("V1", "party", "votes", "share", "V5",
                     "seats", "V7", "V8", "V9", "V10")  
```
  
Then we'll remove the first row which is just a duplicate of the column headers.  


```r
ep14_raw <- ep14_raw[-c(1), ]  
```
  
Next we'll remove the columns that we don't need by selecting only the columns we want.  


```r
ep14_raw <- ep14_raw %>%
  select(party, votes, share, seats)  
```
  
Lastly, we'll identify the numeric variables as such and rename our cleaned table.  


```r
ep14 <- ep14_raw %>%
  mutate(
    votes = parse_number(votes),
    share = as.numeric(share),
    seats = as.numeric(seats)
  )  
```
  
Now that we've created our data let's make a simple dot plot labeling all parties that have a vote share greater than 15.  


```r
ggplot(ep14, aes(x = share, y = seats)) +
  geom_point() +
  theme_minimal() +
  ggrepel::geom_text_repel(
    aes(label = ifelse(share > 15, party, NA)),
    size = 4.5,
    point.padding = .2,
    box.padding = .4
  ) +
  labs(
    y = "Number of seats",
    x = "Vote share",
    title = "2014 European Parliament election, United Kingdom"
  )  
```

```
## Warning: Removed 27 rows containing missing values (`geom_text_repel()`).
```

<img src="2023-01-25-Quantitative-Politics-with-R_files/figure-html/Create dotplot-1.svg" width="100%" />




