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
- Where are cloudburst events concentrated in the city (i.e. northeast, southwest) seasonally? How can we visualize the distribution of rainfall during one of these events?
- Where are water main breaks located and concentrated in Philadelphia in 2020? 

## a. rainfall in philadelphia from 1990-2011
Over 700,000 rows of raw data from rain gauges throughout the city without QAQC, the dataset contained some errors. To read the csv in through pandas, the parameter 'error_bad_lines' had to be adjusted since lines with too many fields will by default cause an exception to be raised, and no DataFrame will be returned.
```df = pd.read_csv("Rainfall_data_19900101_20210602_EST.csv", 
                    error_bad_lines=False,
                    engine='python')
```
Setting the parameter 'error_bad_lines' as False, the "bad lines" are dropped from the DataFrame that is returned. 
### rainfall daily totals 

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
