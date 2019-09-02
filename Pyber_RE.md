

```python
%matplotlib inline
```


```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```


```python
#Files to Load
city_data = "data/city_data.csv"
ride_data = "data/ride_data.csv"

#Read the City and Ride Data
city_df = pd.read_csv(city_data)
ride_df = pd.read_csv(ride_data)

#Combine the data into a single dataset & display the data table for preview
pyber_df = pd.merge (ride_df, city_df, on="city", how="left")
display(pyber_df.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2018-01-14 10:14:22</td>
      <td>13.83</td>
      <td>5739410935873</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Michelleport</td>
      <td>2018-03-04 18:24:09</td>
      <td>30.24</td>
      <td>2343912425577</td>
      <td>72</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Samanthamouth</td>
      <td>2018-02-24 04:29:00</td>
      <td>33.44</td>
      <td>2005065760003</td>
      <td>57</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>2018-02-10 23:22:03</td>
      <td>23.44</td>
      <td>5149245426178</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South Jack</td>
      <td>2018-03-06 04:28:35</td>
      <td>34.58</td>
      <td>3908451377344</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>



```python
pyber_df.columns
```




    Index(['city', 'date', 'fare', 'ride_id', 'driver_count', 'type'], dtype='object')




```python
#BUBBLE PLOT (Pt. 1)
#Calculate info and create new dataframe to pull x & y variables
city_var = pyber_df.groupby(['city'])
avg_fare = city_var['fare'].mean()
tot_rides = city_var['ride_id'].count()
drivers = city_var['driver_count'].mean()
types = city_df.set_index('city')['type']
variables_df = pd.DataFrame({"Average Fare": avg_fare,
                            "Total Rides": tot_rides,
                            "Drivers": drivers,
                            "City Types": types})
display(variables_df.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Fare</th>
      <th>Total Rides</th>
      <th>Drivers</th>
      <th>City Types</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Amandaburgh</th>
      <td>24.641667</td>
      <td>18</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Barajasview</th>
      <td>25.332273</td>
      <td>22</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Barronchester</th>
      <td>36.422500</td>
      <td>16</td>
      <td>11</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Bethanyland</th>
      <td>32.956111</td>
      <td>18</td>
      <td>22</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Bradshawfurt</th>
      <td>40.064000</td>
      <td>10</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
</div>



```python
variables_df.columns
```




    Index(['Average Fare', 'Total Rides', 'Drivers', 'City Types'], dtype='object')




```python
#BUBBLE PLOT (Pt. 2)
# Obtain the x and y coordinates for each of the three city types

#Set types as variables to create x & y coordinates
urban = variables_df[variables_df['City Types'] == 'Urban']
suburban = variables_df[variables_df['City Types'] == 'Suburban']
rural = variables_df[variables_df['City Types'] == 'Rural']

# Build the scatter plots for each city types
#Rural
plt.scatter(rural['Total Rides'], rural['Average Fare'], color="Gold", s=variables_df['Drivers'], marker="o", alpha=0.40, label="Rural", edgecolors="Black", linewidths=0.75)
#Suburban
plt.scatter(suburban['Total Rides'], suburban['Average Fare'], color="Skyblue", s=variables_df['Drivers']*5, marker="o", alpha=0.50, label="Suburban", edgecolors="Black", linewidths=0.75)
#Urban
plt.scatter(urban['Total Rides'], urban['Average Fare'], color="Coral", s=variables_df['Drivers']*10, marker="o", alpha=0.60, label="Urban", edgecolors="Black", linewidths=0.75)


# Incorporate the other graph properties
plt.title('Pyber Ride Sharing Data (2016)')
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')
plt.grid()
plt.text(42, 30, "Note:")
plt.text(42, 28, "Circle size correlates with driver count per city")

# Create a legend
legend = plt.legend(title = "City Types", frameon=True, edgecolor='black')
legend.legendHandles[0]._sizes = [60]
legend.legendHandles[1]._sizes = [60]
legend.legendHandles[2]._sizes = [60]

# Save Figure
plt.savefig("../Pyber/CityPlot.png", bbox_inches='tight')
```


![png](output_6_0.png)



```python
#PIE CHARTS
```


```python
# % OF TOTAL FARES BY CITY TYPE

# Calculate Type Percents
city_fare = pyber_df[['type', 'fare']]
py_fare = city_fare.groupby('type')
total_fare = py_fare['fare'].sum()

# Build Pie Chart
labels = ["Rural", "Suburban", "Urban"]
sizes = total_fare
colors = ['Gold', 'Skyblue', 'Coral']
explode = (0, 0, 0.2)

plt.title("% of Total Fares by City Type")
plt.axis("equal")
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=100)

# Save Figure
plt.savefig("../Pyber/FarePlot.png")
```


![png](output_8_0.png)



```python
# % OF TOTAL RIDES BY CITY TYPE

# Calculate Type Percents
city_rides = pyber_df[['type', 'ride_id']]
py_rides = city_rides.groupby('type')
total_rides = py_rides['ride_id'].count()

# Build Pie Chart
labels = ["Rural", "Suburban", "Urban"]
sizes = total_rides
colors = ['Gold', 'Skyblue', 'Coral']
explode = (0.1, 0.1, 0.2)

plt.title("% of Total Rides by City Type")
plt.axis("equal")
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=150)

# Save Figure
plt.savefig("../Pyber/RidePlot.png")
```


![png](output_9_0.png)



```python
# % OF TOTAL DRIVERS BY CITY TYPE

# Calculate Type Percents
city_drivers = pyber_df[['type', 'driver_count']]
py_drivers = city_drivers.groupby('type')
total_drivers = py_drivers['driver_count'].sum()

# Build Pie Chart
labels = ["Rural", "Suburban", "Urban"]
sizes = total_drivers
colors = ['Gold', 'Skyblue', 'Coral']
explode = (0.2, 0.2, 0.3)

plt.title("% of Total Drivers by City Type")
plt.axis("equal")
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=190)

# Save Figure
plt.savefig("../Pyber/DriverPlot.png")
```


![png](output_10_0.png)



```python

```
