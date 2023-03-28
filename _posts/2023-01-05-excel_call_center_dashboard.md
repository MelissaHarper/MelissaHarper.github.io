---
layout: post
title: "Excel Call Center Dashboard"
date: 23-01-05
categories: dashboards
image: /article_images/2023-01-05-excel_dashboard/Call Center Dashboard.JPG
---

<center><h1> Creating an interactive dashboard using Excel </h1></center>  

![Interactive dashboard built in Excel](/assets/article_images/2023-01-05-excel_dashboard/Call Center Dashboard.JPG)

Above is a screenshot of an interactive dashboard I created in excel. It features dynamic charts that update when the workbook is opened and filters that allow the user to interact with the charts by filtering segmented data.

"Melissa, why not just use one of the myriad of visualization tools that allow for ultimate customization?" Great question, internet stranger. The average user is often intimidated by specialized tools and, understandably, prefer programs with which they are familiar. This lead me to explore ways I could create a dashboard within excel that still provided the dynamic and interactive features of specialized visualization tools.  

### Dynamic

Initially I planned to use a worksheet change event within the VBA code that would cause the pivot tables to refresh when any data was added to the source sheet. However, this would also clear the change history within the sheet as well. As an *avid* undo user, this was not an option. Instead, I compromised by setting the pivot table options to refresh whenever the workbook was opened. While not *ideal* it does ensure users are seeing the most up to date information when opening the file and safeguards users from unknowingly engaging with outdated charts in the event changes have been made by other users.  

![Pivot tables set to automatically refresh when file is opened](/assets/article_images/2023-01-05-excel_dashboard/refresh_data.JPG)

### Interactive

Because I created each chart from a pivot table, I was able to link all of the slicers to every chart on the dashboard using the slicer "Report Connections" feature. This allows the user to filter all the charts on the dashboard by an individual, or a combination of values. In addition to getting an overall view of the data, this segmentation data creates very specific charts as quickly as you can select how you want the data broken down.

![Connect all Pivot Tables to slicers](/assets/article_images/2023-01-05-excel_dashboard/slicer_report_connections.JPG)

<details>

  <summary>Click here to see the process of creating the dashboard</summary>
<h3>Process</h3>

<p>Below is documentation on the steps I took to create the dashboard seen in the screenshot above. These steps assume a working knowledge of Excel. </p>
<h3>Download and open dataset</h3>

<p>This exercise uses the call center data from the <a href="https://www.kaggle.com/datasets/mesumraza/real-world-fake-dataset-for-practice">Real World Fake Dataset</a> that can be found on kaggle.</p>
<h3>Copy dataset to preserve original and then clean the data</h3>  

<ul>
<li>copy file</li>
<li>check data types</li>
<li>extract the day from the call_timestamp column to use in a line chart</li>
</ul>
<h3>Create pivot tables and charts</h3>

<ul>
<li>Create separate sheets for each chart<ul>
<li>Line Chart for the call trends.</li>
<li>A doughnut chart for the Call channel.</li>
<li>A map for the states.</li>
<li>A column chart for the reason of calling.</li>
<li>A bar chart for the response time.</li>
<li>Key Performance Indicators (KPIs)</li>
</ul>
</li>
</ul>
<h4>Call Trends Line Chart</h4>

<ul>
<li>Insert a pivot table on the &quot;call_trends&quot; sheet</li>
<li>Select all the call center data for the pivot chart</li>
<li>Add &quot;call_day&quot; to rows</li>
<li>Add &quot;id&quot; to values</li>
<li>Insert a line chart with markers using the data in this table</li>
<li>For each chart we insert we will edit the layout and design of this chart when we design our dashboard</li>
</ul>
<h4>Call Channel Doughnut Chart</h4>

<ul>
<li>Create a pivot table as above adding &quot;channel&quot; to rows instead of &quot;call_day&quot;</li>
<li>Insert a doughnut chart</li>
</ul>
<h4>Map</h4>

<ul>
<li>Create pivot chart as above using &quot;state&quot; for rows</li>
<li>If we insert a map at this step, we will get an error. Instead:</li>
<li>Copy all data from the pivot table</li>
<li>Paste &quot;values&quot; in a blank cell</li>
<li>Select the pasted data</li>
<li>Insert Map from the charts options</li>
<li>Select &quot;Filled Map&quot;</li>
<li>With the chart selected, under the &quot;Chart Design&quot; tab, click &quot;Select Data&quot;</li>
<li>In the &quot;Select Data Source&quot; dialog box, click the up arrow next to &quot;Chart Data Range&quot;</li>
<li>Select the Pivot Table data</li>
<li>Now that the map is connected to the pivot table, delete the copied data</li>
<li><em>Workaround provided by <a href="https://www.simonsezit.com/article/excel-map-chart/#:~:text=If%20we%20try%20to%20create%20a%20map%20chart%20directly%20from%20our%20Pivot%20Table%2C%20we%20will%20receive%20a%20message%20letting%20us%20know%20we%20cannot%20create%20this%20chart%20type%20using%20data%20inside%20a%20Pivot%20Table.%C2%A0">Simon Sez IT</a></em></li>
</ul>
<h4>Reason for Calling Column Chart</h4>

