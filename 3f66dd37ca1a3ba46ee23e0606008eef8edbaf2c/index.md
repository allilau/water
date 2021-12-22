---
layout: default
altair-loader:
  altair-chart-1: ["charts/breaktypes2020.json", "400"]
  altair-chart-2: "charts/circbreaks2020_map.json"
hv-loader:
  hv-rain-map-1: ["charts/rainfall_map.html", "740"] # second argument is the desired height
  hv-chart-2: ["charts/dayheatmap.html", "830"]
  hv-watershed-map-1: ["charts/watershed_maptrim.html", "750"]
folium-loader:

---

# welcome

For the MUSA550 final project, I wanted to visualize a rainfall dataset that contains hourly precipitation measurements from 1990 to 2011 for each of the 24 rain gauges in Philadelphia. For the second part of the project, I wanted to examine PWD's main break dataset and create visualizations that will look into the concentration of main break types. Due to the large size of the original dataset, I decided to only explore the circumferential main breaks that occurred in 2020. The leading questions that helped drive the creation of the visuals presented below are as follows: 
- Where are cloudburst events concentrated in the city (i.e. northeast, southwest) seasonally? What months did they occur? How can we visualize the distribution of rainfall during one of these events?
- Where are water main breaks located and concentrated in Philadelphia in 2020? 

## a. rainfall in philadelphia from 1990-2011
Over 700,000 rows of raw data from rain gauges throughout the city without QAQC, the dataset contained some errors. To read the csv in through pandas, the parameter 'error_bad_lines' had to be adjusted since lines with too many fields will by default cause an exception to be raised, and no DataFrame will be returned.

```python
df = pd.read_csv("Rainfall_data_19900101_20210602_EST.csv", 
                    error_bad_lines=False,
                    engine='python')
```

Setting the parameter 'error_bad_lines' as False, the "bad lines" are dropped from the DataFrame that is returned. The dataset was further trimmed to only columns that were needed and missing data was removed (i.e. empty columns for rain gauges 25-37, missing data in the columns for coordinates). 

Another note is that the dataset was in a format where each rain gauge had its own column. Because I'm primarily interested in the temporal changes of rainfall in the city, I wanted the data in a format where each row is an observation of the rainfall measurement for each gauge (i.e. date column). I used the melt() function for converting to the desired format. The to_datetime() method was also essential to this analysis and used to convert string Date time into a Python date time object. This returned the Date time column into the format 'YYYY-MM-DD HH:MM:SS-AA'.

### rainfall daily totals 
Cloudbursts are events with sudden, very intense rainfalls that occur for a brief duration of time (within a day period). Cloudbursts are local events, where at times only one part of the city can experience it. For this reason, I wanted to examine the total precipitation per gauge, as grouping them together would lower the precision of the results.

To examine when these events occur, we would have to analyze the dataset on a daily level. After prepping the data, the first step of analysis included calculating the total rainfall per day for each of the 24 gauges. This involved the groupby() operation and taking a sum of the inches for the day for each gauge. In order to calculate the sum by day, I had to group by the gauge and return the DateTime back to a string to represent just the YYYY-MM-DD, without the hourly 'HH:MM:SS'. 

```python
dayavg = df.groupby(["gauge", df.DateTime.dt.strftime('%m-%d-%Y')])['inches'].sum().reset_index()
```

The DateTime column was then converted back to a Python date time object in order to perform further analysis. The hvplot heatmap was chosen to visually represent the daily totals, where the user can filter between the gauges and years and determine (based on a darker blue color), when precipitation was highest in that year for the particular rain gauge. Below is an interactive heatmap of the rain gauge rainfall totals for each day (1990-2011)

<div id="hv-chart-2"></div>

The top precipitation events from 1990-2011 occured on September 16, 1999 and September 28, 2004, with a daily maximum total of 8.7 and 8.3 inches, respectively. Several gauges reported 8+ inches of rainfall on 9/16/1999. I used the nlargest() function to sort through the heatmap dataset and return the top 10 daily precipitation totals. 

~![top-10-daily-rainfall]({{ site.url }}{{ site.baseurl }}/assets/img/top10dailytotalrainfall.PNG)

### rainfall seasonal totals 
How can we visualize where these rain gauges are located in the city / where do these cloudburst events occur on a local scale? 


below is an interactive map of the rain gauge rainfall seasonal totals (1990-2011)

<div id="hv-rain-map-1"></div>

### rainfall seasonal totals for each watershed

below is an interactive map of the rain gauge rainfall seasonal totals for each watershed within Philadelphia (1990-2011)

<div id="hv-watershed-map-1"></div>

## b. main breaks in Philadelphia (2020)

### types of main breaks

below is an interactive bar chart of the number of main breaks that occurred in 2020, grouped by type in descending order (from left to right). 

<div id="altair-chart-1"></div>

### where did circumferential breaks occur? 

<div id="altair-chart-2"></div>

<br/>
