### Case study: Cyclistic bike-share analysis for year 2023

#### Abount Company

Cyclistic, a bike-share program launched in 2016, has grown to include 5,824 bikes and 692 stations in Chicago. The bikes can be unlocked and returned at any station. The company offers flexible pricing plans: single-ride passes, full-day passes, and annual memberships. Single-ride and full-day pass users are casual riders, while annual membership buyers are Cyclistic members.

Financial analysis shows that annual members are more profitable than casual riders. The current goal is to convert casual riders into annual members. Moreno, a team leader, believes this conversion is crucial for growth and aims to design marketing strategies to achieve it. To do so, the team needs to understand the differences between annual members and casual riders, motivations for casual riders to buy memberships, and the role of digital media in marketing. Analyzing historical bike trip data is key to identifying trends and informing these strategies.

#### Questions for Analysis

1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?


#### Prepare

For the analysis, I utilized [Cyclistic’s Historical Trip Data](https://divvy-tripdata.s3.amazonaws.com/index.html) to identify trends. The data covers a period from January 1, 2023, to December 30, 2023, and is stored in CSV files, with each file representing one month's data, totaling 12 CSV files. The data is well-organized and structured.

Although the datasets have different names since Cyclistic is a fictional company, they are suitable for this case study. The data is provided by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement). Given that this data is from an actual bike-sharing company in Chicago, it is reliable, original, current, and properly cited (meeting the ROCCC criteria). However, it is not entirely comprehensive as it lacks certain information.

In terms of data integrity, it is accurate, consistent, and trustworthy.

### Peparing data for analysis

Importing libraries


```python
import pandas as pd 
import matplotlib.pyplot as plt 
import numpy as np 
import glob 

import warnings
warnings.filterwarnings('ignore')
```


```python
plt.style.use(['dark_background']) #style
```


```python
%matplotlib inline
```

### Importing data


```python
df1=pd.read_csv('202301-divvy-tripdata.csv') #importing data from CSV files
df2=pd.read_csv('202302-divvy-tripdata.csv')
df3=pd.read_csv('202303-divvy-tripdata.csv')
df4=pd.read_csv('202304-divvy-tripdata.csv')
df5=pd.read_csv('202305-divvy-tripdata.csv')
df6=pd.read_csv('202306-divvy-tripdata.csv')
df7=pd.read_csv('202307-divvy-tripdata.csv')
df8=pd.read_csv('202308-divvy-tripdata.csv')
df9=pd.read_csv('202309-divvy-tripdata.csv')
df10=pd.read_csv('202310-divvy-tripdata.csv')
df11=pd.read_csv('202311-divvy-tripdata.csv')
df12=pd.read_csv('202312-divvy-tripdata.csv')
```


```python
data = pd.concat( [df1,df2,df3,df4,df5,df6,df7,df8,df9,df10,df11,df12], ignore_index = True) #merging all dataframes
```


```python
data.shape
```




    (5719877, 13)




```python
data.head(10)
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
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>start_station_name</th>
      <th>start_station_id</th>
      <th>end_station_name</th>
      <th>end_station_id</th>
      <th>start_lat</th>
      <th>start_lng</th>
      <th>end_lat</th>
      <th>end_lng</th>
      <th>member_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F96D5A74A3E41399</td>
      <td>electric_bike</td>
      <td>2023-01-21 20:05:42</td>
      <td>2023-01-21 20:16:33</td>
      <td>Lincoln Ave &amp; Fullerton Ave</td>
      <td>TA1309000058</td>
      <td>Hampden Ct &amp; Diversey Ave</td>
      <td>202480.0</td>
      <td>41.924074</td>
      <td>-87.646278</td>
      <td>41.930000</td>
      <td>-87.640000</td>
      <td>member</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13CB7EB698CEDB88</td>
      <td>classic_bike</td>
      <td>2023-01-10 15:37:36</td>
      <td>2023-01-10 15:46:05</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799568</td>
      <td>-87.594747</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BD88A2E670661CE5</td>
      <td>electric_bike</td>
      <td>2023-01-02 07:51:57</td>
      <td>2023-01-02 08:05:11</td>
      <td>Western Ave &amp; Lunt Ave</td>
      <td>RP-005</td>
      <td>Valli Produce - Evanston Plaza</td>
      <td>599</td>
      <td>42.008571</td>
      <td>-87.690483</td>
      <td>42.039742</td>
      <td>-87.699413</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C90792D034FED968</td>
      <td>classic_bike</td>
      <td>2023-01-22 10:52:58</td>
      <td>2023-01-22 11:01:44</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799568</td>
      <td>-87.594747</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3397017529188E8A</td>
      <td>classic_bike</td>
      <td>2023-01-12 13:58:01</td>
      <td>2023-01-12 14:13:20</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799568</td>
      <td>-87.594747</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
    <tr>
      <th>5</th>
      <td>58E68156DAE3E311</td>
      <td>electric_bike</td>
      <td>2023-01-31 07:18:03</td>
      <td>2023-01-31 07:21:16</td>
      <td>Lakeview Ave &amp; Fullerton Pkwy</td>
      <td>TA1309000019</td>
      <td>Hampden Ct &amp; Diversey Ave</td>
      <td>202480.0</td>
      <td>41.926069</td>
      <td>-87.638858</td>
      <td>41.930000</td>
      <td>-87.640000</td>
      <td>member</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2F7194B6012A98D4</td>
      <td>electric_bike</td>
      <td>2023-01-15 21:18:36</td>
      <td>2023-01-15 21:32:36</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799554</td>
      <td>-87.594617</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
    <tr>
      <th>7</th>
      <td>DB1CF84154D6A049</td>
      <td>classic_bike</td>
      <td>2023-01-25 10:49:01</td>
      <td>2023-01-25 10:58:22</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799568</td>
      <td>-87.594747</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
    <tr>
      <th>8</th>
      <td>34EAB943F88C4C5D</td>
      <td>electric_bike</td>
      <td>2023-01-25 20:49:47</td>
      <td>2023-01-25 21:02:14</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799587</td>
      <td>-87.594670</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
    <tr>
      <th>9</th>
      <td>BC8AB1AA51DA9115</td>
      <td>classic_bike</td>
      <td>2023-01-06 16:37:19</td>
      <td>2023-01-06 16:49:52</td>
      <td>Kimbark Ave &amp; 53rd St</td>
      <td>TA1309000037</td>
      <td>Greenwood Ave &amp; 47th St</td>
      <td>TA1308000002</td>
      <td>41.799568</td>
      <td>-87.594747</td>
      <td>41.809835</td>
      <td>-87.599383</td>
      <td>member</td>
    </tr>
  </tbody>
</table>
</div>



### Data Cleaning

Removing data that is not needed for this analysis to improve performance


```python
data = data.drop(columns=['start_station_name', 'start_station_id', 'end_station_name', 'end_station_id', 'start_lat', 'start_lng', 'end_lat', 'end_lng'])
data
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
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>member_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F96D5A74A3E41399</td>
      <td>electric_bike</td>
      <td>2023-01-21 20:05:42</td>
      <td>2023-01-21 20:16:33</td>
      <td>member</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13CB7EB698CEDB88</td>
      <td>classic_bike</td>
      <td>2023-01-10 15:37:36</td>
      <td>2023-01-10 15:46:05</td>
      <td>member</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BD88A2E670661CE5</td>
      <td>electric_bike</td>
      <td>2023-01-02 07:51:57</td>
      <td>2023-01-02 08:05:11</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C90792D034FED968</td>
      <td>classic_bike</td>
      <td>2023-01-22 10:52:58</td>
      <td>2023-01-22 11:01:44</td>
      <td>member</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3397017529188E8A</td>
      <td>classic_bike</td>
      <td>2023-01-12 13:58:01</td>
      <td>2023-01-12 14:13:20</td>
      <td>member</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5719872</th>
      <td>F74DF9549B504A6B</td>
      <td>electric_bike</td>
      <td>2023-12-07 13:15:24</td>
      <td>2023-12-07 13:17:37</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>5719873</th>
      <td>BCDA66E761CC1029</td>
      <td>classic_bike</td>
      <td>2023-12-08 18:42:21</td>
      <td>2023-12-08 18:45:56</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>5719874</th>
      <td>D2CF330F9C266683</td>
      <td>classic_bike</td>
      <td>2023-12-05 14:09:11</td>
      <td>2023-12-05 14:13:01</td>
      <td>member</td>
    </tr>
    <tr>
      <th>5719875</th>
      <td>3829A0D1E00EE970</td>
      <td>electric_bike</td>
      <td>2023-12-02 21:36:07</td>
      <td>2023-12-02 21:53:45</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>5719876</th>
      <td>A373F5B447AEA508</td>
      <td>classic_bike</td>
      <td>2023-12-11 13:07:46</td>
      <td>2023-12-11 13:11:24</td>
      <td>member</td>
    </tr>
  </tbody>
