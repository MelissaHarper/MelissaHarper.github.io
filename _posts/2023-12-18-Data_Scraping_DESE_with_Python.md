---
layout: post
title: "Data Scraping DESE with Python"
date: 23-12-18
categories: data scraping
image:
---

In the past, I performed an extremely basic exploratory data analysis on the
relationship between College and Career Readiness and Child Poverty
within Missouri school districts. That project was limited in scope in
that it only accessed one year of data from the Missouri Department of
Elementary and Secondary Education (DESE). As a former Missouri high
school teacher that remains passionate about education policy, I’m certain I’ll
perpetually return exploring data on Missouri school districts. So, I
decided I would write a python code to download the publicly available
data from DESE.

There were a few things I knew I needed to account for in my code; the
website is dynamic, and there is a button and a slider that need to be
clicked to access all of the data. Additionally, I noticed what
is surely the bane of a data scientist’s existence, the naming
conventions for the individual files are not consistent.

With those things in mind, I start with importing the necessary
libraries.

``` python
import pandas as pd
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options
from bs4 import BeautifulSoup
import urllib.request
```

Right away we’ll address the dynamic aspect of the website by using
selenium to run a chrome driver. I also specify that it should run
headless so that we don’t see a chrome window open, even though it is
running through Chrome driver.

``` python
# Path to the Chrome WebDriver executable
chrome_driver_path = 'path/to/chromedriver'

# Create a new instance of the Chrome driver
chrome_options = Options()
chrome_options.add_argument("--headless")  # Run Chrome in headless mode (without a graphical interface)
service = Service(chrome_driver_path)
driver = webdriver.Chrome(service=service, options=chrome_options)
```

Then we start interacting with the website. First I’ll specify and load
the correct url.

``` python
# Load the webpage
url = 'https://apps.dese.mo.gov/MCDS/home.aspx'
driver.get(url)
```

In case the data we need right away takes some time to load, I tell the
code to wait 10 seconds to give it time to load.

``` python
# Wait for the page to load (adjust the time if needed)
driver.implicitly_wait(10)
```

Then I address the second specific consideration for the code: pushing
the button and clicking the slider. The website loads visuals that show
a summary of district performance from the most recent reports. So, we
need to click the button directing us to the “Reports and Resources”
information. Then, because we want all publicly available data, we need
to click the slider to “Show All Content.”

``` python
# Click on the first button
button1 = driver.find_element(By.ID, 'MainContent_btnLinks')
button1.click()

# Click on the second element
element2 = driver.find_element(By.CLASS_NAME, 'dese-slider')
element2.click()
```

Again, we’ll direct the code to wait for the data to load, if necessary.

``` python
# Wait for a short while for the page to update after clicking
driver.implicitly_wait(2)
```

Now we start with the fun part of retrieving and formatting the data we
want to download.

``` python
# Get the HTML content of the webpage after JavaScript has executed
html_content = driver.page_source

# Parse the HTML content using BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')
```

When inspecting the website, I saw that all of the downloadable files were
a specific class. To keep things simple and avoid potentially sifting
through information we don’t need, I decided to look specifically for
that class. I followed that with a print command to tell me the number
of download links found to confirm it was finding links and
that the number of links were within the expected range.

``` python
# Find all the download links on the webpage using a CSS selector and create list
download_links = []
for link in soup.select('a.file-link.link-LinkLocation'):
    href = link.get('href')
    if href:
        download_link = 'https://apps.dese.mo.gov' + href
        download_link = download_link.replace(' ', '%20')
        download_links.append(download_link)

# Print the number of download links found
print("Number of download links found:", len(download_links))
```

With the list of download links compiled, I turned to addressing the
issue of inconsistent file naming conventions. As I mentioned earlier,
the link descriptions were consistent so, while long, I decided to use
the link descriptions for the file names to make it easier to identify
different files that contain the same data for different years.

I also wrote a few lines to normalize the names with typical file naming
conventions as you’ll see commented below.

``` python
# Create list for file names
filenames = []
for description in soup.select('a.file-link.link-LinkLocation'):
    link_description = description.get_text(strip=True)
    if link_description:
        # Remove any leading or trailing whitespace
        filename = link_description.strip()
        # Replace spaces with underscores
        filename = filename.replace(' ', '_')
        # Make everything lowercase
        filename = filename.lower()
        # Remove any remaining invalid characters in the filename
        filename = ''.join(c for c in filename if c.isalnum() or c in '._-')
        filenames.append(filename)
```

This, however, created a problem of its own as renaming the files
stripped them of their extensions. If left without extensions, the files
would download, but they wouldn’t have any kind of indication as to what
type of file they are. Because I’m dealing with more than one file type,
I couldn’t just slap an extension on the end and call it a day.

I decided I would scrape the class type again, but this time I’d split
the name and keep just the extension. In hindsight, it would have been
more efficient to just iterate over the list of download links and strip
the extensions from that rather than accessing the site again, but this
is what my brain produced at the time.

``` python
# Create list of extentions
extensions = []
for link in soup.select('a.file-link.link-LinkLocation'):
    href = link.get('href')
    if href:
        extension = href.split('.')[-1]
        extensions.append(extension)
```

In another round of “are you sure that’s the way you want to do that?” I
decided I’d make a dictionary from the three lists and convert it to a
dataframe to iterate over for downloading the files.

``` python
link_dict = {'download_link' : download_links,
                'filename' : filenames,
                'extension' : extensions}

link_dict = pd.DataFrame(link_dict)
```

Finally, we’ve made it to the actual download portion of the project.
We’ll iterate over our newly created dataframe to download our files and
instruct the code to tell us when the downloading is complete.

``` python
# Iterate over dictionary to download files
for index, row in link_dict.iterrows():
    download_link = row['download_link']
    filename = row['filename'] + '.' + row['extension']
    urllib.request.urlretrieve(download_link, filename)

# Print the number of download links found
print("Download complete")
```

BUT WAIT, we can’t forget that we’re running Chrome even though we can’t
see it. For the last step, I’ll close out that browser.

``` python
# Close the browser
driver.quit()
```

And that’s it. I’ve got the files I need. In the future I’ll likely add
a step to the code that checks if a file has already been downloaded.
But right now, I’ve got a litte over 100 files in various formats that
need to be sorted, indexed, and placed in a relational database for
future exploration.

Have a great day!

![Don’t tell me what kind of a day to have](/assets/article_images/2023-12-18-Data_Scraping_DESE_with_python/have_a_great_day.jpg)
