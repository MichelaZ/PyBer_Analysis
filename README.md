#PyBer Analysis
## Purpose:
The client wants a DataFrame to summarize data from a ride-sharing app, PyBer, by city type: urban, suburban, and rural. Using the DataFrame I created a chart showing the weekly fares for each city type and was able to analyze the data to help the client determine how city type effects the average fare. Then I gave recommendations to improve access and affordability based on this data.
## Deliverable 1: Method
1. I import dependencies.
```
# Add Matplotlib inline magic command
%matplotlib inline
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```
2. I load data from csv’s into DataFrames.
```
# File to Load (Remember to change these)
city_data_to_load = "Resources/city_data.csv"
ride_data_to_load = "Resources/ride_data.csv"

# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)
```
3. I used groupby’s to get the get the total rides,  total drivers, total fares, and  average fare per ride for each city type. I calculated the average fare per driver.
```
total_rides_by_type = pyber_data_df.groupby("type").ride_id.count()
total_rides_by_type

total_driver_by_type = city_data_df.groupby("type").driver_count.sum()
total_driver_by_type

sum_fares_by_type = pyber_data_df.groupby("type").fare.sum()
sum_fares_by_type

avg_fares_by_type = pyber_data_df.groupby("type").fare.mean()
avg_fares_by_type

avg_fare_driver_type = sum_fares_by_type / total_driver_by_type
avg_fare_driver_type
```
4. I created a summary DataFrame with the data above.
```
pyber_summary_df = pd.DataFrame({
    "Total Rides" : total_rides_by_type, 
    "Total Drivers" : total_driver_by_type,
    "Total Fares" : sum_fares_by_type,
    "Average Fare per Ride" : avg_fares_by_type,
    "Average Fare per Driver" :avg_fare_driver_type})
```
5. I removed the index name and formatted the columns.
```
pyber_summary_df.index.name = None

pyber_summary_df["Total Rides"] = pyber_summary_df["Total Rides"].map("{:.0f}".format)
pyber_summary_df["Total Drivers"] = pyber_summary_df["Total Drivers"].map("{:.0f}".format)
pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map('${:,.2f}'.format)
pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map('${:,.2f}'.format)
pyber_summary_df["Average Fare per Driver"] = pyber_summary_df["Average Fare per Driver"].map('${:,.2f}'.format)
```
## Deliverable 1: Results
![ PyBer summary DataFrame](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/pyber_sum.png)
 
##### Urban Cities: 
Urban cities have the most drivers, rides, and make the most total fares. They have the lowest average fares per ride and driver. 
I would have liked to see some data on duration of the ride. Cities are more densely populated so places are closer together, so I suspect the mileage and fare per ride to be lower. They also probably have to drive less between fares than suburban or rural drivers. **However, if the driver is stuck in traffic the mileage might not be an accurate measure of value for the driver’s time.** I think this could also be an issue in suburban cities too.
*It would be interesting to see if there are areas in each city where rides are originating.* Then do some research on why this might be. Maybe there are a lot of rides originating in high income neighborhoods, but few in lower income ones. To offset this, you could increase the fare rate in higher income neighborhoods and lower them in more impoverished ones to make them more accessible. Although drivers might have less incentive to service lower income neighborhoods for a lower fare rate, so this could also lead to issues.
##### Suburban Cities:
Suburban cities are in the middle for all data metrics. This makes sense they are less densely populated than cities, but more so than rural ones. They tend to be more car dependent than urban areas, but there is usually more access to public transport than rural areas. This is probably partially because things are more spread out.
Looking into the availability of public transport in suburban cities would help to determine how to improve accessibility. Look for times of the day or days of the week when the public transportation system isn’t operating or is inadequate for supply and demand. You could advertise more heavily during these times. If you are looking for more money you could raise prices during these times. **If your goal is accessibility, you could lower the price during times when the public transportation system is inadequate and increase the prices when public transport is available.** I think you would have less issues with drivers. The drivers who drive full time will make a higher rate during the day. Assuming the public transportation system has less availability in the evening and weekends there will most likely be more ride requests during these times and your drivers who work elsewhere will have more availability during these times. You would want some further data to back that up.
##### Rural Cities: 
Rural cities have the least number of drivers (2.6%), rides(5.3%), and fares(6.8%). Rural areas tend to be more car dependent and by definition are less densely populated.  The U.S. Economic Research Services states the following:

> Overall, 92.7 percent of rural households had access to a car in 2000, compared with 88.9 percent of urban households. 

An additional data point that would be beneficial to examine is the average trip length. The average fare per ride in rural cities was the highest at $34.62 which is 29% higher than the average urban fare. This is most likely because the average mileage of the trip is probably longer as destinations in rural areas tend to be further apart. With the high average fare per ride and low driver count it’s not surprising that the average fare per driver is highest for rural cities at $55.49. The longer trips take more time, require more fuel, and put more wear and tear on the drivers vehicle.

**Decreasing the average fare cost in rural cities should be a priority for increasing affordability and accessibility, because they have higher poverty rates.** The US census data from 2016 states:

> According to the U.S. Census Bureau, the 2016 official poverty rate in rural areas was almost 16 percent compared to just over 12 percent in urban areas. 

More then 1,000,000 Americans in rural households do not have access to a car. Compared to urban residents, rural residents have much less access to public transportation. Only 60% of rural counties have a public transportation system and 28% of them only service one city. Most users of rural public transport systems are female, elderly, and/or disabled. Although, the majority of carless households are in urban areas, the majority of the counties with over 10% carelessness are in rural counties where Black, Hispanic, or Native American residents make up a higher percentage of the population. 

I think it is important for PyBer to look into grants or another means to lower the cost of rides in rural communities to make transportation more accessible to the women, people of color, the disabled, elderly, and impoverished who are all disproportionately affected by the realities of carelessness in rural America.
![ Three Pie Charts of % by type fares, rides, and drivers.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/analysis/picharts.png)
*These pie charts were created in the PyBer_ride_data notebook during the module.
## Deliverable 2: Method
1.  First I examined the data from the merged data frame of the city_data and ride_data. Then I used a groupby of the date and time series from the merged data frame to create a new DataFrame to show the sum of the fares.
```
pyber_data_df.head()
df = pyber_data_df.groupby(["date","type"]).sum()["fare"]
```
![ Goupby date and type data frame.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/groupby_df.png)

2.  I reset the index and created a pivot table where the index was the date, the columns was the city type, and the values were the fare.
```
df = df.reset_index()
df = pyber_data_df.pivot(index = 'date', columns = 'type', values = 'fare')
```
![ Pivot table DataFrame.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/pivot_df.png)

3. I used the loc method on the DataFrame with to get rows from 2019-01-01 through 2019-04-28 into a DataFrame.
```
df2 = df.loc["2019-01-01":"2019-04-28"] 
```
![ loc table DataFrame.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/loc_df.png)

4. I set the date index to a datetime datatype and then used the .index function to verify it worked. I was then able to create a new DataFrame using the resample function to get the sum of fares by week.
```
df2.index = pd.to_datetime(df2.index)
df2.info()
df3 = df2.resample('W').sum()
```
![ Resampled DataFrame.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/df_resampled.png)

5. I was then able to create a chart to show the fares by type from January to April. 
- First I used the MATPLOT method, because I was more familiar with it.
```
df.plot(figsize = (20,5))
# Import the style from Matplotlib.
from matplotlib import style
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')
plt.title("Total Fare By City Type")
plt.ylabel("Fare ($USD)")
plt.xlabel("Months")
#save chart
plt.savefig("resources/PyBer_fare_summary.png",dpi= 300, bbox_inches='tight')
plt.show()
```
![ matplot chart.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/PyBer_fare_summary.png)

- Then I used the object oriented method, but the dates were formatted as weeks so I found some code to convert the x axis labels to months

