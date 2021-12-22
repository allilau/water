---
layout: default
altair-loader:
  altair-chart-1: "charts/measlesAltair.json"
hv-loader:
  hv-rain-map-1: ["charts/rainfall_map.html", "800"] # second argument is the desired height
  hv-chart-2: ["charts/dayheatmap.html", "860"]
  hv-watershed-map-1: ["charts/watershed_maptrim.html", "850"]
folium-loader:

---

# welcome

For the MUSA550 final project, I want to visualize a rainfall dataset that contains hourly precipitation measurements from 1990 to 2011 for each of the 24 rain gauges in Philadelphia. 

## rainfall daily totals 

below is an interactive heatmap of the rain gauge rainfall totals for each day (1990-2011)cinnamon

<div id="hv-chart-2"></div>

## rainfall seasonal totals 

below is an interactive map of the rain gauge rainfall seasonal totals (1990-2011)

<div id="hv-rain-map-1"></div>

## rainfall seasonal totals for each watershed

below is an interactive map of the rain gauge rainfall seasonal totals for each watershed within Philadelphia (1990-2011)

<div id="hv-watershed-map-1"></div>

<br/>