</table>
<p>5719877 rows × 5 columns</p>
</div>



Checking for duplicates


```python
print(data.duplicated().sum())
```

    0
    


```python
data.isnull().sum()
```




    ride_id          0
    rideable_type    0
    started_at       0
    ended_at         0
    member_casual    0
    dtype: int64



#### Manipulating data
Splitting date and time to separate columns and fixing datatypes


```python
data[['st_date','st_time']]=data.started_at.str.split(expand=True)
```


```python
data[['end_date','end_time']]=data.ended_at.str.split(expand=True)
```


```python
data["started_at"]=pd.to_datetime(data["started_at"])
data['ended_at']=pd.to_datetime(data['ended_at'])
```


```python
data['st_date']=pd.to_datetime(data['st_date'])
data['end_date']=pd.to_datetime(data['end_date'])
data['end_time']=pd.to_datetime(data['end_time'])
data['st_time']=pd.to_datetime(data['st_time'])
```


```python
data['day_of_week'] = data['started_at'].dt.dayofweek
```


```python
data['hour']=data.st_time.dt.hour
```


```python
data['ord_day']=data.started_at.dt.day_of_year
```


```python
data['name_day']=data.st_date.dt.day_name()
```


```python
data['name_day']=data.st_date.dt.day_name()
```


```python
data['name_month']=data.started_at.dt.month_name()
```


```python
data['num_month']=data.started_at.dt.month
```


```python
data['total_ride_length']=data['ended_at']-data['started_at']
```


```python
data['total_ride_length']=pd.to_numeric(data['total_ride_length'])/6e+10
```


```python
data=data.drop(data[data['total_ride_length']<1].index) #removing negative values
```


```python
data[data['total_ride_length']<1].count()
```




    ride_id              0
    rideable_type        0
    started_at           0
    ended_at             0
    member_casual        0
    st_date              0
    st_time              0
    end_date             0
    end_time             0
    day_of_week          0
    hour                 0
    ord_day              0
    name_day             0
    name_month           0
    num_month            0
    total_ride_length    0
    dtype: int64



#### Analysing Data


