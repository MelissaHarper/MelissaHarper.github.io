----
layout: post
title: "Call Center Excel Dashboard
date: 23-01-05
categories: dashboards
image: /article_images/2023-01-05-excel_dashboard/Call Center Dashboard.JPG
----

<center><h1> Creating a dashboard using Excel </h1></center>

![Screenshot of a dashboard created in excel. The dashboard, titled Call Center Performance Dashboard Operations Assistance Inc. October Report, contains five charts visualizing data for call counts by date, calls by state, reason for call, and cell channel. At the bottom are interactive filters allowing the user to break down the data by state, reason for calling, channel, and response time. To the left are three text boxes containg the total number of calls, the average csat score, and the average call duration.](/assets/article_images/2023-01-05-excel_dashboard/Call Center Dashboard.JPG)


Above is a screenshot of an interactive dashboard I created in excel. It features dynamic charts that update when the workbook is opened and filters that allow the user to interact with the charts by filtering segmented data.

"Melissa, why not just use one of the myriad of visualization tools that allow for ultimate customization?" Great question, internet stranger. The average user is often intimidated by specialized tools and, understandably, prefer programs with which they are familiar. This lead me to explore ways I could create a dashboard within excel that still provided the dynamic and interactive features of specialized visualization tools.  

#### Dynamic

Initially I planned to use a worksheet change event within the VBA code that would cause the pivot tables to refresh when any data was added to the source sheet. However, this would also clear the change history within the sheet as well. As an *avid* undo user, this was not an option. Instead, I compromised by setting the pivot table options to refresh whenever the workbook was opened. While not *ideal* it does ensure users are seeing the most up to date information when opening the file and safeguards users from unknowingly engaging with outdated charts in the event changes have been made by other users. 

![Picture of pivot table options menu in excel. Options, Data, and Refrest data when opening the file are highlighted](/assets/article_images/2023-01-05-excel_dashboard/refresh_data.JPG)

#### Interactive

Because I created each chart from a pivot table, I was able to link all of the slicers to every chart on the dashboard using the slicer "Report Connections" feature. This allows the user to filter all the charts on the dashboard by an individual, or a combination of values. In addition to getting an overall view of the data, this segmentation data creates very specific charts as quickly as you can select how you want the data broken down.

![Picture of pivot table options menu in excel. Options, Data, and Refrest data when opening the file are highlighted](/assets/article_images/2023-01-05-excel_dashboard/slicer_report_connections.JPG)


<details>

  <summary>Click here to see the process of creating the dashboard</summary>


## Process

Below is documentation on the steps I took to create the dashboard seen in the screenshot above. These steps assume a working knowledge of Excel. 

### Download and open dataset

