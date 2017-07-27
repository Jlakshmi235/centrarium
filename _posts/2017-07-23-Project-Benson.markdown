---
layout: post
title:  "Project Benson!"
date:   2017-07-23 11:49:45 +0200
categories: jekyll update
---



## Introduction:

As a part of the Metis Data Science Bootcamp, I wrapped up my first mini-project earlier this week. We were given the task of collaborating with WomenTechWomenYes (WTWY), a fictitious organization, who wishes to spread word of their annual gala (on May 30th 2017) by placing street teams in front of various New York MTA stations. The street teams intend to interact with the crowd and get them to sign up for the gala. The goal of the gala is to increase the participation of women in technology and build awareness. In order to optimize the effectiveness of the street teams and increase the gala attendance, we were asked to harness the power of data analytics and suggest the stations and timings for the street teams to be placed.

## Data Resources:

In order to achieve this, we wanted to maximize not just the number of commuters the street teams would see, but the number of people who would most likely be interested in their program and gala. This means we needed data on subway traffic and demographic information about who would be likely be using the subway stations, including sex, education, and profession.


The data sources used include:

* The GPS coordinates and the foot traffic (turnstile counts) for each MTA station was obtained from [http://www.mta.info/](http://www.mta.info/).

* Demographics of the neighborhoods near the stations was obtained from [http://maps.nyc.gov/census](http://maps.nyc.gov/census/).

* Locations of the technology startups was obtained from [http://www.digital.nyc/](http://www.digital.nyc/)

The demographic information collected includes the sex ratio, educational information (highest degree attained) and professions. The tech startups could be used as a tracer for identifying tech hubs, and then the GPS coordinates of the stations could be used to locate stations near those hubs. By combining this information with the ridership information, we sought to increase the likelihood of canvassing female employees of technology companies.

<img src="/images/tech_hub_map.png" width="500">

## Data Preprocessing:

As the gala is scheduled for 30th May, 2017, WTWY has planned to canvas the MTA stations in April and May. Hence, the analysis was done on data from April-May 2016.

Here is a sample of the data obtained from web.mta.info:

The turnstile data was created by audits of the turnstiles every 4 hours. The turnstile data includes

* Control Area (C/A),
* Unit, a unique identifier for each station
* Subunit Control Positions (SCP)
* Station name
* Line names of subway lines served by the station
* Division (original subway line)
* Date of audit
* Time of audit
* Description of audit (usually “Regular”)
* Cumulative entries
* Cumulative exits

Notice that a particular station can have multiple lines. But the entrances to platforms for different lines with the same station name can be located on two ends of a street. Hence, a combination of Station name and Line names is required to identify a station platform. 

Within each station, there can be multiple C/A’s. Each C/A has multiple turnstiles identified by its SCP, but the SCP number is not necessarily unique across different C/A’s (N024 and R306 both have a turnstile with SCP number 00-00-00). Hence, a combination of the C/A and SCP is required to identify unique turnstile.

The entry and exit counts for each turnstile are cumulated every 4 hours. While pre-processing the data, we noticed that some turnstile cumulative totals decreased. This could be due to the turnstiles being reset. A few turnstiles would continually decrease over some periods of time for unknown reasons. These anomalous readings were discarded. 

Not all audit timings were synchronized. Most turnstiles readings were recorded at midnight and every 4 hours until the next midnight. The second most common series started at 1:00am. Most of the remainder had start times that were completely random. To fix this, and to synchronize the data, we considered anything between 10:00pm and 2:00am as being midnight. This shifted some of the data by at most 2 hours, but it allowed us to more directly compare the turnstile counts based on morning, midday, and afternoon intervals. 

Some turnstile counts were unrealistically high. To fix this, we made an upper limit of counts we would accept. 
Some readings suggested that people as much as 850K passed through a particular turnstile in a day. In order to set the upper bound, we examined the histograms which binned the counts for all turnstiles (shown below). After 9000, the histograms readings get really sparse and look anomalous. Hence, we used 10,000 as an upper limit, leaving commuter counts that are likely real and typical for each turnstile.


## Stations with highest ridership:


<img src="/images/station_ridership_barh.png" width="600">

After cleaning the data, we calculated the total ridership for different stations (identified by Station - Line name) by summing up the counts in the 4 hour intervals.

<img src="/images/station_day_heat.png" width="700">


## Stations within tech hubs:

Using the coordinates of each station, we could find the stations within a targeted region. Using the map on NYC startup locations as a tracer, we identified two tech hubs namely Silicon Alley and Lower Manhattan.

To get stations near tech hubs, we used coordinate boxes that encompassed those areas, and did a search of stations within that area. For example, to search the area of dense tech companies just south of Central Park, we limited the search to stations with longitudes between -73.994992° and -73.975443°, and latitudes between 40.735691° and 40.759721°.

The following is the data frame created showing the top stations in the target area, showing their positions for all of New York City, and the total counts for April/May 2016.


## Targeting Demographics:

Although, sending the WTWY street teams to the busiest stations would result in higher interactions with people, its not necessary that they would be interested in the gala. 

<img src="/images/station_top_day_hour_heat.png" width="700">

t
