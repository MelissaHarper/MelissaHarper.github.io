---
title: "The Relationship Between Child Poverty and College and Career Readiness in Missouri School Districts"
layout: post
date: 2022-12-11
output:
  md_document:
    variant: markdown_github
    preserve_yaml: true
# output:
#   html_document:
#     #keep_md: TRUE
#     self_contained: yes
#     mode: selfcontained
always_allow_html: TRUE
---

### Purpose

It’s not a stretch to imagine that child poverty within a school
district can adversely affect student performance. As a former teacher,
I can anecdotally say a child dealing with poverty at home is less
likely to succeed within the standard expectations of a classroom.

This project is an initial exploratory data analysis of the relationship
between child poverty and College and Career Readiness in Missouri
School Districts for the 2019 school year. It stands as an introduction
to further analysis

### Guiding Question

What is the relationship between a school district’s **College and
Career Readiness** (CCR) scores<sup>i</sup> and the ratio of children
living in poverty<sup>ii</sup> within that school district?

### Data Sources

I’ve gathered performance data from the Missouri Department of
Elementary and Secondary Education
[(DESE)](https://apps.dese.mo.gov/MCDS/home.aspx)<sup>i</sup>, and
census.gov’s Small Area Income and Poverty Estimates
[(SAIPE)](https://www.census.gov/programs-surveys/saipe/data/datasets.2019.List_1743592724.html#list-tab-List_1743592724)<sup>ii</sup>
from 2019 to explore an introductory look at that relationship.

### The Short Story

In each of the three CCR categories, we can see a decline in CCR as
child poverty increases. With the most drastic decline presenting within
assessment scores. <br>

<img src="/C:/Users/Guita/Documents/Coding/GitHub/melissaharper.github.io/assets/article_images/2022-12-11-ccr_poverty_report/Plot-3-Criteria-and-Poverty-Ratio-1.svg" width="100%" style="display: block; margin: auto;" />

<br>

### Notes on the Data

<table class=" lightable-classic" style="font-size: 16px; font-family: Cambria; width: auto !important; float: left; margin-right: 10px;">
<caption style="font-size: initial !important;">
2019 Poverty Guidelines for Missouri
</caption>
<thead>
<tr>
<th style="text-align:center;">
Household Size
</th>
<th style="text-align:center;">
Poverty Guidelines
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
$12,490
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
$16,910
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
$21,330
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
$25.750
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
$30,170
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
$34,590
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
$39,010
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
$43,430
</td>
</tr>
</tbody>
</table>

<br> **DESE’s College and Career Readiness (CCR)** scores are based on
the percentage of graduating students that meet specific criteria within
three categories:

• College Level Proficiency  
• College or Career Path  
• Assessment Scores

Within those categories, the districts’ percentage scores are divided
into 4 determinations: “Target”, “Approaching”, “On Track”, “Floor.” For
this project, “Target” indicates the district has met the standard for
that category. “Approaching” indicates their score is just below meeting
the state standard. “On Track” falls below that, with “Floor” being the
lowest determination.<sup>iii</sup>

*Note, the percentage range for each determination is different between
categories. The determination percentage range for the indicated
category can be found within the legends where applicable.* <br> \##
Analysis

### Proficiency in College Level Courses

**Definition:** Graduates, “who earned a qualifying score on an AP, IB,
or IRC assessments and/or receive college credit through early college,
dual enrollment, or approved dual credit courses meets or exceeds the
state standard or demonstrates required improvement” while still in high
school (MSIP5)

Within this category 47% of graduating students must show proficiency as
defined above in order to meet the Target. [The United States Government
Accountability Office found
that](https://www.gao.gov/assets/gao-19-8.pdf) students attending high
school in a district with higher poverty rates had less access to
academic offerings that would provide college level proficiency.

In Missouri districts, we see a large disparity between readiness
scores. The lowest performing district shows only 3.7% of graduates
meeting the state standard though its child poverty ratio falls within
the lowest 50% with a ratio of 15.2. The highest performing district
earned a score of 103.1% also boasts the lowest poverty ratio of 2.5. At
the extremes, there is a performance difference of 99.4% and a
difference in poverty ratio of 12.7.

<img src="/C:/Users/Guita/Documents/Coding/GitHub/melissaharper.github.io/assets/article_images/2022-12-11-ccr_poverty_report/College-Level-Proficiency-Scores-1.svg" width="100%" style="display: block; margin: auto;" />

## College or Career Path

**Definition:** Graduates who are in college, the military, or on a
career path directly related to their career education program within 6
months of graduating.

In contrast, we see the least disparity within the College or Career
Path category with a high of 100% and a low of 58.9% of graduates
meeting the standard, for a variation of only 41.1%. This is still a
significant difference, but when compared to the 99.4% difference within
the Proficiency in College Level Courses category, it seems quite tame.

<img src="/C:/Users/Guita/Documents/Coding/GitHub/melissaharper.github.io/assets/article_images/2022-12-11-ccr_poverty_report/Districts'-College-or-Career-Path-Scores-1.svg" width="100%" style="display: block; margin: auto;" />

Indeed, when we look at the school districts within the highest 25% of
child poverty, we see more districts meeting or approaching the state
standard here than in any other category.

<img src="/C:/Users/Guita/Documents/Coding/GitHub/melissaharper.github.io/assets/article_images/2022-12-11-ccr_poverty_report/Child-Poverty-Third-Quartile-1.svg" width="100%" />

While college level proficiency shows two more districts meeting the
Target standard than college or career placement, college or career
placement shows vastly more districts approaching the target standard.
This makes the likelihood of more districts meeting the target standard
in the future much higher. Conversely, assessment scores leave
*significant* room for improvement.

## Assessment Scores

**Definition:** Graduates who scored at or above the state standard on
any department-approved measure of college and career readiness test.

<img src="/C:/Users/Guita/Documents/Coding/GitHub/melissaharper.github.io/assets/article_images/2022-12-11-ccr_poverty_report/Districts'-Assessment-Scores-1.svg" width="100%" style="display: block; margin: auto;" />

In the assessment category, we see the least number of districts meeting
the state standard with only 189 hitting the target. Compare this to 285
and 310 districts meeting the state standard in College or Career
Placement and College Level Proficiency respectively.

<img src="/C:/Users/Guita/Documents/Coding/GitHub/melissaharper.github.io/assets/article_images/2022-12-11-ccr_poverty_report/Determination-Counts-1.svg" width="100%" style="display: block; margin: auto;" />

## Recomendation

While many times correlation does not equal causation, further steps
should be taken to investigate a causation link, including analyzing
data from multiple years to further identify trends. If causation is
found, identify what changes should be made within the current system to
mitigate the effects.

## Further Exploration

There are many other questions that arise when looking at this data as a
starting point.

-   Does enrollment size affect performance when determining the affect
    of poverty?
-   What is the student to teacher ratio within each district?
    -   Is there a correlation between teacher base pay and high student
        to teacher ratios?
-   Is there a correlation between high performing teachers and
    districts with a lower poverty rate?
-   How do demographics compare between high scoring and low scoring
    districts?
-   Does our current system perpetuate under performance?

## Process

By far the most difficult part of this project was sifting through
DESE’s documentation to find complete and appropriate data. The only
data that met that criteria was for the year 2019. Therefore, this
project explored this relationship for only a single year: 2019.

After finding and compiling the data, I made a copy of each raw data
file, normalized the district names across all sources, filtered each
database to contain only information pertinent to this project while
keeping data that might be useful for further analysis. I then verified
that the resulting cleaned data was accurate to its source material and,
using PostgreSQL, created a singular combined data table.

Of the original 553 reported districts from DESE, 95 districts do not
have a year 12 (students graduating high school). Twelve more are
charter or special school districts that do not have corresponding child
poverty data. Filtering out these districts leaves us with 446
districts.

More detailed process information can be found in the word document
found at [this
link](https://github.com/MelissaHarper/mo_ccr_and_child_poverty/blob/3bc413b0b7768dbeb71d92262bc1b61e8b27fd2f/Missouri%20Education%20Exploration%20Documentation.docx)

------------------------------------------------------------------------

### Endnotes

<sup>i</sup>[College and Career
Readiness](https://dese.mo.gov/sites/dese/themes/dese_2020/mo-viewer/viewer.html?file=https%3A%2F%2Fdese.mo.gov%2Fsites%2Fdese%2Ffiles%2Fmedia%2Fpdf%2F2020%2F09%2Fqs-MSIP5-2019-Comprehensive-Guide-11-15-2019.pdf#page=26&zoom=auto,-13,738)
—“The district provides adequate post-secondary preparation for all
students.  
1. The percent of graduates who scored at or above the state standard on
any department-approved measure(s) of college and career readiness, for
example, the ACT®, SAT®, COMPASS® or Armed Services Vocational Aptitude
Battery (ASVAB), meets or exceeds the state standard or demonstrates
required improvement.  
2. The district’s average composite score(s) on any department-approved
measure(s) of college and career readiness, for example, the ACT®, SAT®,
COMPASS®, or ASVAB, meet(s) or exceed(s) the state standard or
demonstrate(s) required improvement.  
3. The percent of graduates who participated in any department-approved
measure(s) of college and career readiness, for example, the ACT®, SAT®,
COMPASS®, or ASVAB, meets or exceeds the state standard or demonstrates
required improvement.  
4. The percent of graduates who earned a qualifying score or grade on an
Advanced Placement (AP), International Baccalaureate (IB), or Technical
Skills Attainment (TSA) assessments and/or receive college credit or a
qualifying grade through early college, dual enrollment, or approved
dual credit courses meets or exceeds the state standard or demonstrates
required improvement.  
5. The percent of graduates who attend post-secondary education/training
or are in the military within six months of graduating meets the state
standard or demonstrates required improvement.  
6. The percent of graduates who complete career education programs
approved by DESE and are placed in occupations directly related to their
training, continue their education, or are in the military within six
months of graduating meets the state standard or demonstrates required
improvement.”
[DESE](https://dese.mo.gov/sites/dese/themes/dese_2020/mo-viewer/viewer.html?file=https%3A%2F%2Fdese.mo.gov%2Fsites%2Fdese%2Ffiles%2Fmedia%2Fpdf%2F2020%2F09%2Fqs-MSIP5-2019-Comprehensive-Guide-11-15-2019.pdf#page=26&zoom=auto,-13,738)

<sup>ii</sup> “In addition, in order to implement provisions under Title
I of the Elementary and Secondary Education Act as amended, we produce
the following estimates for school districts: • total population •
number of children ages 5 to 17 • number of related children ages 5 to
17 in families in poverty The estimates are not direct counts from
enumerations or administrative records, nor direct estimates from sample
surveys. Instead, for counties and states, we model income and poverty
estimates by combining survey data with population estimates and
administrative records. For school districts, we use the model-based
county estimates and inputs from federal tax information and multi-year
survey data to produce estimates of poverty.”
[(census.gov)](https://www.census.gov/programs-surveys/saipe/about.html#:~:text=In%20addition%2C%20in,estimates%20of%20poverty.)

<sup>iii</sup> DESE ranks these in the order of Target, On Track,
Approaching, Floor. This wording often causes confusion about the
rankings of the categories with “Approaching” inferring a closeness to
the target. I have chosen to swap the order of the terms to eliminate
that confusion.
