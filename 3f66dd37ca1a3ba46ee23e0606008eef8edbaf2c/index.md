---
layout: default
altair-loader:
  altair-chart-1: "charts/measlesAltair.json"
hv-loader:
  hv-chart-1: ["charts/measlesHvplot.html", "920"] # second argument is the desired height
  hv-chart-2: ["charts/dayheatmap.html", "900"]
folium-loader:
  folium-chart-1: ["charts/foliumChart.html", "400"] # second argument is the desired height
  folium-chart-2: ["charts/percent_no_internet.html", "400"] # second argument is the desired height
---

# welcome

For the MUSA550 final project, I want to visualize a rianfall dataset that contains hourly precipitation measurements from 1990 to 2011 for each of the 24 rain gauges in Philadelphia. 

For examples of how to use markdown to style text, see this [this page](./another-page.html).

# Example: Embedding Altair & Hvplot Charts

This section will show examples of embedding interactive charts produced using [Altair](https://altair-viz.github.io) and [Hvplot](https://hvplot.pyviz.org/).

## rainfall daily totals 

below is an interactive heatmap of the rain gauge rainfall totals for each day (1990-2011)donut

<div id="hv-chart-2"></div>

## rainfall seasonal totals 

below is an interactive map of the rain gauge rainfall seasonal totals (1990-2011)

<div id="hv-rain-map-1"></div>

#rainfall seasonal totals for each watershed

below is an interactive map of the rain gauge rainfall seasonal totals for each watershed within Philadelphia (1990-2011)

<div id="hv-watershed-map-1"></div>

# Example: Embedding Folium charts

This post will show examples of embedding interactive maps produced using [Folium](https://github.com/python-visualization/folium).

## OSMnx and Street Networks

The shortest route between the Art Museum and the Liberty Bell:

<div id="folium-chart-1"></div>

<br/>

## Percentage of Households without Internet

The percentage of households without internet by county:

<div id="folium-chart-2"></div>

See the [lecture 9B slides](https://musa-550-fall-2021.github.io/slides/lecture-9B.html) and the [lecture 10A slides](https://musa-550-fall-2021.github.io/slides/lecture-10A.html) for the code that produced these plots.

