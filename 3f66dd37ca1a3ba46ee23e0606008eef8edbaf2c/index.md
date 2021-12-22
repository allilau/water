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

For the MUSA550 final project, I wanted to visualize a rainfall dataset that contains hourly precipitation measurements from 1990 to 2011 for each of the 24 rain gauges in Philadelphia. For the second part of the project, I wanted to examine PWD's main break dataset and create visualizations that will look into the concentration of circumferential break types in 2020. 

## a. rainfall in philadelphia from 1990-2011
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