```python
data #top and bottom 5 rows of the final dataset 
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
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>member_casual</th>
      <th>st_date</th>
      <th>st_time</th>
      <th>end_date</th>
      <th>end_time</th>
      <th>day_of_week</th>
      <th>hour</th>
      <th>ord_day</th>
      <th>name_day</th>
      <th>name_month</th>
      <th>num_month</th>
      <th>total_ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F96D5A74A3E41399</td>
      <td>electric_bike</td>
      <td>2023-01-21 20:05:42</td>
      <td>2023-01-21 20:16:33</td>
      <td>member</td>
      <td>2023-01-21</td>
      <td>2024-07-09 20:05:42</td>
      <td>2023-01-21</td>
      <td>2024-07-09 20:16:33</td>
      <td>5</td>
      <td>20</td>
      <td>21</td>
      <td>Saturday</td>
      <td>January</td>
      <td>1</td>
      <td>10.850000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13CB7EB698CEDB88</td>
      <td>classic_bike</td>
      <td>2023-01-10 15:37:36</td>
      <td>2023-01-10 15:46:05</td>
      <td>member</td>
      <td>2023-01-10</td>
      <td>2024-07-09 15:37:36</td>
      <td>2023-01-10</td>
      <td>2024-07-09 15:46:05</td>
      <td>1</td>
      <td>15</td>
      <td>10</td>
      <td>Tuesday</td>
      <td>January</td>
      <td>1</td>
      <td>8.483333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BD88A2E670661CE5</td>
      <td>electric_bike</td>
      <td>2023-01-02 07:51:57</td>
      <td>2023-01-02 08:05:11</td>
      <td>casual</td>
      <td>2023-01-02</td>
      <td>2024-07-09 07:51:57</td>
      <td>2023-01-02</td>
      <td>2024-07-09 08:05:11</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
      <td>Monday</td>
      <td>January</td>
      <td>1</td>
      <td>13.233333</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C90792D034FED968</td>
      <td>classic_bike</td>
      <td>2023-01-22 10:52:58</td>
      <td>2023-01-22 11:01:44</td>
      <td>member</td>
      <td>2023-01-22</td>
      <td>2024-07-09 10:52:58</td>
      <td>2023-01-22</td>
      <td>2024-07-09 11:01:44</td>
      <td>6</td>
      <td>10</td>
      <td>22</td>
      <td>Sunday</td>
      <td>January</td>
      <td>1</td>
      <td>8.766667</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3397017529188E8A</td>
      <td>classic_bike</td>
      <td>2023-01-12 13:58:01</td>
      <td>2023-01-12 14:13:20</td>
      <td>member</td>
      <td>2023-01-12</td>
      <td>2024-07-09 13:58:01</td>
      <td>2023-01-12</td>
      <td>2024-07-09 14:13:20</td>
      <td>3</td>
      <td>13</td>
      <td>12</td>
      <td>Thursday</td>
      <td>January</td>
      <td>1</td>
      <td>15.316667</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5719872</th>
      <td>F74DF9549B504A6B</td>
      <td>electric_bike</td>
      <td>2023-12-07 13:15:24</td>
      <td>2023-12-07 13:17:37</td>
      <td>casual</td>
      <td>2023-12-07</td>
      <td>2024-07-09 13:15:24</td>
      <td>2023-12-07</td>
      <td>2024-07-09 13:17:37</td>
      <td>3</td>
      <td>13</td>
      <td>341</td>
      <td>Thursday</td>
      <td>December</td>
      <td>12</td>
      <td>2.216667</td>
    </tr>
    <tr>
      <th>5719873</th>
      <td>BCDA66E761CC1029</td>
      <td>classic_bike</td>
      <td>2023-12-08 18:42:21</td>
      <td>2023-12-08 18:45:56</td>
      <td>casual</td>
      <td>2023-12-08</td>
      <td>2024-07-09 18:42:21</td>
      <td>2023-12-08</td>
      <td>2024-07-09 18:45:56</td>
      <td>4</td>
      <td>18</td>
      <td>342</td>
      <td>Friday</td>
      <td>December</td>
      <td>12</td>
      <td>3.583333</td>
    </tr>
    <tr>
      <th>5719874</th>
      <td>D2CF330F9C266683</td>
      <td>classic_bike</td>
      <td>2023-12-05 14:09:11</td>
      <td>2023-12-05 14:13:01</td>
      <td>member</td>
      <td>2023-12-05</td>
      <td>2024-07-09 14:09:11</td>
      <td>2023-12-05</td>
      <td>2024-07-09 14:13:01</td>
      <td>1</td>
      <td>14</td>
      <td>339</td>
      <td>Tuesday</td>
      <td>December</td>
      <td>12</td>
      <td>3.833333</td>
    </tr>
    <tr>
      <th>5719875</th>
      <td>3829A0D1E00EE970</td>
      <td>electric_bike</td>
      <td>2023-12-02 21:36:07</td>
      <td>2023-12-02 21:53:45</td>
      <td>casual</td>
      <td>2023-12-02</td>
      <td>2024-07-09 21:36:07</td>
      <td>2023-12-02</td>
      <td>2024-07-09 21:53:45</td>
      <td>5</td>
      <td>21</td>
      <td>336</td>
      <td>Saturday</td>
      <td>December</td>
      <td>12</td>
      <td>17.633333</td>
    </tr>
    <tr>
      <th>5719876</th>
      <td>A373F5B447AEA508</td>
      <td>classic_bike</td>
      <td>2023-12-11 13:07:46</td>
      <td>2023-12-11 13:11:24</td>
      <td>member</td>
      <td>2023-12-11</td>
      <td>2024-07-09 13:07:46</td>
      <td>2023-12-11</td>
      <td>2024-07-09 13:11:24</td>
      <td>0</td>
      <td>13</td>
      <td>345</td>
      <td>Monday</td>
      <td>December</td>
      <td>12</td>
      <td>3.633333</td>
    </tr>
  </tbody>
</table>
<p>5570262 rows × 16 columns</p>
</div>




```python
data.describe()
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
      <th>started_at</th>
      <th>ended_at</th>
      <th>st_date</th>
      <th>st_time</th>
      <th>end_date</th>
      <th>end_time</th>
      <th>day_of_week</th>
      <th>hour</th>
      <th>ord_day</th>
      <th>num_month</th>
      <th>total_ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>5570262</td>
      <td>5570262</td>
      <td>5570262</td>
      <td>5570262</td>
      <td>5570262</td>
      <td>5570262</td>
      <td>5.570262e+06</td>
      <td>5.570262e+06</td>
      <td>5.570262e+06</td>
      <td>5.570262e+06</td>
      <td>5.570262e+06</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2023-07-16 19:10:09.162088448</td>
      <td>2023-07-16 19:28:48.912187648</td>
      <td>2023-07-16 04:34:37.007940352</td>
      <td>2024-07-09 14:35:32.154150400</td>
      <td>2023-07-16 04:43:53.695577344</td>
      <td>2024-07-09 14:44:55.216607744</td>
      <td>3.029594e+00</td>
      <td>1.409146e+01</td>
      <td>1.971907e+02</td>
      <td>7.002842e+00</td>
      <td>1.866250e+01</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2023-01-01 00:02:06</td>
      <td>2023-01-01 00:07:23</td>
      <td>2023-01-01 00:00:00</td>
      <td>2024-07-09 00:00:00</td>
      <td>2023-01-01 00:00:00</td>
      <td>2024-07-09 00:00:00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2023-05-21 17:21:34.500000</td>
      <td>2023-05-21 17:44:51.500000</td>
      <td>2023-05-21 00:00:00</td>
      <td>2024-07-09 11:09:29</td>
      <td>2023-05-21 00:00:00</td>
      <td>2024-07-09 11:20:05</td>
      <td>1.000000e+00</td>
      <td>1.100000e+01</td>
      <td>1.410000e+02</td>
      <td>5.000000e+00</td>
      <td>5.700000e+00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2023-07-21 06:40:50.500000</td>
      <td>2023-07-21 07:01:09</td>
      <td>2023-07-21 00:00:00</td>
      <td>2024-07-09 15:26:46</td>
      <td>2023-07-21 00:00:00</td>
      <td>2024-07-09 15:39:38</td>
      <td>3.000000e+00</td>
      <td>1.500000e+01</td>
      <td>2.020000e+02</td>
      <td>7.000000e+00</td>
      <td>9.800000e+00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2023-09-17 01:36:05.750000128</td>
      <td>2023-09-17 02:00:38</td>
      <td>2023-09-17 00:00:00</td>
      <td>2024-07-09 18:10:48</td>
      <td>2023-09-17 00:00:00</td>
      <td>2024-07-09 18:23:29</td>
      <td>5.000000e+00</td>
      <td>1.800000e+01</td>
      <td>2.600000e+02</td>
      <td>9.000000e+00</td>
      <td>1.723333e+01</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2023-12-31 23:58:55</td>
      <td>2024-01-01 23:50:51</td>
      <td>2023-12-31 00:00:00</td>
      <td>2024-07-09 23:59:59</td>
      <td>2024-01-01 00:00:00</td>
      <td>2024-07-09 23:59:59</td>
      <td>6.000000e+00</td>
      <td>2.300000e+01</td>
      <td>3.650000e+02</td>
      <td>1.200000e+01</td>
      <td>9.848907e+04</td>
    </tr>
    <tr>
      <th>std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.950792e+00</td>
      <td>4.940263e+00</td>
      <td>8.286472e+01</td>
      <td>2.720981e+00</td>
      <td>1.825898e+02</td>
    </tr>
  </tbody>
