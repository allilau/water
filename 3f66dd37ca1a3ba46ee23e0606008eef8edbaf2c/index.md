---
layout: default
altair-loader:
  altair-chart-0: ["charts/breaktypes2020.json"]
  altair-chart-2: ["charts/circbreaks2020_map.json"]
hv-loader:
  hv-rain-map-1: ["charts/rainfall_map.html", "660"] # second argument is the desired height
  hv-chart-2: ["charts/dayheatmap.html", "830"]
  hv-watershed-map-1: ["charts/watershed_maptrim.html", "660"]
folium-loader:
  folium-chart-1: ["charts/breakheatmap.html", "500"]
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
How can we visualize where these rain gauges are located in the city / where do these cloudburst events occur on a local scale? How can we visualize the seasonal patterns of preciptation in the city? Seasonal statistics for precipitation are helpful in planning work, as it helps identify patterns of heavy precipitation in the city through the year. Often times, precipitation statistics are presented as annual averages, but seasonal averages are more applicable in a local context. 

To prepare and analyze the heatmap daily totals dataset to calculate the seasonal totals, I first determined the monthly rainfall totals for each gauge using the groupby() operation and taking the sum of daily rainfall for each month (following the same procedure from the section above). To find the seasonal totals, I first had to assign months to the appropriate season and then created a new column in the dataset with the relevant season. Winter months include December through February (12, 1, 2); Fall months are from September through November (9-11); Spring months  are from March through May (3-5); Summer months are from June through August (6-8). 

```python
conditions = [
    (monthtt.DateTime.dt.month.isin([12, 1, 2])),
    (monthtt.DateTime.dt.month.isin([9, 10,11])),
    (monthtt.DateTime.dt.month.isin([3, 4, 5])), 
    (monthtt.DateTime.dt.month.isin([6, 7, 8])),
  ]
choices = ['winter','fall', 'spring', 'summer']
monthtt['season'] = np.select(conditions, choices)
```
At this point, we have each gauge month record associated with a season. We have still need to aggregate the months to find the seasonal totals. I first created a new column with just the year using the .strftime() parameter for %Y, and used the groupby() operation to find the seasonal rainfall totals for each gauge. 

To visualize spatially, we need associated coordinates for each of the 24 gauges. I loaded the csv with the relevant longitude and latitude coordinates for each gauge, dropped any missing coordinate values, and used the geopandas .points_from_xy() to generate a GeometryArray of shapely Point geometries. I then converted it to a GeoDataFrame and specified its original CRS in EPSG:4326 (or WGS 84). Philadelphia has a latitude of about 40 degrees and longitude of about -75 degrees, therefore I was able to assume that it is in EPSG=4326 after looking at the coordinates column in the original dataframe. In the final step, I merged the GeoDataFrame with the seasonal totals dataframe, while dropping any missing values for seasons. 

The GeoDataFrame for seasonal rainfall totals was then plotted using hvplot.points and included a widget-dropdown menu for each season and year. Below is the resultant interactive map of the rain gauge rainfall seasonal totals (1990-2011). 

<div id="hv-rain-map-1"></div>

The tooltip identifies the gauge, its location, and the total rainfall measured at its location for the chosen season and year. Similar to the previous section, I identified the top 5 seasonal precipitation totals. The most precipitation in Philadelphia from 1990-2011 occured in the summer months, with over 20 inches of rainfall accumulated in parts of the city. From the map above, you can see that rain gauges 21 (RG21) and 22 (RG22) are located in northwest Philadelphia and rain gauges 5 (RG5) and 12 (RG12) are located in the southeast corner of the city. These areas received the most rainfall in the summer of 2006 and 2009. 

~![top-5-seasonal-rainfall]({{ site.url }}{{ site.baseurl }}/assets/img/top10seasontotalrainfall.PNG)

### rainfall seasonal totals for each watershed
Currently, seasonal precipitation totals are helpful for determining precipitation projections. However, the resolution of available precipitation-prediction models are not yet high enough to to accomodate each single gauge location. A larger geometry/coverage area is needed, such as splitting the area into quadrants. Because we are dealing with hydrology, organizing and binning rain gauges into their respective watersheds can be one method to scale to a useful coverage area and would be helpful for future work with downscaled projections from regional and global climate models. Philadelphia's land surface ultimately drains into one of the [seven watersheds that are tributaries to the Delaware River.](http://archive.phillywatersheds.org/your_watershed/delaware#:~:text=Philadelphia's%20entire%20land%20surface%20ultimately,tributaries%20to%20the%20Delaware%20River). There are a total of 7 major watersheds in the Philadelphia region: Wissahickon, Schuylkill, Darby-Cobbs, Delaware, Tacony-Frankford, Pennypack, and Poquessing. 

