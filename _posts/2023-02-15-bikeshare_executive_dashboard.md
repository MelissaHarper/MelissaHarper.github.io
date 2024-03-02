---
layout: post
title: "Bike Store Revenue- Tableau Executive Dashboard"
author: "Harper"
date: "2023-02-15"
categories: dashboards
tags:
image:
output:
  html_document:
    keep_md: TRUE
  github_document:
always_allow_html: TRUE

---



Our fictional bike store company has requested a dashboard for which they can easily look at revenue by several variables. Starting in PostgreSQL, I extracted the necessary data from 8 different tables within a relational database. Taking that data over to Tableau, you can see the interactive dashboard embedded below. 


<center>###########</center>

<br>
<div class='tableauPlaceholder' id='viz1705522535120' style='position: relative'>
<noscript>
<a href='#'>
<img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Bi&#47;BikeStores_17051003154170&#47;Dashboard1&#47;1_rss.png' style='border: none' />
</a></noscript><object class='tableauViz'  style='display:none;'>
<param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> 
<param name='embed_code_version' value='3' /> <param name='site_root' value='' />
<param name='name' value='BikeStores_17051003154170&#47;Dashboard1' />
<param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Bi&#47;BikeStores_17051003154170&#47;Dashboard1&#47;1.png' /> 
<param name='animate_transition' value='yes' />
<param name='display_static_image' value='yes' />
<param name='display_spinner' value='yes' />
<param name='display_overlay' value='yes' />
<param name='display_count' value='yes' />
<param name='language' value='en-US' />
</object>
</div>                



<script>

var divElement = document.getElementById('viz1705522535120');                    
var vizElement = divElement.getElementsByTagName('object')[0];                    
if ( divElement.offsetWidth > 800 ) { vizElement.style.width='800px';vizElement.style.height='1627px';} 
else if ( divElement.offsetWidth > 500 ) { vizElement.style.width='800px';vizElement.style.height='1627px';} 
else { vizElement.style.width='100%';vizElement.style.height='2777px';}                     
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);  

</script>

<br>

### Dashboard Design: Tableau Filters and Elementary School Math

<br>

As you can see, the entire dashboard can be filtered by year as well as by state. Separately, the Top Customers chart uses a parameter that shows only the Top 6 customers by overall revenue generated, all of which happened to be in NY. When checking for errors before rolling out the dashboard, I noticed that, when NY was filtered out of the dashboard, the Top Customers chart was suddenly empty. The same thing happened when filtering by year, this time the chart was still populated, but only by a few customers. It turns out, the breakdown was happening because of the parameter. Tableau, like any code, has an established "order of operations", if you will. It turns out, Tableau processes parameters before filters. So unless one of the Top 6 overall customers was from the state or year you filter to, they're not going to be in that chart.

Think of it like the order of operations for math. Let's process the equation 4+5x2. <i>First</i> we multiply 5x2 and get 10, because multiplication gets processed before addition. <i>Then</i> when we add 4, it only applies the what we had left after we multiplied, 10. We don't multiply again just because we added 4 to the results.

When Tableau processes the dashboard onto our screens, the parameters are processed before the filters, just like multiplication is applied before addition. <i>First</i> we apply the parameter to every year and state and get 6 customers. <i>Then</i> when we add a filter, it only applies those 6 customers because that is all we have left after applying the parameter. We don't apply the parameter again just because we added a filter to the results.

Fixing it was as easy as looking at Tableau's "order of operations" and seeing if there was any workaround to apply the filters before applying the parameters. Turns out, Context filters get processed before parameters. So I added the filters to the context of the Top Customers chart so the filters would be processed before the parameters.