</table>
</div>




```python
member=data[data.member_casual=='member'] 
```


```python
casual=data[data.member_casual=='casual']

```


```python
d=data.member_casual.value_counts()
d=pd.DataFrame(d)
d.reset_index(drop=False, inplace=True)
d
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
      <th>member_casual</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>member</td>
      <td>3564512</td>
    </tr>
    <tr>
      <th>1</th>
      <td>casual</td>
      <td>2005750</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.pie(d['count'],labels=d['member_casual'],autopct='%1.1f%%',colors=['#6dc2ca', '#597dce','#ccff99'])
plt.title("Member type distribution(Casuals X Members)")
plt.show()
```


    
![png](img/output_41_0.png)
    


- Members: 64%
- Casual: 36%
This chart indicates that the majority of users are members, accounting for 64% of the total, while casual users make up 36%.


```python
m3=member.rideable_type.value_counts()
m3=pd.DataFrame(m3)
m3.reset_index(drop=False, inplace=True)
m3
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
      <th>rideable_type</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>classic_bike</td>
      <td>1790185</td>
    </tr>
    <tr>
      <th>1</th>
      <td>electric_bike</td>
      <td>1774327</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.pie(m3['count'],labels=m3['rideable_type'],autopct='%1.1f%%',colors=['#6dc2ca', '#597dce','#ccff99'])
plt.title("The distribution of rideable type for Member ")
plt.show()
```


    
![png](img/output_44_0.png)
    


- Classic Bike: 50.2%
- Electric Bike: 49.8%

Among members, the usage of classic bikes and electric bikes is almost evenly split, with classic bikes being slightly more popular at 50.2%.


```python
c3=casual.rideable_type.value_counts()
c3=pd.DataFrame(c3)
c3.reset_index(drop=False, inplace=True)
c3
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
      <th>rideable_type</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>electric_bike</td>
      <td>1063621</td>
    </tr>
    <tr>
      <th>1</th>
      <td>classic_bike</td>
      <td>864555</td>
    </tr>
    <tr>
      <th>2</th>
      <td>docked_bike</td>
      <td>77574</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.pie(c3['count'],labels=c3['rideable_type'],autopct='%1.1f%%',colors=['#6dc2ca', '#597dce','#133984'])
plt.title("The distribution of rideable type for Casual ")
plt.show()
```


    
![png](img/output_47_0.png)
    


- Electric Bike: 53%
- Classic Bike: 43.1%
- Docked Bike: 3.9%

Casual users show a preference for electric bikes, which constitute 53% of their rides. Classic bikes account for 43.1%, and docked bikes are the least used at 3.9%.


```python
m1=member.rideable_type.value_counts()
m1=pd.DataFrame(m1)
c1=casual.rideable_type.value_counts()
c1=pd.DataFrame(c1)
```


```python
df = pd.concat([c1, m1] , axis=1)
df.columns = ['Casual', 'Member']
df.plot(kind='bar',color=['#6dc2ca', '#597dce'])
plt.xlabel('Rideable Type')
plt.ylabel('Count')
plt.title('Rideable Type Distribution by User Type')
plt.show()
```


    
![png](img/output_50_0.png)
    


Electric and classic bikes are preferred by both user types, while docked bikes are less popular.