This exercise uses the call center data from the [Real World Fake Dataset](https://www.kaggle.com/datasets/mesumraza/real-world-fake-dataset-for-practice) that can be found on kaggle.

### Copy dataset to preserve original and then clean the data 

- copy file
- check data types
- extract the day from the call_timestamp column to use in a line chart

### Create pivot tables and charts

- Create separate sheets for each chart
	- Line Chart for the call trends.
	- A doughnut chart for the Call channel.
	- A map for the states.
	- A column chart for the reason of calling.
	- A bar chart for the response time.
	- Key Performance Indicators (KPIs)
	
#### Call Trends Line Chart
	
	- Insert a pivot table on the "call_trends" sheet
		- Select all the call center data for the pivot chart
		- Add "call_day" to rows
		- Add "id" to values
	- Insert a line chart with markers using the data in this table
	- For each chart we insert we will edit the layout and design of this chart when we design our dashboard
	
#### Call Channel Doughnut Chart
	
	- Create a pivot table as above adding "channel" to rows instead of "call_day"
	- Insert a doughnut chart

#### Map
	
	- Create pivot chart as above using "state" for rows
	- If we insert a map at this step, we will get an error. Instead:
		- Copy all data from the pivot table
		- Paste "values" in a blank cell
		- Select the pasted data
		- Insert Map from the charts options
		- Select "Filled Map"
		- With the chart selected, under the "Chart Design" tab, click "Select Data"
		- In the "Select Data Source" dialog box, click the up arrow next to "Chart Data Range"
		- Select the Pivot Table data
		- Now that the map is connected to the pivot table, delete the copied data
		- *Workaround provided by [Simon Sez IT](https://www.simonsezit.com/article/excel-map-chart/#:~:text=If%20we%20try%20to%20create%20a%20map%20chart%20directly%20from%20our%20Pivot%20Table%2C%20we%20will%20receive%20a%20message%20letting%20us%20know%20we%20cannot%20create%20this%20chart%20type%20using%20data%20inside%20a%20Pivot%20Table.%C2%A0)*
	
#### Reason for Calling Column Chart

	- Create pivot chart as above using "reason" for rows
	- Insert a column chart
	
#### Repsonse Time Bar Chart

	- Create a pivot table as above using "response_time" for rows
	- Insert a bar chart
	
#### Key Performance Indicators (KPI's)
  
  - Create a pivot table adding "customer_name" to values
  - Create a second pivot table adding "csat_score" to values
    - Click on the calculation just added to values and select "Value Field Settings"
    - Under "Summarize Values By" select "Average"
  - Create a third pivot chart selecting "call duration in minutes" to values and set to average as above
  - In a cell under each pivot chart type "=" and then select the pivot chart above. Label these "Get Pivot Data" (We will use this later)
	
### Build Dashboard

	- Create another sheet named "Dashboard"
		- Remove the grid lines
		- Under "Page Layout", insert a background picture
			- Choose a picture that is monochromatic without a lot of geometry that may obscure the actual dashboard
		- Insert a text box at top center for your header, naming the dashboard "Performance Dashboard"
			- Remove fill and line from text box
		- Insert a line below the header
		- Insert a subheading below naming your fictional call center
		- Format header and sub header to be visually pleasing
	- Insert a rectangle and as the background for the line chart
		- Under "Format Shape", set transparency of rectangles to ~60% and select "No line" under "Line Heading"
		- Copy and paste the rectangle, one for each chart with three more for the KPIs
		- Arrange and resize rectangles in a visually appealing layout for the dashboard
	- Insert and size a text box, creating a heading for one chart
		- Copy and paste the text box into each rectangle created in the last step, naming each chart and KPI
			- Call trends
			- Call channel
			- Calls by state
			- Call Reasons
			- Response time
			- Number of Calls
			- Average Satisfaction Score
			- Average Call Duration
	- Insert icons for each chart indicating the data found in the chart
	- Copy and paste each chart from their original sheets to their respective rectangle on the dashboard
		- Format charts to, again, be visually appealing making sure the data is easy to read
			- Remove backgrounds and extraneous information, resize, uniform fonts and colors
	- Create a text box for each KPI 
	  - In the formula bar for each text box type "=" then click on the cell created under "Get Pivot Data" for each respective KPI
	  - Format the text
	- Create filters (slicers) for the data, making the dashboard interactive
		- Select the line chart
		- Click "Insert" -> "Slicer"
		- Select data to filter by
			- call_trends
			- call channel
			- csat_score
			- reason
			- response_time
		- Right click on a slicer, and select "Report Connections"
		- Select each pivot table used in the dashboard to connect the slicers to each chart
		- Repeat for each slicer
		- Format slicers
			- Slicers can be customized by select "more" -> "new slicer style" under the Slicer Ribbon option
		- Resize and layout slicers in dashboard  

For a more detailed description, you can follow the video below that uses a different data set. Or, follow the steps found in [this article](https://medium.com/@Armonia1999/data-analysis-project-excel-dashboard-10c6160f2dbe) that uses the data set as the above dashboard.

{% include excel_dashboard_youtube.html id="20zDV9MNE0s"%}

</details>

	

	
	
	
	
