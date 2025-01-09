---
layout: post
title: "Most Subscribed Youtube Channels Mini Exploritory Data Analysys"
date: 09/01/2025
categories: python, data analysis
# image: /assests/atricle_images/[post_folder]/[image] -this is the desktop image
# image2: /assests/atricle_images/[post_folder]/[image] -this is the mobile image
---
In this post, we’ll explore some data on the most subscribed Youtube
channels as of Novemeber 2022. It’s set up in a simple step-by-step
assignment structure.

Start by importing the csv module. Load and read the Most Subscribed Youtube
Channels data set from the topSubscribed.csv file into a DictReader
inside a "with" statement. Print the field names of the data set. Iterate
over the reader to put the data into a list name “channels”.

``` python
import csv
filepath = "topSubscribed.csv"
channels = [] 

with open(filepath) as csvfile:
    reader = csv.DictReader(csvfile)
    
    print(reader.fieldnames) # print field names
    for row in reader:
        channels.append(row)
```

    ## ['Rank', 'Youtube Channel', 'Subscribers', 'Video Views', 'Video Count', 'Category', 'Started']  


How many channels are there in total?

``` python
channels_count = 0

for row in channels:
    channels_count +=1 # increase count by 1 for each row

print(channels_count)
```

    ## 1000  


How many channels are in the each category?

``` python
#Create a new list containing values from the "Category" column in channels.
channel_categories = [row['Category'] for row in channels]


#Create a new dictionary with values of that column as keys and the counts as values.
count = {}
for category in channel_categories:
    count[category] = count.get(category, 0) + 1

#Get a list of the dictionary keys and values (use the items() method) and sort them in reverse order, from greatest (most channels) to least.
#Get and print the top 3.
count_sorted = sorted(count.items(), key = lambda x: x[1], reverse = True)

print(count_sorted)
```

    ## [('Entertainment', 238), ('Music', 217), ('People & Blogs', 132), ('Gaming', 94), ('Comedy', 68), ('Film & Animation', 50), ('Education', 45), ('Howto & Style', 43), ('https://us.youtubers.me/global/all/top-1000-most_subscribed-youtube-channels', 30), ('News & Politics', 27), ('Science & Technology', 18), ('Shows', 14), ('Sports', 10), ('Pets & Animals', 6), ('Trailers', 2), ('Nonprofits & Activism', 2), ('Movies', 2), ('Autos & Vehicles', 1), ('Travel & Events', 1)]  


What Howto & Style Channel has the most views? Print the name.

``` python
def is_valid_view_count(count_as_string):
    '''
    Converts input to an integer. If it cannot be converted to int, returns False.

    '''
    
    try:
        view_count = int(count_as_string) # try casting as int
        
    except ValueError:
        
        return False # return false if ValueError is generated
    
    else:
        
        return view_count # return view_count as int

is_howto_style = [row for row in channels if row['Category'] == 'Howto & Style']

howto_style_max = max(is_howto_style, key = lambda x: (is_valid_view_count(x['Video Views'])))

name = howto_style_max['Youtube Channel']

print(name)
```

    ## 5-Minute Crafts  


Let’s consider and "early adopter" any channel started within 5 years of Youtube’s launch,
started before 2011. Write code to filter out any channels created after 2010
and find the channel with the least number of videos. Print the name of
the channel and the number of videos.

``` python
def is_valid_number(count_as_string):
    '''
    Converts input to an integer. If it cannot be converted to int, returns False.

    '''

    try:
        valid_number = int(count_as_string) # try casting as int

    except ValueError:

        return False # return false if ValueError is generated

    else:

        return valid_number # return video_count as int

early_adopters = [row for row in channels if is_valid_number(row['Started']) < 2011]

min_ea_vid_count = min(early_adopters, key = lambda x: is_valid_number(x['Video Count']))

print('The early adopter channel with the least number of videos is', min_ea_vid_count['Youtube Channel'], 'It has', min_ea_vid_count['Video Count'], 'videos.')
```

    ## The early adopter channel with the least number of videos is T-Series It has 18,515 videos.  


What are the top 3 Categories, and how many channels are there for each?
Get the top 3 and store in “top3categories”. Print the result.

``` python
#Create a new list containing values from the "Category" column in channels.
channel_categories = [row['Category'] for row in channels]


#Create a new dictionary with values of that column as keys and the counts as values.
count = {}
for category in channel_categories:
    count[category] = count.get(category, 0) + 1

#Get a list of the dictionary keys and values (use the items() method) and sort them in reverse order, from greatest (most channels) to least.
#Get and print the top 3.
count_sorted = sorted(count.items(), key = lambda x: x[1], reverse = True)

top3categories = count_sorted[:3]

print(top3categories)
```

    ## [('Entertainment', 238), ('Music', 217), ('People & Blogs', 132)]  