```python
d2=data.name_month.value_counts()
d2=pd.DataFrame(d2)
d2.reset_index(drop=False, inplace=True)
months_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
d2['month_order'] = d2['name_month'].apply(lambda x: months_order.index(x))
d2_sorted = d2.sort_values(by='month_order')
d2_sorted.drop('month_order', axis=1, inplace=True)
d2_sorted
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
      <th>name_month</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11</th>
      <td>January</td>
      <td>184113</td>
    </tr>
    <tr>
      <th>10</th>
      <td>February</td>
      <td>184444</td>
    </tr>
    <tr>
      <th>8</th>
      <td>March</td>
      <td>249601</td>
    </tr>
    <tr>
      <th>6</th>
      <td>April</td>
      <td>411607</td>
    </tr>
    <tr>
      <th>4</th>
      <td>May</td>
      <td>586955</td>
    </tr>
    <tr>
      <th>2</th>
      <td>June</td>
      <td>700835</td>
    </tr>
    <tr>
      <th>1</th>
      <td>July</td>
      <td>747487</td>
    </tr>
    <tr>
      <th>0</th>
      <td>August</td>
      <td>753322</td>
    </tr>
    <tr>
      <th>3</th>
      <td>September</td>
      <td>652054</td>
    </tr>
    <tr>
      <th>5</th>
      <td>October</td>
      <td>525420</td>
    </tr>
    <tr>
      <th>7</th>
      <td>November</td>
      <td>355114</td>
    </tr>
    <tr>
      <th>9</th>
      <td>December</td>
      <td>219310</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(20,11))
plt.plot(d2_sorted['name_month'],d2_sorted['count'],'ro-')
plt.title("total ride count by users per month")
plt.ylabel("count")
plt.xlabel("month")
plt.show()
```


    
![png](img/output_53_0.png)
    


The highest number of bike trips occurred between month of June and August.


```python
m2=member.name_month.value_counts()
m2=pd.DataFrame(m2)
m2.reset_index(drop=False, inplace=True)
m2
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
      <th>name_month</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>August</td>
      <td>449587</td>
    </tr>
    <tr>
      <th>1</th>
      <td>July</td>
      <td>424922</td>
    </tr>
    <tr>
      <th>2</th>
      <td>June</td>
      <td>407789</td>
    </tr>
    <tr>
      <th>3</th>
      <td>September</td>
      <td>396288</td>
    </tr>
    <tr>
      <th>4</th>
      <td>May</td>
      <td>359580</td>
    </tr>
    <tr>
      <th>5</th>
      <td>October</td>
      <td>352461</td>
    </tr>
    <tr>
      <th>6</th>
      <td>April</td>
      <td>269086</td>
    </tr>
    <tr>
      <th>7</th>
      <td>November</td>
      <td>258852</td>
    </tr>
    <tr>
      <th>8</th>
      <td>March</td>
      <td>189322</td>
    </tr>
    <tr>
      <th>9</th>
      <td>December</td>
      <td>168723</td>
    </tr>
    <tr>
      <th>10</th>
      <td>January</td>
      <td>145276</td>
    </tr>
    <tr>
      <th>11</th>
      <td>February</td>
      <td>142626</td>
    </tr>
  </tbody>
</table>
</div>




```python
c2=casual.name_month.value_counts()
c2=pd.DataFrame(c2)
c2.reset_index(drop=False, inplace=True)
c2
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
      <th>name_month</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>July</td>
      <td>322565</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>303735</td>
    </tr>
    <tr>
      <th>2</th>
      <td>June</td>
      <td>293046</td>
    </tr>
    <tr>
      <th>3</th>
      <td>September</td>
      <td>255766</td>
    </tr>
    <tr>
      <th>4</th>
      <td>May</td>
      <td>227375</td>
    </tr>
    <tr>
      <th>5</th>
      <td>October</td>
      <td>172959</td>
    </tr>
    <tr>
      <th>6</th>
      <td>April</td>
      <td>142521</td>
    </tr>
    <tr>
      <th>7</th>
      <td>November</td>
      <td>96262</td>
    </tr>
    <tr>
      <th>8</th>
      <td>March</td>
      <td>60279</td>
    </tr>
    <tr>
      <th>9</th>
      <td>December</td>
      <td>50587</td>
    </tr>
    <tr>
      <th>10</th>
      <td>February</td>
      <td>41818</td>
    </tr>
    <tr>
      <th>11</th>
      <td>January</td>
      <td>38837</td>
    </tr>
  </tbody>
</table>
</div>




```python
mrg1=pd.merge(m2,c2, on='name_month', how='outer', suffixes=('_member', '_casual'))
months_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
mrg1['month_order'] = mrg1['name_month'].apply(lambda x: months_order.index(x))
mrg_sorted1 = mrg1.sort_values(by='month_order')
mrg_sorted1.drop('month_order', axis=1, inplace=True)
mrg_sorted1
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
      <th>name_month</th>
      <th>count_member</th>
      <th>count_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>January</td>
      <td>145276</td>
      <td>38837</td>
    </tr>
    <tr>
      <th>3</th>
      <td>February</td>
      <td>142626</td>
      <td>41818</td>
    </tr>
    <tr>
      <th>7</th>
      <td>March</td>
      <td>189322</td>
      <td>60279</td>
    </tr>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>269086</td>
      <td>142521</td>
    </tr>
    <tr>
      <th>8</th>
      <td>May</td>
      <td>359580</td>
      <td>227375</td>
    </tr>
    <tr>
      <th>6</th>
      <td>June</td>
      <td>407789</td>
      <td>293046</td>
    </tr>
    <tr>
      <th>5</th>
      <td>July</td>
      <td>424922</td>
      <td>322565</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>449587</td>
      <td>303735</td>
    </tr>
    <tr>
      <th>11</th>
      <td>September</td>
      <td>396288</td>
      <td>255766</td>
    </tr>
    <tr>
      <th>10</th>
      <td>October</td>
      <td>352461</td>
      <td>172959</td>
    </tr>
    <tr>
      <th>9</th>
      <td>November</td>
      <td>258852</td>
      <td>96262</td>
    </tr>
    <tr>
      <th>2</th>
      <td>December</td>
      <td>168723</td>
      <td>50587</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(20,11))
mrg_sorted1.plot(kind='bar',color=['#6dc2ca', '#597dce'],ax=ax)
plt.title("Ride count by users per month ")
plt.ylabel("No. of rides")
plt.xlabel("Month") 
plt.xticks(np.arange(12), mrg_sorted1['name_month'], rotation=45)
plt.show()
```


    
![png](img/output_58_0.png)
    


