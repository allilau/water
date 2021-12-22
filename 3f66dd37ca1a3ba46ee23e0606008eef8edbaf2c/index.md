---
layout: default
altair-loader:
  altair-chart-1: ["charts/breaktypes2020.json", "400"]
  altair-chart-2: "charts/circbreaks2020_map.json"
hv-loader:
  hv-rain-map-1: ["charts/rainfall_map.html", "740"] # second argument is the desired height
  hv-chart-2: ["charts/dayheatmap.html", "860"]
  hv-watershed-map-1: ["charts/watershed_maptrim.html", "750"]
folium-loader:

---

# welcome

For the MUSA550 final project, I wanted to visualize a rainfall dataset that contains hourly precipitation measurements from 1990 to 2011 for each of the 24 rain gauges in Philadelphia. For the second part of the project, I wanted to examine PWD's main break dataset and create visualizations that will look into the concentration of main break types. Due to the large size of the original dataset, I decided to only explore the circumferential main breaks that occurred in 2020. The leading questions that helped drive the creation of the visuals presented below are as follows: 
- Where are cloudburst events concentrated in the city (i.e. northeast, southwest) seasonally? What months did they occur? How can we visualize the distribution of rainfall during one of these events?
- Where are water main breaks located and concentrated in Philadelphia in 2020? 

## a. rainfall in philadelphia from 1990-2011
Over 700,000 rows of raw data from rain gauges throughout the city without QAQC, the dataset contained some errors. To read the csv in through pandas, the parameter 'error_bad_lines' had to be adjusted since lines with too many fields will by default cause an exception to be raised, and no DataFrame will be returned.

```df = pd.read_csv("Rainfall_data_19900101_20210602_EST.csv", 
                    error_bad_lines=False,
                    engine='python')
```

Setting the parameter 'error_bad_lines' as False, the "bad lines" are dropped from the DataFrame that is returned. The dataset was further trimmed to only columns that were needed and missing data was removed (i.e. empty columns for rain gauges 25-37, missing data in the columns for coordinates). 

Another note is that the dataset was in a format where each rain gauge had its own column. Because I'm primarily interested in the temporal changes of rainfall in the city, I wanted the data in a format where each row is an observation of the rainfall measurement for each gauge (i.e. date column). I used the melt() function for converting to the desired format. The to_datetime() method was also essential to this analysis and used to convert string Date time into a Python date time object. 

### rainfall daily totals 
Cloudburst events are After prepping the data, the first step of analysis includes calculating the total rainfall per day for each of the 24 gauges. 

below is an interactive heatmap of the rain gauge rainfall totals for each day (1990-2011)

<div id="hv-chart-2"></div>

### rainfall seasonal totals 

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
