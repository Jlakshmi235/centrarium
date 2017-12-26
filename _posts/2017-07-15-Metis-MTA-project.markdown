---
title: Project 1 @ Metis Complete
date: 2017-07-15 09:49:45 Z
categories:
- project
layout: post
---

<style type="text/css">
	ul.a {
    list-style-type: circle;
}
</style>
<img src="/images/mta.jpg" width="100%" height="100%" align="middle">                                                                                                                  
## Introduction:

As a part of the Metis Data Science Bootcamp, I wrapped up my first mini-project earlier this week. We were given the task of collaborating with WomenTechWomenYes (WTWY), a fictitious organization, who wishes to spread word of their annual gala (on May 30th 2017) by placing street teams in front of various New York MTA stations. The street teams intend to interact with the crowd and get them to sign up for the gala. The goal of the gala is to increase the participation of women in technology and build awareness. In order to optimize the effectiveness of the street teams and increase the gala attendance, we were asked to harness the power of data analytics and suggest the stations and timings for the street teams to be placed.

## Data Resources:

In order to achieve this, we wanted to maximize not just the number of commuters the street teams would see, but the number of people who would most likely be interested in their program and gala. This means we needed data on subway traffic and demographic information about who would be likely be using the subway stations, including sex, education, and profession.


The data sources used include:

* 1) The GPS coordinates and the foot traffic (turnstile counts) for each MTA station was obtained from [http://www.mta.info/](http://www.mta.info/).

* 2) Demographics of the neighborhoods near the stations was obtained from [http://maps.nyc.gov/census](http://maps.nyc.gov/census/).

* 3) Locations of the technology startups was obtained from [http://www.digital.nyc/](http://www.digital.nyc/)

The demographic information collected includes the sex ratio, educational information (highest degree attained) and professions. The tech startups could be used as a tracer for identifying tech hubs, and then the GPS coordinates of the stations could be used to locate stations near those hubs. By combining this information with the ridership information, we sought to increase the likelihood of canvassing female employees of technology companies.



## Data Preprocessing:

As the gala is scheduled for 30th May, 2017, WTWY has planned to canvas the MTA stations in April and May. Hence, the analysis was done on data from April-May 2016.

Here is a sample of the data obtained from web.mta.info:
<img src="/images/p1_dataFrame.png" width="100%" height="100%" align="middle">


The turnstile data was created by audits of the turnstiles every 4 hours. The turnstile data includes

1. Control Area (C/A)<br/>  
2. Unit, a unique identifier for each station<br/>
3. Subunit Control Positions (SCP)  <br/>
4. Station name  <br/>
5. Line names of subway lines served by the station  <br/>
6. Division (original subway line)  <br/>
7. Date of audit  <br/>
1. Time of audit  <br/>
1. Description of audit (usually “Regular”)  <br/>
1. Cumulative entries  <br/>
1. Cumulative exits  <br/>

Given below is a sample of the data collected a 103rd Street Station (103 ST):

<img src="/images/p1_station.png"  style="width:30%;height: 30%;" align="middle"> <br/>
<img src="/images/103st.png"  style="width:100%;height: 100%;" align="middle"> <br/>
Notice that a particular station can have multiple lines. But the entrances to for different lines with the same station name can be located a couple of blocks away. Hence, in this project, traffic at each individual combination of Station name and Line names is calculated for placing the street teams. 

Within each station, there can be multiple C/A’s. Each C/A has multiple turnstiles identified by its SCP, but the SCP number is not necessarily unique across different C/A’s (R170 and R252 both have a turnstile with SCP number 00-00-00). Hence, a combination of the C/A and SCP is required to identify unique turnstile.

The entry and exit counts for each turnstile are cumulated every 4 hours. While pre-processing the data, we noticed that some turnstile cumulative totals decreased. This could be due to the turnstiles being reset. A few turnstiles would continually decrease over some periods of time for unknown reasons. These anomalous readings were discarded. 

Not all audit timings were synchronized. Most turnstiles readings were recorded at midnight and every 4 hours until the next midnight. The second most common series started at 1:00am. Most of the remainder had start times that were completely random. To fix this, and to synchronize the data, we considered anything between 10:00pm and 2:00am as being midnight. This shifted some of the data by at most 2 hours, but it allowed us to more directly compare the turnstile counts based on morning, midday, and afternoon intervals. 

Some turnstile counts were unrealistically high. To fix this, an upper limit of counts we would accept was made. 
Some readings suggested that people as much as 1.9 billion passed through a particular turnstile in a day. In order to set the upper bound, we examined the histograms which binned the counts for all turnstiles. After 9000, the histograms readings get really sparse and look anomalous. Hence, we used 10,000 as an upper limit, leaving commuter counts that are likely real and typical for each turnstile.


## Stations with highest ridership:


After cleaning the data, we calculated the total ridership for different stations (identified by Station - Line name) by summing up the counts in the 4 hour intervals.

<img src="/images/p1_top20.png" width="600">



## Stations within tech hubs:

Using the coordinates of each station, we could find the stations within a targeted region. Using the map on NYC startup locations as a tracer, we identified two tech hubs namely Silicon Alley and Lower Manhattan.

<img src="/images/p1_nyTech.png" width="600">

Coordinate boxes were used to find the statons that are in close proximity of the tech hubs. For example, to search the area of dense tech companies just south of Central Park, we limited the search to stations with longitudes between -73.994992° and -73.975443°, and latitudes between 40.735691° and 40.759721°.


## Targeting Demographics:

Although, sending the WTWY street teams to the busiest stations would result in higher interactions with people, its not necessary that they would be interested in the gala. So, we bought in the demographics element of the neighborhood surrounding these stations. The demographic information collected includes the sex ratio, educational information (highest degree attained) and professions. Using these numbers, the top stations were re-ranked.

1. GRD CNTRL-42 ST 4567S <br/>
2. 86 ST 456 <br/>
1. 34 ST-PENN STA ACE <br/>
1. 47-50 STS ROCK BDFM <br/>
1. 59 ST COLUMBUS ABCD1 <br/>
1. 34 ST-PENN STA 123ACE <br/>
1. W 4 ST-WASH SQ ABCDEFM <br/>
1. FULTON ST 2345ACJZ <br/>
1. LEXINGTON AV/53 EM6 <br/>
1. 42 ST-BRYANT PK BDFM7 <br/>
1. CHAMBERS ST ACE23 <br/>

Many of the top stations stayed the same after the re-ranking, but some showed a large change due to a smaller number of target commuters. Typically the percentage of residents around the stations with Bachelors was in 30-40’s range, graduate degree percentages were also in the 30-40’s, and percentage of careers in professional and scientific industries in the 10-20’s. But, as an example, Flushing-Main St. (Line 7) is in a neighborhood with residents having significantly fewer Bachelor degrees (17%), graduate degrees (8.1%), and careers in professional and scientific industries (9.1%). While Flushing-Main St. was the eight busiest station, it dropped to the 22nd top station of targeted commuters.

We compared our stations to the 10 stations with the most overall traffic.  
<strong>By going to the stations in our list, WTWY teams will see 9% fewer commuters, resulting in 5% fewer females.
However, these are targeted commuters, so you are projected to see:  
6% more people in technical professions (1-2% more females)  <br/>
10% more people with graduate degrees (5% more females)  </strong>

<img src="/images/p1_MTA_Station_Locations.png" width="700">

## Day and Time:

Now that we have figured out the stations which the WTWY teams should be canvassing, we now have to find out when they should be out canvassing.  

We found that consistently the weekdays showed much more traffic than weekends (as expected). To illustrate this, below is the average weekly traffic for 34 ST-Herald Sq (linename: 123ACE) in April/May 2016.

<img src="/images/p1_weekly_ridership.png" width="700">

However, while there is less traffic on the weekend, there are still commuter counts in the 10’s and 100’s of thousands. While the teams may not interact with as many people, they may not be in a rush to get to work or back home and perhaps more likely to talk with the street teams, so that would need to be taken into consideration.

In order to look at the best times for street times to canvass during the weekdays, we looked at traffic rates for different times of the day. All the stations we looked were busiest between 4-8pm. Overall, majority of the traffic occurs between 8am-8pm.   

To illustrate this, below is the average daily traffic for 34 ST-Herald Sq (linename: 123ACE) in April/May 2016.


<img src="/images/p1_daily_ridership.png" width="700">

## Conclusion

By using commuter rates, demographic information, and location data for MTA subway stations, we were able to create a priority list of stations that would most likely have commuters who are famales with higher education degrees and careers in tech industries, as well as the academic community from top-tier universities. While these stations did show 9% less traffic, they actually resulted in an increase of 1-2% females in science/technical professions and 5% more females with graduate degrees. We were also able to identify optimal days and times for WTWY to be convassing for their gala.

With this information, we could create a canvassing schedule based on the availability of WTWY street teams and the number of days they are able to work. Based on the initial response received by the teams, we could further optimize the schedule.

