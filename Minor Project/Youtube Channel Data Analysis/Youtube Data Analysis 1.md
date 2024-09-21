#                                     Youtube Data Analysis
### Insights through API Integration and Data Visualization


```python

```

#### Import Necessary libraries


```python
from googleapiclient.discovery import build
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
api_key = 'your_api_key'
```


```python
youtube = build('youtube', 'v3', developerKey=api_key)
```

# Data Extraction

######  - Extract video metadata (title, description, tags).


```python
channel_ids = ['UCChmJrVa8kDg05JfCmxpLRw',
               'UCmLGJ3VYBcfRaWbP6JLJcpA',
               'UCnz-ZXXER4jOvuED5trXfEA',
               'UCBwmMxybNva6P_5VmxjzwqA',
               'UC8butISFwT-Wl7EV0hUK0BQ',
               'UCiT9RITQ9PW6BhXK0y2jaeg',
               'UC2UXDak6o7rBm23k3Vv5dww',
               'UC7cs8q-gJRlGwj4A8OmCmXg',
               'UCsvqVGtbbyHaMoevxPAq9Fg'
              ]
```


```python
def get_channel_stats(youtube, channel_ids):
    
    all_data = []
    request = youtube.channels().list(
                part = 'snippet, contentDetails, statistics',
                id = ','.join(channel_ids))
    response = request.execute()

    for i in range(len(response['items'])):
        data = dict(channel_name = response['items'][i]['snippet']['title'],
                    Subscribers = response['items'][i]['statistics']['subscriberCount'],
                    Views = response['items'][i]['statistics']['viewCount'],
                    Total_videos = response['items'][i]['statistics']['videoCount'])

        all_data.append(data)
    
    return all_data
```


```python
channel_statistics = get_channel_stats(youtube, channel_ids)
```


```python
channel_data.dtypes
```




    channel_name    object
    Subscribers     object
    Views           object
    Total_videos    object
    dtype: object




```python
channel_data = pd.DataFrame(channel_statistics)
```


```python
channel_data
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
      <th>channel_name</th>
      <th>Subscribers</th>
      <th>Views</th>
      <th>Total_videos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Apna College</td>
      <td>5570000</td>
      <td>872763411</td>
      <td>835</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ken Jee</td>
      <td>260000</td>
      <td>9051595</td>
      <td>287</td>
    </tr>
    <tr>
      <th>2</th>
      <td>techTFQ</td>
      <td>312000</td>
      <td>17236988</td>
      <td>137</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Simplilearn</td>
      <td>4220000</td>
      <td>370388203</td>
      <td>7741</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Tina Huang</td>
      <td>669000</td>
      <td>31806637</td>
      <td>229</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Seattle Data Guy</td>
      <td>95500</td>
      <td>4990547</td>
      <td>253</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Darshil Parmar</td>
      <td>141000</td>
      <td>6624331</td>
      <td>154</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Alex The Analyst</td>
      <td>818000</td>
      <td>36831835</td>
      <td>312</td>
    </tr>
    <tr>
      <th>8</th>
      <td>freeCodeCamp.org</td>
      <td>9690000</td>
      <td>735824353</td>
      <td>1705</td>
    </tr>
  </tbody>
</table>
</div>



# Data Cleaning and Transformation


```python
channel_data['Subscribers'] = pd.to_numeric(channel_data['Subscribers'])
channel_data['Views'] = pd.to_numeric(channel_data['Views'])
channel_data['Total_videos'] = pd.to_numeric(channel_data['Total_videos'])
```


```python
channel_data.dtypes
```




    channel_name    object
    Subscribers      int64
    Views            int64
    Total_videos     int64
    dtype: object



# Data Analysis and Visualization

# Identify Content Trends


#### The project uses the YouTube Data API to gather channel names, subscriber counts, view counts, and total video counts. Content trends are identified by creating a bar chart that shows each channel's name on the x-axis and its total number of videos on the y-axis. This chart quickly highlights which channels are producing more content. By studying these trends, creators and marketers can understand popular content themes and strategies that attract viewers. This helps in refining content approaches to better engage audiences and enhance performance on YouTube.

#### Plot total videos per channel


```python
channel_data
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
      <th>channel_name</th>
      <th>Subscribers</th>
      <th>Views</th>
      <th>Total_videos</th>
      <th>performance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Apna College</td>
      <td>5570000</td>
      <td>872763411</td>
      <td>835</td>
      <td>878333411</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ken Jee</td>
      <td>260000</td>
      <td>9051595</td>
      <td>287</td>
      <td>9311595</td>
    </tr>
    <tr>
      <th>2</th>
      <td>techTFQ</td>
      <td>312000</td>
      <td>17236988</td>
      <td>137</td>
      <td>17548988</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Simplilearn</td>
      <td>4220000</td>
      <td>370388203</td>
      <td>7741</td>
      <td>374608203</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Tina Huang</td>
      <td>669000</td>
      <td>31806637</td>
      <td>229</td>
      <td>32475637</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Seattle Data Guy</td>
      <td>95500</td>
      <td>4990547</td>
      <td>253</td>
      <td>5086047</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Darshil Parmar</td>
      <td>141000</td>
      <td>6624331</td>
      <td>154</td>
      <td>6765331</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Alex The Analyst</td>
      <td>818000</td>
      <td>36831835</td>
      <td>312</td>
      <td>37649835</td>
    </tr>
    <tr>
      <th>8</th>
      <td>freeCodeCamp.org</td>
      <td>9690000</td>
      <td>735824353</td>
      <td>1705</td>
      <td>745514353</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(12, 6))