1. Seasonal Trends:

    - Summer Peak: Months from June to August (June, July, August) show consistently high ride counts for both members and casual riders, with June and July having the highest overall counts.
    - Winter Decrease: Months from November to February (November, December, January, February) generally have lower ride counts, especially among casual riders.

2. Member vs. Casual Rider Usage:

    - Member Dominance: Across most months, members tend to have a higher number of rides compared to casual riders, indicating that members are more consistent in their usage throughout the year.
    - Casual Rider Spikes: There are noticeable spikes in casual rider counts during peak summer months (June, July, August), suggesting increased usage by occasional or seasonal users during vacation periods.

3. Annual Trends:

    - Overall Increase: The total number of rides generally increases from the beginning of the year (January) through the summer months, peaking in July.
    - Fall Decrease: Rides tend to decrease starting from September through December, reflecting seasonal trends and potentially cooler weather affecting ridership.

4. Implications for Marketing and Strategy:

    - Targeted Campaigns: Understanding these seasonal variations can help Cyclistic design targeted marketing campaigns to attract more casual riders during peak seasons and encourage membership during slower months.
    - Service Planning: Insights into member usage patterns can inform service planning and resource allocation, such as bike maintenance and station management.


```python
m1=member.name_day.value_counts()
m1=pd.DataFrame(m1)
m1.reset_index(drop=False, inplace=True)
m1
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
      <th>name_day</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Thursday</td>
      <td>573822</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Wednesday</td>
      <td>571585</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tuesday</td>
      <td>561963</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Friday</td>
      <td>517281</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Monday</td>
      <td>481944</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Saturday</td>
      <td>459879</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Sunday</td>
      <td>398038</td>
    </tr>
  </tbody>
</table>
</div>




```python
c1=casual.name_day.value_counts()
c1=pd.DataFrame(c1)
c1.reset_index(drop=False, inplace=True)
c1
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
      <th>name_day</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Saturday</td>
      <td>399905</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sunday</td>
      <td>326829</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Friday</td>
      <td>303857</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Thursday</td>
      <td>263642</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Wednesday</td>
      <td>242761</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Tuesday</td>
      <td>239914</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Monday</td>
      <td>228842</td>
    </tr>
  </tbody>
</table>
</div>




```python
mrg=pd.merge(m1,c1, on='name_day', how='outer', suffixes=('_member', '_casual'))

weekdays_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
mrg['day_order'] = mrg['name_day'].apply(lambda x: weekdays_order.index(x))
mrg_sorted = mrg.sort_values(by='day_order')
mrg_sorted.drop('day_order', axis=1, inplace=True)
mrg_sorted
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
      <th>name_day</th>
      <th>count_member</th>
      <th>count_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Monday</td>
      <td>481944</td>
      <td>228842</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Tuesday</td>
      <td>561963</td>
      <td>239914</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Wednesday</td>
      <td>571585</td>
      <td>242761</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Thursday</td>
      <td>573822</td>
      <td>263642</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Friday</td>
      <td>517281</td>
      <td>303857</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saturday</td>
      <td>459879</td>
      <td>399905</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sunday</td>
      <td>398038</td>
      <td>326829</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(20,11))
mrg_sorted.plot(kind='bar',color=['#6dc2ca', '#597dce'],ax=ax)
plt.title("Average number of ride for each casual and member ")
plt.ylabel("No. of rides")
plt.xlabel("Days") 
plt.xticks(np.arange(7), mrg_sorted['name_day'],rotation=45)
plt.show()
```


    
![png](img/output_63_0.png)
    


1. Weekday vs. Weekend Usage:

    - Weekday Peaks: Thursday and Wednesday have the highest total ride counts, with 573,822 and 571,585 rides respectively. This suggests that members and casual riders alike tend to use Cyclistic bikes more on weekdays.
    - Weekend Decrease: Saturday and Sunday show lower total ride counts compared to weekdays, which aligns with typical commuter patterns where bike-sharing usage may decrease on weekends.

2. Member vs. Casual Rider Trends:

    - Member Dominance: Members consistently outnumber casual riders in ride counts across all days of the week. This indicates that members are more frequent users of the bike-share service, relying on it for regular commuting or daily transportation needs.
    - Casual Rider Spikes: While members generally dominate, there are notable spikes in casual rider counts on weekends (Saturday and Sunday). This suggests that casual riders might be more likely to use the service for recreational or leisure purposes during weekends.

3. Daily Patterns:

    - Midweek Consistency: Ride counts for both members and casual riders are relatively consistent from Monday to Thursday, with slight variations.
    - Friday Variation: Fridays show a higher count of rides compared to weekends, particularly among casual riders, which might indicate increased usage for social or weekend starting activities.

4. Implications for Strategy:

    - Targeted Promotions: Understanding these daily patterns can help Cyclistic tailor promotions and incentives to encourage more frequent usage, especially among casual riders during weekends.
    - Operational Planning: Insights into member usage can inform operational decisions such as bike deployment and station management during peak weekdays.


```python
mh=member.hour.value_counts()
mh=pd.DataFrame(mh)
mh.reset_index(drop=False, inplace=True)
mh['hour'] = mh['hour'] + 1
mh_sorted = mh.sort_values('hour')
mh_sorted.head()
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
      <th>hour</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>18</th>
      <td>1</td>
      <td>34485</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2</td>
      <td>20522</td>
    </tr>
    <tr>
      <th>21</th>
      <td>3</td>
      <td>11906</td>
    </tr>
    <tr>
      <th>23</th>
      <td>4</td>
      <td>7734</td>
    </tr>
    <tr>
      <th>22</th>
      <td>5</td>
      <td>8489</td>
    </tr>
  </tbody>
</table>
</div>




```python
ch=casual.hour.value_counts()
ch=pd.DataFrame(ch)
ch.reset_index(drop=False, inplace=True)
ch['hour'] = ch['hour'] + 1
ch_sorted = ch.sort_values('hour')
ch_sorted.head()
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
      <th>hour</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>17</th>
      <td>1</td>
      <td>35781</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2</td>
      <td>23195</td>
    </tr>
    <tr>
      <th>20</th>
      <td>3</td>
      <td>14018</td>
    </tr>
    <tr>
      <th>22</th>
      <td>4</td>
      <td>7715</td>
    </tr>
    <tr>
      <th>23</th>
      <td>5</td>
      <td>5811</td>
    </tr>
  </tbody>
