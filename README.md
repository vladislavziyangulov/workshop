# Hourly taxi demand prediction
In the project I used a large dataframe of ~6M rows to predict the number of taxi trips for the next hour for each community area in Chicago. Data taken from [data.cityofchicago.org](https://data.cityofchicago.org/Transportation/Taxi-Trips-2022/npd7-ywjz)

# To see all the plots:
[taxi_trips](https://nbviewer.org/github/vladislavziyangulov/workshop/blob/main/taxi_trips.ipynb)

[area8_taxi_trips](https://nbviewer.org/github/vladislavziyangulov/workshop/blob/b0b28e7bf40116310989eaec6ceac840bb01febe/area8_taxi_trips.ipynb)

# Summarizon

Pandas cannot process such a large dataframe efficiently, hence pyspark was used. The raw data was grouped and ordered by each community area and timestamp truncated to hour. Here are some findings: community areas 8, 32 and 76 are the busiest with an annual number of trips of about 1M; most other areas have less than 0.25M trips. There is different consumer behaviour on holidays like the New Year, Lincoln's Birthday, Washington's Birthday, Thanksgiving and Christmas. The middle of June was extremely hot and people used taxis more often. Demand is higher from 12:00 to 18:00. Thursday and Friday are the busiest weekdays, Sunday and Monday - the least. People use taxis the most in June, August and September. A GBT regressor model predicted the number of trips for the next hour moderately well for most areas with MAE~1.5. Most useful features were lag_1, hour and lag_24. However, I wasn't satisfied with MAE=22.2 for area 8. Hence, in a separate notebook, I trained an LSTM neural network and improved the test result to 17.6. 

# Difficulties / Challenges / Acquired knowledge
| Problem | Solution |
|--|--|
| **Running pyspark on a low-powered computer** | **Optimized pyspark session** |
| Missing data of demand for several hours | Joined a self-made dummy dataframe to fill in the gaps |
| Slow conversion from Pyspark to Pandas DataFrames | Reconfigured Pyspark session |
| Wanted to plot the demand on a map | Learned a new library Folium |
| Wanted to visualize outliers on a calendar | Used a new library and customized the colorscale |
| Adding a new "holidays" column | Learned to use holidays library & learned to use spark broadcast|
| No time-series crossvalidation in pyspark |  Wrote a function with a loop and grid search |
| Low metric for community area 8 | Analyzed the area separately |
| **No available Chicago weather dataset** | **Compiled myself with Selenium** |

# Used libraries
pandas, numpy

seaborn, matplotlib, plotly, folium

pyspark

selenium

sklearn, lightgbm, statsmodels, phik

tensorflow, keras