sns.barplot(data=channel_data, x='channel_name', y='Total_videos')
plt.title('Total Videos per Channel')
plt.xlabel('Channel Name')
plt.ylabel('Total Videos')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_21_0.png)
    



```python

```

# Understand Engagement Patterns


#### understanding engagement patterns involves visualizing the relationship between subscribers and views for each channel. The plot combines a bar chart and a line plot to display these metrics side by side.




```python
# Plot subscribers and views per channel
fig, ax1 = plt.subplots(figsize=(14, 7))

ax2 = ax1.twinx()
sns.barplot(data=channel_data, x='channel_name', y='Subscribers', ax=ax1, color='b', alpha=0.6)
sns.lineplot(data=channel_data, x='channel_name', y='Views', ax=ax2, color='r', marker='o')

ax1.set_xlabel('Channel Name')
ax1.set_ylabel('Subscribers', color='b')
ax2.set_ylabel('Views', color='r')
ax1.set_title('Subscribers and Views per Channel')
ax1.tick_params(axis='x', rotation=45)

plt.show()
```


    
![png](output_25_0.png)
    


#### Bar Chart (Subscribers): The blue bars represent the number of subscribers for each channel. This helps to identify channels with larger subscriber bases, indicating potential audience reach and loyalty.

#### Line Plot (Views): The red line with markers shows the number of views each channel has accumulated. This allows comparison against the subscriber base, highlighting channels where content resonates well with viewers.

#### By plotting subscribers and views together, creators and marketers can gauge how effectively channels convert subscribers into viewers. Channels with high engagement ratios may indicate strong content strategies that attract and retain viewership. This analysis guides strategies to enhance content engagement and optimize channel performance on YouTube.


```python

```

# Evaluate Channel Performance

#### Calculate performance metric (subscribers + views)



```python
channel_data['performance'] = channel_data['Subscribers'] + channel_data['Views']

# Plot channel performance
plt.figure(figsize=(12, 6))
sns.barplot(data=channel_data, x='channel_name', y='performance')
plt.title('Channel Performance (Subscribers + Views)')
plt.xlabel('Channel Name')
plt.ylabel('Performance')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_31_0.png)
    


#### Each bar represents a channel, with its length indicating the combined metric of subscribers and views. This visualization offers a direct comparison of how channels are performing in terms of audience reach and engagement.


```python

```