```
fig, ax = plt.subplots()
ax.plot(df)
# Import the style from Matplotlib.
from matplotlib import style
from matplotlib import dates
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')
ax.set_title("Total Fare By City Type")
ax.set_ylabel("Fare ($USD)")
ax.set_xlabel("Months")
ax.legend()
#save chart
plt.savefig("resources/PyBer_fare_summary5.png",dpi= 300, bbox_inches='tight')
plt.show()
```
![ matplot chart.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/chart_formatting.png)
```
from matplotlib import dates
#code to format dates as months: https://stackoverflow.com/questions/67582913/plotting-time-series-in-matplotlib-with-month-names-ex-january-and-showing-ye
ax.xaxis.set_major_locator(dates.MonthLocator(bymonthday=15))
ax.xaxis.set_major_formatter(dates.DateFormatter('%b'))
```
- Then the legend disappeared so I added a variable to add it back in.
```
legend = df.columns
ax.legend(legend, title="type")
```
-  To wrap things up I changed the figure size and changed the legend position. With the loc set to zero it was on the upper left of the chart, but I prefer the upper right so I set it to 7. Setting loc to 10 would put it in center like on the reference image.
```
fig, ax = plt.subplots()
ax.plot(df)
legend = df.columns
# Import the style from Matplotlib.
from matplotlib import style
from matplotlib import dates
#code to format dates as months: https://stackoverflow.com/questions/67582913/plotting-time-series-in-matplotlib-with-month-names-ex-january-and-showing-ye
# Use the graph style fivethirtyeight.
ax.xaxis.set_major_locator(dates.MonthLocator(bymonthday=15))
ax.xaxis.set_major_formatter(dates.DateFormatter('%b'))
style.use('fivethirtyeight')
ax.set_title("Total Fare By City Type")
ax.set_ylabel("Fare ($USD)")
ax.set_xlabel("Months")
#setting loc to 10 would put it in center & 0 would be best, but I prefer 7 top right.
ax.legend(legend, title="type", loc=10)
fig.set_size_inches(15,5)
#save chart
plt.savefig("resources/PyBer_fare_summary3.png",dpi= 300, bbox_inches='tight')
plt.show()
```
![ matplot chart.](https://github.com/MichelaZ/PyBer_Analysis/blob/main/Resources/PyBer_fare_summary3.png)

## Deliverable 2: Results
I don’t know if there is enough data to make this chart particularly useful I don’t know if the there is enough data over a long enough period of time. It does look like there might be a slight increase in in rides for urban cities, but not in rural or suburban ones. So maybe the number of available drivers is limiting the ability for growth in these regions. I would do some analysis on the drivers per capita. I would also increase advertising in rural and suburban cities both for recruitment and customers.

## Recommendation Summary: 
- Do further research into population, demographics (of cities, drivers, and riders), public transportation systems, length of rides in minutes, and average mileage per ride.
- Increase the fares in urban cities so that drivers make a more competitive wage without decreasing the affordability and accessibility to underserved neighborhoods.
-  In suburban cities lower the price during times when the public transportation system is inadequate and increase the prices when public transport is available.
- Look into grants to provide rides to people in rural areas.
- Increase advertising in rural and suburban areas.

##### References:
“Agriculture Information Bulletin Number 795 Rural Transportation At A Glance Rural Transportation At A Glance January 2005”, *United States Department of Agriculture: Economic Research Service*, Jan. 2005, https://www.ers.usda.gov/webdocs/publications/42593/30151_aib795full_002.pdf?v=41262. Accessed Apr. 28 2022.

J. L. Semega, K. R. Fontenot, and M. A. Kollar, “Income and poverty in the United States: 2016,” Current Population Reports. Washington, D.C.: *U.S. Census Bureau*, 2017.

R. Bellis, “More than one million households without a car in rural America need better transit,” *Smart Growth America*, May 14, 2020,  https://smartgrowthamerica.org/more-than-one-million-households-without-a-car-in-rural-america-need-better-transit/. Accessed 28 Apr. 2022.