<ul>
<li>Create pivot chart as above using &quot;reason&quot; for rows</li>
<li>Insert a column chart</li>
</ul>
<h4>Repsonse Time Bar Chart</h4>

<ul>
<li>Create a pivot table as above using &quot;response_time&quot; for rows</li>
<li>Insert a bar chart</li>
</ul>
<h4>Key Performance Indicators (KPI&#39;s)</h4>

<ul>
<li>Create a pivot table adding &quot;customer_name&quot; to values</li>
<li>Create a second pivot table adding &quot;csat_score&quot; to values<ul>
<li>Click on the calculation just added to values and select &quot;Value Field Settings&quot;</li>
<li>Under &quot;Summarize Values By&quot; select &quot;Average&quot;</li>
</ul>
</li>
<li>Create a third pivot chart selecting &quot;call duration in minutes&quot; to values and set to average as above</li>
<li>In a cell under each pivot chart type &quot;=&quot; and then select the pivot chart above. Label these &quot;Get Pivot Data&quot; (We will use this later)</li>
</ul>
<h3>Build Dashboard</h3>

<ul>
<li>Create another sheet named &quot;Dashboard&quot;</li>
<li>Remove the grid lines</li>
<li>Under &quot;Page Layout&quot;, insert a background picture<ul>
<li>Choose a picture that is monochromatic without a lot of geometry that may obscure the actual dashboard</li>
</ul>
</li>
<li>Insert a text box at top center for your header, naming the dashboard &quot;Performance Dashboard&quot;<ul>
<li>Remove fill and line from text box</li>
</ul>
</li>
<li>Insert a line below the header</li>
<li>Insert a subheading below naming your fictional call center</li>
<li>Format header and sub header to be visually pleasing</li>
<li>Insert a rectangle and as the background for the line chart</li>
<li>Under &quot;Format Shape&quot;, set transparency of rectangles to ~60% and select &quot;No line&quot; under &quot;Line Heading&quot;</li>
<li>Copy and paste the rectangle, one for each chart with three more for the KPIs</li>
<li>Arrange and resize rectangles in a visually appealing layout for the dashboard</li>
<li>Insert and size a text box, creating a heading for one chart</li>
<li>Copy and paste the text box into each rectangle created in the last step, naming each chart and KPI<ul>
<li>Call trends</li>
<li>Call channel</li>
<li>Calls by state</li>
<li>Call Reasons</li>
<li>Response time</li>
<li>Number of Calls</li>
<li>Average Satisfaction Score</li>
<li>Average Call Duration</li>
</ul>
</li>
<li>Insert icons for each chart indicating the data found in the chart</li>
<li>Copy and paste each chart from their original sheets to their respective rectangle on the dashboard</li>
<li>Format charts to, again, be visually appealing making sure the data is easy to read<ul>
<li>Remove backgrounds and extraneous information, resize, uniform fonts and colors</li>
</ul>
</li>
<li>Create a text box for each KPI <ul>
<li>In the formula bar for each text box type &quot;=&quot; then click on the cell created under &quot;Get Pivot Data&quot; for each respective KPI</li>
<li>Format the text</li>
</ul>
</li>
<li>Create filters (slicers) for the data, making the dashboard interactive</li>
<li>Select the line chart</li>
<li>Click &quot;Insert&quot; -&gt; &quot;Slicer&quot;</li>
<li>Select data to filter by<ul>
<li>call_trends</li>
<li>call channel</li>
<li>csat_score</li>
<li>reason</li>
<li>response_time</li>
</ul>
</li>
<li>Right click on a slicer, and select &quot;Report Connections&quot;</li>
<li>Select each pivot table used in the dashboard to connect the slicers to each chart</li>
<li>Repeat for each slicer</li>
<li>Format slicers<ul>
<li>Slicers can be customized by select &quot;more&quot; -&gt; &quot;new slicer style&quot; under the Slicer Ribbon option</li>
</ul>
</li>
<li>Resize and layout slicers in dashboard  </li>
</ul>
<p>For a more detailed description, you can follow the video below that uses a different data set. Or, follow the steps found in <a href="https://medium.com/@Armonia1999/data-analysis-project-excel-dashboard-10c6160f2dbe">this article</a> that uses the data set as the above dashboard.</p>


{% include excel_dashboard_youtube.html id="20zDV9MNE0s"%}

</details>

	

	
	
	
	
