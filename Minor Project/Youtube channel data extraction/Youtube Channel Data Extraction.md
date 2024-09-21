#                                     Youtube Channel Data Analysis
### Insights through API Integration and Data Visualization
# Large Volume of Data


```python
from googleapiclient.discovery import build
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
api_key = 'api_key'
```


```python
youtube = build('youtube', 'v3', developerKey=api_key)
```

# Data Extraction

####  - Extract video metadata.


```python
 channel_ids = [
                "UCBElRVEL-H0eDYdL2mb1Sng",
                "UCUJNWCiqMT_UudVRSSGZ3SA",
                "UCeoRAN5sr02w8_9aFWxIM4g",
                "UCkpm-wL7mUqPMTlFma8TOUg",
                "UC1XFTCWdHFiaWPk0lYNWWdg",
                "UCU2LTDwLnH6RZPnVWD2hLqA",
                "UCDwSnyXwd9WbAruzGKmQH9A",
                "UCBMjORco1vbZmC1cf_xnEmg",
                "UCwzfmK3YWOGPjrF0UDvzQ-Q",
                "UCvnkwI3ErNfTfuREuRXI6ZQ",
                "UCRoX4taJZz5R8fAQhysmIhw",
                "UCcaIZBgmiYTJP9fpO_xjMTQ",
                "UCBWVn5UkZqhQBdw1FHnGdpg",
                "UCHKVXtT1YBCYUnnr4apqXfg",
                "UC9j-uA9gV2B9nIX3lPaLsKQ",
                "UCaaCrr_lZjK1OydGJ87Y_hA",
                "UCME-uQDMlInGIFuC0-9ZrgQ",
                "UCgDLQqKapQGUKXghNtnWzwg",
                "UCQwXtYkGD3T3LxBKhhn74nA",
                "UCQrk97MBH6DToctKRfQJcNQ",
                "UCz2nLQAUvyG2AOWgRppaeKA",
                "UCuSJe2DB6dKV32GQ2lm25hg",
                "UCvZz3tLib3VHstDDn2KVnFw",
                "UC8b5ecQZKt9a1jDTuzudhwA",
                "UCrBPPGyR_7aOd882VpMRkRg",
                "UC_mr7bghelcFoKrB0B2FxSw",
                "UCtHv1zzGSawcksbm7Vj3w6w",
                "UCrcHYfph5Q1owBdVp7-1LTg",
                "UC-JFCYHPWDWhTe2RzAwfgrQ",
                "UC23D6jJsp9bpsfLpWgHVFRQ",
                "UCGUyYpOkxJ3ZQKXKgoLo49Q",
                "UCY4XZ2TuqgCDw6Tgdh8Nkrw",
                "UC6_D4w9dip50a3dVnt7S03Q",
                "UCZu-ll8ueyVMvnMTEOVnm5Q",
                "UCDIB942078BlAGqzRa4yEkw",
                "UCT7aiCHgDM-knR4E-UBBvgA",
                "UCuyMhLIzWRBnJ6dNdPXfXMQ",
                "UCpjs8ca_snn1pWB4ZICRzvg",
                "UC9ykRiM3WYAbsaUupoXsNSg",
                "UCWt3Cx6RrHX86_yF4I7f1LA",
                "UCso84GiPe7g4KCOx99frXhA",
                "UC7TlH8MVFR05hPG4T2f9Rbg",
                "UCJE6i6h4zye8suiOp2CXwJQ",
                "UCYoLcwG4o8S7vIfQSRDsmUw",
                "UCnTsUMBOA8E-OHJE-UrFOnA",
                "UCzoQzoOfHQCCouHbjrDEvPQ",
                "UCwXfRdTtVeVnXXF4poaYw0Q",
                "UCXMeQFZaNWnF7l8iHrLxxEQ",
                "UCAsALJ8Phn7crZCEwj7u5FQ",
                "UCaTCSgg52ehydUtwlny7CJQ"
                ]

# technology
# Gaming
# Vlogs
# Travel


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
      <td>Harr Travel</td>
      <td>138000</td>
      <td>49765059</td>
      <td>1958</td>
    </tr>
    <tr>
      <th>1</th>
      <td>VNT FOOD &amp; TRAVEL</td>
      <td>722000</td>
      <td>387226426</td>
      <td>772</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Travel With Mansoureh</td>
      <td>54800</td>
      <td>6832862</td>
      <td>513</td>
    </tr>
    <tr>
      <th>3</th>
      <td>travel india with rishi</td>
      <td>942000</td>
      <td>203853266</td>
      <td>643</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Budget food &amp; travel</td>
      <td>371000</td>
      <td>37442200</td>
      <td>204</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Motorcycle Travel Channel</td>
      <td>81400</td>
      <td>9338080</td>
      <td>234</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Laura Grace - Live Adventure Travel</td>
      <td>40500</td>
      <td>3935350</td>
      <td>83</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Travel with Shripad</td>
      <td>13200</td>
      <td>1805417</td>
      <td>486</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Travel With Twines</td>
      <td>2610</td>
      <td>264338</td>
      <td>294</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Travelling Mantra</td>
      <td>425000</td>
      <td>78506758</td>
      <td>528</td>
    </tr>
    <tr>
      <th>10</th>
      <td>The Traveling Trader</td>
      <td>276000</td>
      <td>16775077</td>
      <td>959</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Travel Alone Idea</td>
      <td>1530000</td>
      <td>246641380</td>
      <td>30</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Travel Bros</td>
      <td>270000</td>
      <td>76753757</td>
      <td>1329</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Travel King</td>
      <td>108000</td>
      <td>16188159</td>
      <td>605</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Travel Obscurer</td>
      <td>14200</td>
      <td>2365137</td>
      <td>121</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Travel in Europe(Norway Home)</td>
      <td>165000</td>
      <td>16063381</td>
      <td>172</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Travel with AK</td>
      <td>679000</td>
      <td>129660584</td>
      <td>243</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Helen and Tim Travel</td>
      <td>5430</td>
      <td>601895</td>
      <td>58</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Travel With Chatura</td>
      <td>363000</td>
      <td>32324864</td>
      <td>301</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Travel Alone India</td>
      <td>15200</td>
      <td>2722359</td>
      <td>93</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Travel With AB</td>
      <td>331000</td>
      <td>49548088</td>
      <td>276</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Travel Thirsty</td>
      <td>8050000</td>
      <td>3783432511</td>
      <td>914</td>
    </tr>
    <tr>
      <th>22</th>
      <td>DennisBunnik Travels</td>
      <td>132000</td>
      <td>26576853</td>
      <td>183</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Travel With Shilpa</td>
      <td>287000</td>
      <td>109898758</td>
      <td>619</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Travel With Laxman</td>
      <td>87300</td>
      <td>20451591</td>
      <td>398</td>
    </tr>
    <tr>
      <th>25</th>
      <td>The New Travel</td>
      <td>356000</td>
      <td>47391460</td>
      <td>424</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Tint &amp; Travel</td>
      <td>11000</td>
      <td>3195413</td>
      <td>1399</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Kamil - In Travel</td>
      <td>577000</td>
      <td>97404926</td>
      <td>500</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Travel Lover♡Anna</td>
      <td>59300</td>
      <td>9890713</td>
      <td>150</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Seoul Travel Walker</td>
      <td>160000</td>
      <td>45630727</td>
      <td>602</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Tech Travel Eat by Sujith Bhakthan</td>
      <td>2200000</td>
      <td>737516696</td>
      <td>2166</td>
    </tr>
    <tr>
      <th>31</th>
      <td>DustBugs Travel</td>
      <td>38600</td>
      <td>11713497</td>
      <td>548</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Travel and Knowledge</td>
      <td>72100</td>
      <td>10244911</td>
      <td>2399</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Travel Escapes</td>
      <td>188000</td>
      <td>37277107</td>
      <td>170</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Fit and Travel</td>
      <td>1280000</td>
      <td>167006892</td>
      <td>590</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Welcome to Travel</td>
      <td>5920</td>
      <td>378724</td>
      <td>80</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Travel Zindagi</td>
      <td>145000</td>
      <td>31784280</td>
      <td>609</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Travel Addicts Life</td>
      <td>22000</td>
      <td>2573869</td>
      <td>237</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Grounded Life Travel</td>
      <td>135000</td>
      <td>22848880</td>
      <td>614</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Travel With Kids TV</td>
      <td>18800</td>
      <td>7151557</td>
      <td>384</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Travel to b</td>
      <td>2330</td>
      <td>342557</td>
      <td>60</td>
    </tr>
    <tr>
      <th>41</th>
      <td>DREAM AND TRAVEL</td>
      <td>178000</td>
      <td>32904944</td>
      <td>679</td>
    </tr>
    <tr>
      <th>42</th>
      <td>My Travel Support</td>
      <td>387000</td>
      <td>83081136</td>
      <td>368</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Travel Forever : Вячеслав и Ирина Юмабаевы</td>
      <td>127000</td>
      <td>22737809</td>
      <td>245</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Travel with Prateek</td>
      <td>10500</td>
      <td>1485823</td>
      <td>314</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Travel With OTA EXPERT</td>
      <td>438000</td>
      <td>75805470</td>
      <td>713</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Travel Heart</td>
      <td>57800</td>
      <td>7644624</td>
      <td>222</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Travel With Adil</td>
      <td>144000</td>
      <td>19204603</td>
      <td>379</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Samuel and Audrey - Travel and Food Videos</td>
      <td>427000</td>
      <td>104885203</td>
      <td>1347</td>
    </tr>
    <tr>
      <th>49</th>
      <td>TARDY TRAVEL</td>
      <td>87700</td>
      <td>26238583</td>
      <td>140</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Save to CSV
channel_data.to_csv('youtube_channel_Travel.csv', index=False)

print("CSV file created successfully!")
```

    CSV file created successfully!
    