I retrieved the major watersheds in Philadelphia and their geometries from [OpenDataPhilly](https://www.opendataphilly.org/dataset/major-watersheds) and imported it as a geojson. I specified the crs as EPSG:4326 and used the spatial join .sjoin() operation to categorize each rain gauge and rainfall total into their respective watershed areas. I then trimmed the GeoDataFrame to only show the columns I needed. 

```python
rainfall_watershed = gpd.sjoin(rainfall_gauge, watersheds, op='within', how='left')
```
*How can we visualize the seasonal totals for each watershed for years 1990-2011?*
I have seasonal rainfall totals for each gauge, but want to find the seasonal totals for each watershed. Using the groupby() operation, I created a new dataframe that grouped the gauges into the watershed and calculated the total seasonal rainfall for each area.

```python
out = rainfall_watershed.groupby(['WATERSHED_NAME', 'year', 'season'], as_index=False)['inches'].sum()
```

Finally, I merged it with the previous GeoDataFrame that contains the coordinates for each watershed and removed missing data for the years. Using hvplot.polygons, I created an interactive map of the rain gauge rainfall seasonal preciptation totals for each watershed within Philadelphia (1990-2011). The user can toggle the year and season to show the seasonal rainfall totals for each of the 7 watersheds. Dark green areas indicate a large amount of rainfall, while light yellow/white areas show a smaller total amount. 

<div id="hv-watershed-map-1"></div>

Consistent with the previous section, the summer of 2006 and 2009 received the most rainfall. This occured in the Schuylkill River and Delaware Watersheds. 

## b. main breaks in Philadelphia (2020)
Philadelphia's main break data is considered the oldest in the country with records dating back to 1964.  It is manually updated with written work orders, handled and organized by PWD's GIS and linear asssets planning unit. With over 47,000 records of main breaks in the city, I wanted to examine a smaller subset that can be visualized in a meaningful way. Because the 2018-2020 main break entries are considered most accurate due to a revised internal QAQC process, I decided to focus on the main breaks that occurred in 2020. 

### types of main breaks
To prepare the data, I first removed missing latitude and longitude values and converted the dataframe to a GeoDataFrame using gpd.points_from_xy(). I also trimmed the dataset to the columns I needed. There is a separate csv that contains the main break type associated with a number in the main break dataset. I merged these dataframes so that there is a main break type-description with each entry. In order to select the main breaks in 2020, I first had to convert the string Date time into a Python date time object (same procedure done with the rainfall dataset above). The merged dataframe was then converted to a GeoDataFrame with a specified crs of EPSG:4326. I then selected the main breaks that occured in 2020 and created a new dataframe ('breaks2020'):

```python
breaks2020 = breaks.loc[(breaks['BreakDate'] >= '2020-01-01')
                               & (breaks['BreakDate'] < '2020-12-31')]
```
There were a total of 604 (recorded) main breaks in 2020. I was interested in exploring the most common types of main breaks that happened in the city. For this, I first had to catgeorize by the type of main break using the groupby() operation and calculate the count for each type using .size(). 

<div id="altair-chart-0"></div>

Using altair, I created an interactive bar chart of the number of main breaks that occurred in 2020, grouped by type in descending order (from left to right) shown below. The tooltip displays the type description and the number of breaks that occurred for that type of break. 

The most breaks that occurred in 2020, by a margin of 191, was circumferential (with a total of 288 breaks in 2020). *Is this consistent with the rest of the main breaks in Philadelphia?* When compared to the top 5 types of main breaks that occurred since 1964, circumferential is also the most common type of break - occuring almost 3 times more than the second most common type of break (longitudinal). Circumferential breaks are among the most common type of main breaks, usually due to older types of bedding material. 

~![top-5-main-break-type]({{ site.url }}{{ site.baseurl }}/assets/img/top5break.PNG)

### where did circumferential breaks occur? 
How can we visualize the circumferential breaks in 2020? I created a dataframe with only the circumferential main break types in 2020 using the .query() operation. To visualize at first glance, I created a heatmap using folium, centered in Philadelphia. 

```python
circ_m = folium.Map(
    location=[39.99, -75.13],
    tiles='Cartodb Positron',
    zoom_start=12,
)
HeatMap(circ_2020_coords, radius=15).add_to(circ_m)
```
<div id="folium-chart-1"></div>
The heatmap shows that in 2020, main breaks mostly occurred in Southeast Philadelphia. This correlates with the PGW event that occurred in that year, which ended up also breaking many transmission mains. 

For a more detailed analysis, I wanted to explore the breaks by neighborhood. Using the zillow dataset from class, I spatially joined the neighborhood data with the 2020 main break data so that each event is associated with a neighborhoood. I then used the .groupby() operation and .size() to determine the number of main breaks that occurred in each neighborhood. I merged this dataframe back with the zillow neighborhood data so that I can plot this visually using altair.

Below is an interactive map that displays the number of main breaks that occurred in each neighborhood in 2020. Neighborhoods with no main breaks are shown in white. The tooltip displays the neighborhood name and the number of main breaks. 
<div id="altair-chart-2"></div>
The most main breaks in 2020 occured in Grays Ferry (total of 12 events). 
<br/>