</table>
</div>




```python
mrg2=pd.merge(mh_sorted,ch_sorted, on='hour', how='outer', suffixes=('_member', '_casual'))
mrg2.head()
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
      <th>hour</th>
      <th>count_member</th>
      <th>count_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>34485</td>
      <td>35781</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20522</td>
      <td>23195</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>11906</td>
      <td>14018</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>7734</td>
      <td>7715</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>8489</td>
      <td>5811</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(20,11))
mrg2.plot(kind='bar',color=['#6dc2ca', '#597dce'],ax=ax)
plt.title("Ride count by users per month ")
plt.ylabel("No. of rides")
plt.xlabel("Hour") 
plt.xticks(np.arange(24), mrg2['hour'], rotation=0) 
plt.show()
```


    
![png](img/output_68_0.png)
    


1. Peak Hours:

    - Evening Peak: The highest counts of rides occur during late afternoon to early evening hours, particularly between 4 PM and 6 PM (hour 16 to 18), for both members and casual riders. This suggests that many users utilize the bikes for commuting home from work or school.
    - Morning Surge: There's also a notable peak in the morning, particularly between 7 AM and 9 AM (hour 7 to 9), indicating morning commute usage primarily by members.

2. Usage Patterns Throughout the Day:

    - Member Dominance: Throughout most hours of the day, members tend to have higher ride counts compared to casual riders. This indicates that members are consistent users of the bike-sharing service for daily commuting or regular transportation needs.
    - Casual Rider Patterns: Casual riders show more fluctuation in their ride counts throughout the day, with peaks during late afternoon to evening hours, likely reflecting recreational or leisure use.

3. Late Night and Early Morning Usage:

    - Late Night Decline: Ride counts decrease significantly during late night and early morning hours (hour 0 to 5), indicating minimal usage during these times.
    - Potential for Service Adjustments: Understanding these patterns can help Cyclistic optimize bike deployment and station operations, ensuring bikes are available where and when they are most needed by riders.

4. Strategic Insights:

    - Targeted Marketing: Based on these usage patterns, Cyclistic could focus marketing efforts on promoting membership benefits for daily commuters during peak hours and offer incentives to casual riders during evening and weekend peaks.
    - Operational Efficiency: Insights into hourly usage can inform operational decisions such as scheduling maintenance, redistributing bikes, and managing station capacities more effectively.


```python
d1=data.groupby("member_casual").agg({"total_ride_length":"mean"})
d1.reset_index(drop=False, inplace=True)
d1
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
      <th>member_casual</th>
      <th>total_ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>casual</td>
      <td>28.987448</td>
    </tr>
    <tr>
      <th>1</th>
      <td>member</td>
      <td>12.852657</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.bar(d1.member_casual,d1.total_ride_length,color=['#6dc2ca', '#597dce' ])
plt.title("Average(mean) ride duration per member(Casuals X Members)")
plt.ylabel("Average ride duration")
plt.xlabel("Member type")
plt.ylim(0,40)
plt.show()
```


    
![png](img/output_71_0.png)
    


1. Average Ride Length:

    - Casual Riders: On average, casual riders have longer bike rides with an average duration of approximately 28.99 minutes.
    - Members: Members, on the other hand, have shorter bike rides with an average duration of about 12.85 minutes.

2. Implications:

    - Usage Patterns: The longer average ride length for casual riders may indicate that they use the bikes for leisurely activities or longer-distance trips compared to members, who likely use the service more frequently for shorter commutes or routine travel.


```python
m4=member.groupby("name_month").agg({"total_ride_length":"mean"})
m4.reset_index(drop=False, inplace=True)
m4
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
      <th>name_month</th>
      <th>total_ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>12.123603</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>14.098308</td>
    </tr>
    <tr>
      <th>2</th>
      <td>December</td>
      <td>11.685570</td>
    </tr>
    <tr>
      <th>3</th>
      <td>February</td>
      <td>11.062655</td>
    </tr>
    <tr>
      <th>4</th>
      <td>January</td>
      <td>10.706461</td>
    </tr>
    <tr>
      <th>5</th>
      <td>July</td>
      <td>14.046708</td>
    </tr>
    <tr>
      <th>6</th>
      <td>June</td>
      <td>13.532983</td>
    </tr>
    <tr>
      <th>7</th>
      <td>March</td>
      <td>10.823058</td>
    </tr>
    <tr>
      <th>8</th>
      <td>May</td>
      <td>13.429322</td>
    </tr>
    <tr>
      <th>9</th>
      <td>November</td>
      <td>11.799580</td>
    </tr>
    <tr>
      <th>10</th>
      <td>October</td>
      <td>12.402766</td>
    </tr>
    <tr>
      <th>11</th>
      <td>September</td>
      <td>13.416386</td>
    </tr>
  </tbody>
</table>
</div>




```python
c4=casual.groupby("name_month").agg({"total_ride_length":"mean"})
c4.reset_index(drop=False, inplace=True)
c4
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
      <th>name_month</th>
      <th>total_ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>28.584172</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>36.090705</td>
    </tr>
    <tr>
      <th>2</th>
      <td>December</td>
      <td>20.354585</td>
    </tr>
    <tr>
      <th>3</th>
      <td>February</td>
      <td>23.845751</td>
    </tr>
    <tr>
      <th>4</th>
      <td>January</td>
      <td>23.593841</td>
    </tr>
    <tr>
      <th>5</th>
      <td>July</td>
      <td>33.202546</td>
    </tr>
    <tr>
      <th>6</th>
      <td>June</td>
      <td>30.216061</td>
    </tr>
    <tr>
      <th>7</th>
      <td>March</td>
      <td>22.082546</td>
    </tr>
    <tr>
      <th>8</th>
      <td>May</td>
      <td>29.360879</td>
    </tr>
    <tr>
      <th>9</th>
      <td>November</td>
      <td>20.324220</td>
    </tr>
    <tr>
      <th>10</th>
      <td>October</td>
      <td>23.403470</td>
    </tr>
    <tr>
      <th>11</th>
      <td>September</td>
      <td>25.752194</td>
    </tr>
  </tbody>
