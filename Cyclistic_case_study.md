## **Google Data Analytics Capstone Project: Cyclistic bike-share analysis**
Kenny Phan    
2022-05-06

<br/><br/>
***About the company***

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that
are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and
returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments.
One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes,
and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers
who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the
pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will
be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a
very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic
program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to
do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why
casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are
interested in analyzing the Cyclistic historical bike trip data to identify trends.

<br/><br/>
***Scenario***

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director
of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore,
your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights,
your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives
must approve your recommendations, so they must be backed up with compelling data insights and professional data
visualizations.

Three questions will guide the future marketing program:
* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned you the first question to answer: How do annual members and casual riders use Cyclistic bikes differently?

<br/><br/>
***Business Task***

Our goal is to analyze rider's usage of Cyclistic from historical data to determine if annual members and casual riders differ and
offer insights to stakeholders on recommended conversion program(s) to increase Cyclistic overall profit through additional annual memberships.

<br/><br/>
***Data***

All data used in this analysis is Cyclistic first party sourced gathered from their customers and can be found **[here](https://divvy-tripdata.s3.amazonaws.com/index.html)**.
Specific datasets used can be downloaded in the following: 
**[2022-04](https://divvy-tripdata.s3.amazonaws.com/202204-divvy-tripdata.zip)**, 
**[2022-03](https://divvy-tripdata.s3.amazonaws.com/202203-divvy-tripdata.zip)**, 
**[2022-02](https://divvy-tripdata.s3.amazonaws.com/202202-divvy-tripdata.zip)**, 
**[2022-01](https://divvy-tripdata.s3.amazonaws.com/202201-divvy-tripdata.zip)**,
**[2021-12](https://divvy-tripdata.s3.amazonaws.com/202112-divvy-tripdata.zip)**,
**[2021-11](https://divvy-tripdata.s3.amazonaws.com/202111-divvy-tripdata.zip)**,
**[2021-10](https://divvy-tripdata.s3.amazonaws.com/202110-divvy-tripdata.zip)**,
**[2021-09](https://divvy-tripdata.s3.amazonaws.com/202109-divvy-tripdata.zip)**,
**[2021-08](https://divvy-tripdata.s3.amazonaws.com/202108-divvy-tripdata.zip)**,
**[2021-07](https://divvy-tripdata.s3.amazonaws.com/202107-divvy-tripdata.zip)**,
**[2021-06](https://divvy-tripdata.s3.amazonaws.com/202106-divvy-tripdata.zip)**,
**[2021-05](https://divvy-tripdata.s3.amazonaws.com/202105-divvy-tripdata.zip)**.
The data has been made available by Motivate Internation Inc. under this **[license](https://ride.divvybikes.com/data-license-agreement)**. Though this is first party source, note
that data-privacy issues prohibit you from using riders' personally identifible information. This means that you won't be able to connect pass purchases to credit card
numbers to determine if casual riders live in the Cyclistic service area or if they have puchased multiple single passes.

To keep the analysis as current as possible, only the past 12 months were used.    
Microsoft Excel power query is used as the files already came in a .csv format.    
Power query is used to organize data by:
* Removed all columns not needed for the business task (e.g., start_lat, start_lng, end_lat, end_lng, start_station_id, and end_station_id)
* Removed any blank rows
* Removed error cells
* Removed any leading and trailing whitespaces and line breaks
* Removed any duplicate rows
* Calculated ride time (duration of each ride).
* Calculated day of the week for each ride.



All datasets were then consolidated into MySQL for ease of data transformation and performing aggregate functions as well has having all datasets combined in one place.    
All SQL queries are located **[here](https://github.com/kxp2770/Google_Capstone/blob/main/SQL_Query.md)**.    
While working with the data in MySQL, the column member_casual which stores whether or not the ride was by a member or a casual individual had carriage returns in column that was missed during excel cleaning process. When running queries to determine member counts, values returned as 0 which brought up the idea that data is not always fully cleaned.
All new aggregated data tables were exported into csv files for data viz usages in Tableau. Tables created are:
[total rides by membership by days](https://public.tableau.com/views/TotalRidesbyMembershipStatusbyDay/Dashboard2?:language=en-US&:display_count=n&:origin=viz_share_link) and 
[total rides by membership by months](https://public.tableau.com/views/TotalRidesbyMembershipStatusbyMonth/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)


<div class='tableauPlaceholder' id='viz1652124118444' style='position: relative'><noscript><a href='#'><img alt='Dashboard 2 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;To&#47;TotalRidesbyMembershipStatusbyDay&#47;Dashboard2&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='TotalRidesbyMembershipStatusbyDay&#47;Dashboard2' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;To&#47;TotalRidesbyMembershipStatusbyDay&#47;Dashboard2&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' /></object></div>               




<div class='tableauPlaceholder' id='viz1652124009978' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;To&#47;TotalRidesbyMembershipStatusbyMonth&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='TotalRidesbyMembershipStatusbyMonth&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;To&#47;TotalRidesbyMembershipStatusbyMonth&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>             

<br/><br/>
***Summary***

