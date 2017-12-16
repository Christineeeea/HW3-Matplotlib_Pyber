# HW3-Matplotlib_Pyber




```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
```


```python
#load csv files
```


```python
ride = pd.read_csv('Resources/ride_data.csv')
ride.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
city = pd.read_csv('Resources/city_data.csv')
city.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#merge two csv files
```


```python
pyber_df = pd.merge(ride, city, how='outer', on='city', sort=True)
pyber_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>Alvarezhaven</td>
      <td>2016-04-18 20:51:29</td>
      <td>31.93</td>
      <td>4267015736324</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alvarezhaven</td>
      <td>2016-08-01 00:39:48</td>
      <td>6.42</td>
      <td>8394540350728</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alvarezhaven</td>
      <td>2016-09-01 22:57:12</td>
      <td>18.09</td>
      <td>1197329964911</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alvarezhaven</td>
      <td>2016-08-18 07:12:06</td>
      <td>20.74</td>
      <td>357421158941</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alvarezhaven</td>
      <td>2016-04-04 23:45:50</td>
      <td>14.25</td>
      <td>6431434271355</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# * Average Fare ($) Per City
# * Total Number of Rides Per City
# * Total Number of Drivers Per City
# * City Type (Urban, Suburban, Rural)
```


```python
#separate the df by city types
urban_df=pyber_df.loc[pyber_df['type'] == 'Urban']
suburban_df=pyber_df.loc[pyber_df['type'] == 'Suburban']
rural_df=pyber_df.loc[pyber_df['type'] == 'Rural']
```


```python
#sum,count, and mean for each city type
urban_city_sum=urban_df.groupby('city').sum()
urban_city_ct=urban_df.groupby('city').count()
urban_city_avg=urban_df.groupby('city').mean()
```


```python
#mean=avg fare, ct=total rides, sum/ct=driver counts
urban_x_axis=list(urban_city_ct['ride_id'])
urban_y_axis=list(urban_city_avg['fare'])
urban_s=list((urban_city_sum/urban_city_ct)['driver_count'])
```


```python
sub_city_sum=suburban_df.groupby('city').sum()
sub_city_ct=suburban_df.groupby('city').count()
sub_city_avg=suburban_df.groupby('city').mean()
sub_x_axis=list(sub_city_ct['ride_id'])
sub_y_axis=list(sub_city_avg['fare'])
sub_s=list((sub_city_sum/sub_city_ct)['driver_count'])
```


```python
rural_city_sum=rural_df.groupby('city').sum()
rural_city_ct=rural_df.groupby('city').count()
rural_city_avg=rural_df.groupby('city').mean()
rural_x_axis=list(rural_city_ct['ride_id'])
rural_y_axis=list(rural_city_avg['fare'])
rural_s=list((rural_city_sum/rural_city_ct)['driver_count'])
```


```python
plt.figure(figsize=(8,6))
sns.set(color_codes=True)

#urban plot
plt.scatter(urban_x_axis, urban_y_axis, marker='o', facecolors='lightcoral', edgecolors='black', s=[x*4 for x in urban_s], alpha=0.5, label='Urban', linewidth=2.0)
#suburban plot
plt.scatter(sub_x_axis, sub_y_axis, marker='o', facecolors='lightskyblue', edgecolors='black', s=[x*4 for x in sub_s], alpha=0.5, label='Suburban', linewidth=2.0)
#rural plot
plt.scatter(rural_x_axis, rural_y_axis, marker='o', facecolors='gold', edgecolors='black', s=[x*4 for x in rural_s], alpha=0.5, label='Rural', linewidth=2.0)

#set xlim and ylim
plt.xlim(0, 40)
plt.ylim(15,45)

plt.title('Pyber Ride Sharing Data (2016)', fontsize=16)
plt.xlabel('Total Number of Rides (Per City)', fontsize=14)
plt.ylabel('Average Fare ($)', fontsize=14)


plt.annotate('Note:\nCircle size correlates with driver count per city',
            xy=(1, 0.5),  xytext=(5, 10), xycoords=('axes fraction', 'figure fraction'),
            textcoords='offset points', size=14)
lgnd = plt.legend(fontsize=12, markerscale=1, frameon=False, title='City Types')
plt.setp(lgnd.get_title(),fontsize=14)
lgnd.legendHandles[0]._sizes = [100]
lgnd.legendHandles[1]._sizes = [100]
lgnd.legendHandles[2]._sizes = [100]

plt.grid(True)
plt.show()
```


![png](output_12_0.png)



```python
# In addition, you will be expected to produce the following three pie charts:

# % of Total Fares by City Type
# % of Total Rides by City Type
# % of Total Drivers by City Type
```


```python
total_fare=pyber_df.sum()['fare']
```


```python
urban_fare = urban_df.groupby('type').sum()['fare']
urban_fare_percent=urban_fare/total_fare
urban_fare_percent
```




    type
    Urban    0.619745
    Name: fare, dtype: float64




```python
suburban_fare = suburban_df.groupby('type').sum()['fare']
suburban_fare_percent=suburban_fare/total_fare
suburban_fare_percent
```




    type
    Suburban    0.314458
    Name: fare, dtype: float64




```python
rural_fare = rural_df.groupby('type').sum()['fare']
rural_fare_percent=rural_fare/total_fare
rural_fare_percent
```




    type
    Rural    0.065798
    Name: fare, dtype: float64




```python
plt.figure(figsize=(7,7))
faresize=list(zip(urban_fare_percent, suburban_fare_percent, rural_fare_percent))
labels = ['Urban', 'Suburban', 'Rural']
colors = ['lightcoral', 'lightskyblue', 'gold']
explode = (0.1,0,0)

plt.pie(faresize[0], explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=230)
plt.title('% of Total Fares by City Type',fontsize=14)
plt.axis('equal')
plt.show()
```


![png](output_18_0.png)



```python
total_rides=len(pyber_df)
urban_rides=urban_df.groupby('type').count()['ride_id']
```


```python
suburban_rides=suburban_df.groupby('type').count()['ride_id']
suburban_rides
```




    type
    Suburban    657
    Name: ride_id, dtype: int64




```python
rural_rides=rural_df.groupby('type').count()['ride_id']
rural_rides
```




    type
    Rural    125
    Name: ride_id, dtype: int64




```python
plt.figure(figsize=(7,7))
ridesize=list(zip(urban_rides, suburban_rides, rural_rides))
labels = ['Urban', 'Suburban', 'Rural']
colors = ['lightcoral', 'lightskyblue', 'gold']
explode = (0.1,0,0)

plt.pie(ridesize[0], explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=230)
plt.title('% of Total Rides by City Type',fontsize=14)
plt.axis('equal')
plt.show()
```


![png](output_22_0.png)



```python
driver_df = pyber_df.drop_duplicates(['city'],keep='first')
driver_type=driver_df.groupby('type').sum()
driver_type
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>575.86</td>
      <td>80908776277624</td>
      <td>104</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>1184.07</td>
      <td>170431128985342</td>
      <td>635</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1567.85</td>
      <td>319081797791091</td>
      <td>2607</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(7,7))
driversize=list(driver_type['driver_count'])
labels = ['Rural', 'Suburban', 'Urban']
colors = ['gold', 'lightskyblue', 'lightcoral']
explode = (0,0,0.1)

plt.pie(driversize, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)
plt.title('% of Total Drivers by City Type',fontsize=14)
plt.axis('equal')
plt.show()
```


![png](output_24_0.png)

