# Effects of Varying Speed Limits on Fatal Car Crashes

## Problem Definition

### Research Question:
Among NCDOT state-maintained roads, not including interstate roads, in Mecklenburg County, how does the rate of fatal and serious-injury crashes vary by posted speed limit?

I am always fascinated by how many terrible drivers there are on the road, despite there being an entire process to determine your competency to operate an automobile. Therefore, I decided to study whether varying speed limits have an association with fatal car crashes. I decided to exclude interstate highways, as those have extremely high speed limits, which in turn will have a very high number of fatal automobile incidents. My goal was to understand where the most fatal accidents occurred on regular roads. With these findings, I believe we can have a better understanding of the dangers of driving as well as maybe finding ways to improve the safety of roads in Mecklenburg County


## Data Description

For my data, I pulled from a variety of different ArcGIS APIs, each containing different mapping data. I had one table with records of every fatal or serious injury crash from 2016 to 2026 in North Carolina. I had another table with the different speed limits of state-maintained roads in North Carolina. Finally I had a table with basic county data for every county in North Carolina.

The main variables I was interested in were the coordinates of roads and crashes for mapping purposes, and the posted speed limit for each road.


## Data Cleaning and Preparation

Despite the APIs having data for all of North Carolina, my queries to the server focused on data solely for Mecklenburg County. This helped to narrow the number of records I pulled from the API and allow me to work with a more manageable dataset.

In addition to Mecklenburg County, I also queried road data from the surrounding counties in order to find roads that crossed the border between counties, and then cut them off so that only the part that was in Mecklenburg County was kept. I also ensured that only records for state-maintained roads, not including interstate roads, was kept within the dataset.

As for the crash data, there seemed to be a couple errors with the data where the location of a crash was no within the borders of Mecklenburg county. To deal with this issue, I decided to keep any crashes that were within a 50 foot radius of the Mecklenburg County border, as I believed that those crashes were within the margin of error for the coordinates provided by the NCDOT dataset and the mapping abilities of the GeoPandas library. Anything farther than the 50 foot limit was removed from the dataset.

Also the dataset I used for the speed limits of roads only contained information regarding roads that are maintained by the state, meaning any roads maintained by local governments was not included in the dataset. This meant that many crashes listed within the crash dataset did not have a road to connect to. I decided once gain on a 50 foot cutoff to connect a crash point to its nearest road. If a state-maintained road was not within 50 feet of a crash, that crash was removed form the dataset.

##  Visualizations and Insights

The product of my analysis was a map of Mecklenburg County showcasing state-maintained roads, color-coded by the posted speed limit of that road. There are also the crash points that have occurred on those roads over the past decade.

I also have a bar plot of the number of fatal crashes that occurred on different speed limits. However this does not take into account, the different in road length of each speed limit. That's why I made another visual that took the ratio of fatal crashes per mile of road length for each speed limit. This gives a more balanced comparison between the different speed limits.

## Storytelling and Narrative 

With the analysis that I performed, I was able to determine that there was a slight correlation between posted speed limit and fatal crashes, with fatal crashes per mile of road length increasing as posted speed limit increases.

However, there are some issues with the data that does provide some unusual conclusions. The fatal crash rate per road mile at 20 mph is almost at 4, towering over the rest of the speed limits. This is not because so many fatal crashes occur at 20 mph, but that there is one fatal crash at that speed limit, and this occurred because a car hit a pedestrian. Because there is only a quarter mile of state-maintained roads at 20 mph, the ratio is very high. This makes it easy to misunderstand what the bar plot is showing, thinking that there are many fatal crashes at 20 mph when that is not the case.

## Limitations, Ethics, and Reflection

There are some limitations with this dataset. For one, this does not include all roads in Mecklenburg County, only state-maintained roads. While only 10% of the fatal crash data was removed due to not having a corresponding road in the roads dataset, this could still have an impact on the final conclusion of this research question, and as such needs to be taken into consideration.

## Code and Transparency 

As I am not very familiar with ArcGIS or working with mapping data, there was the usage of ChatGPT's GPT-5.6 Sol to help generate code to manipulate the data using GeoPandas, and successfully pull from the ArcGIS API. The purpose of using ChatGPT was not to brainstorm ideas but to execute on my own ideas through the help of code generation. 