</table>
</div>




```python
mrg4=pd.merge(m4,c4, on='name_month', how='outer', suffixes=('_member', '_casual'))

months_order = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
mrg4['month_order'] = mrg4['name_month'].apply(lambda x: months_order.index(x))
mrg_sorted4 = mrg4.sort_values(by='month_order')
mrg_sorted4.drop('month_order', axis=1, inplace=True)
mrg_sorted4
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
      <th>name_month</th>
      <th>total_ride_length_member</th>
      <th>total_ride_length_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>January</td>
      <td>10.706461</td>
      <td>23.593841</td>
    </tr>
    <tr>
      <th>3</th>
      <td>February</td>
      <td>11.062655</td>
      <td>23.845751</td>
    </tr>
    <tr>
      <th>7</th>
      <td>March</td>
      <td>10.823058</td>
      <td>22.082546</td>
    </tr>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>12.123603</td>
      <td>28.584172</td>
    </tr>
    <tr>
      <th>8</th>
      <td>May</td>
      <td>13.429322</td>
      <td>29.360879</td>
    </tr>
    <tr>
      <th>6</th>
      <td>June</td>
      <td>13.532983</td>
      <td>30.216061</td>
    </tr>
    <tr>
      <th>5</th>
      <td>July</td>
      <td>14.046708</td>
      <td>33.202546</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>14.098308</td>
      <td>36.090705</td>
    </tr>
    <tr>
      <th>11</th>
      <td>September</td>
      <td>13.416386</td>
      <td>25.752194</td>
    </tr>
    <tr>
      <th>10</th>
      <td>October</td>
      <td>12.402766</td>
      <td>23.403470</td>
    </tr>
    <tr>
      <th>9</th>
      <td>November</td>
      <td>11.799580</td>
      <td>20.324220</td>
    </tr>
    <tr>
      <th>2</th>
      <td>December</td>
      <td>11.685570</td>
      <td>20.354585</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(20,11))
mrg_sorted4.plot(kind='bar',color=['#6dc2ca', '#597dce'],ax=ax)
plt.title("Ride duration by users per month ")
plt.ylabel("Duration")
plt.xlabel("Month") 
plt.xticks(np.arange(12), mrg_sorted4['name_month'], rotation=45)
plt.show()
```


    
![png](img/output_76_0.png)
    


1. Seasonal Trends in Ride Length:

    - Summer Peaks: Months from June to August (June, July, August) consistently show the longest average ride lengths for both members and casual riders. This suggests that users tend to take longer rides during the warmer months, possibly for leisure or recreational purposes.
    - Winter Decrease: Ride lengths tend to be shorter during winter months (December to February), which may indicate reduced bike usage for commuting or outdoor activities during colder weather.

2. Member vs. Casual Rider Patterns:

    - Consistent Differences: Throughout the year, casual riders generally have longer average ride lengths compared to members. This pattern holds across most months, indicating that casual riders may use the bikes for longer trips or more extended periods compared to members who might use the service for shorter, more frequent trips.

3. Monthly Variations:

    - August Peak: August shows the highest average ride lengths for both groups, suggesting peak summer usage patterns.

4. Strategic Insights:

    - Service Planning: Understanding these seasonal and monthly variations can help Cyclistic optimize bike deployment, station management, and maintenance schedules to meet the differing needs of riders throughout the year.
    - Marketing and Promotions: Tailoring marketing campaigns and promotions based on these insights can help attract and retain riders during peak months and encourage usage during quieter periods.

### Recommendations

Based on the analyses of Cyclistic's bike-share data, here are several recommendations for improving service delivery, attracting more riders, and maximizing the conversion of casual riders into annual members:

1. Targeted Marketing Campaigns:

    - Design marketing strategies that target casual riders during peak usage periods, such as weekends and summer months. Highlight the convenience and benefits of becoming a member, including cost savings and priority access.
    - Use digital media effectively to reach potential members, emphasizing the flexibility and cost-effectiveness of annual memberships compared to single-ride or day-pass options.

2. Promotional Incentives:

    - Offer promotions and discounts specifically aimed at converting casual riders into members. For instance, provide discounted membership rates for first-time sign-ups or introduce referral programs where current members can earn rewards for referring new members.
    - Tailor promotional offers based on seasonal trends and rider preferences identified in the data, such as longer rides during summer and leisure activities.

3. Service Optimization:

    - Based on hourly and daily usage patterns, adjust bike deployment and station capacities to meet peak demand, especially during commuting hours and weekends.
    - Ensure that stations are well-maintained and strategically located in areas with high rider traffic, enhancing convenience for both members and casual riders.

4. Enhanced Customer Experience:

    - Improve the overall customer experience through user-friendly mobile apps and intuitive station interfaces. Simplify the membership sign-up process and provide real-time updates on bike availability and station statuses.
    - Gather feedback from current members and casual riders to identify pain points and areas for improvement, focusing on enhancing service reliability and user satisfaction.

5. Data-Driven Decision Making:

    - Continue analyzing historical trip data to monitor trends and adjust strategies accordingly. Use predictive analytics to forecast future demand and optimize resource allocation, such as bike inventory and station capacity.
    - Implement A/B testing for marketing campaigns to evaluate effectiveness and refine strategies based on performance metrics.

6. Community Engagement:

    - Foster a sense of community among Cyclistic users through events, social media engagement, and partnerships with local businesses. Highlight the environmental benefits and community impact of bike-sharing to attract socially conscious riders.

By implementing these recommendations, Cyclistic can effectively capitalize on its existing user base, attract new riders, and enhance overall operational efficiency and customer satisfaction in its bike-share program.
