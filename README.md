# Airline-Traffic-Visualization
_Visualization of passenger traffic at top 100 US airports from 2019-2020_

This visualization uses mobile GPS data pulled from 378 primary airports in the US between 2019 and 2020 and aggregated to a weekly level to show the estimated traffic observed at each airport over time. While measures were taken to validate and improve the accuracy of GPS based traffic estimations, GPS traffic is not wholly representative of true traveler traffic. As a result, this visualization is intended only to be used as a novel view of broad trends in the airline industry.

The visualization includes two animated components, a map and a line graph. The map includes bubbles for the top 100 airports by enplanements (sourced from BTS) within the studied timeframe. The size and color of the bubbles are animated based on weekly enplanement estimates by airport generated using mobile GPS traffic introduced above scaled to the level of enplanements using monthly BTS enplanement data. The line graph shows total BTS enplanements which has been disaggregated to a weekly level with GPS traffic.


**GPS Data Collection:**

This visualization was made possible through access to anonymized mobile phone GPS data. This dataset includes a unique identifier for all mobile devices in the US activated through a wireless plan as well as its position through time (indicated by latitude and longitude). Data was pulled via spatial query within the geofences of 378 primary airports and processed to identify devices most likely to belong to actively flying passengers. 


**GPS Data Processing:**

The following steps were taken in order to ensure mobile GPS traffic is only considered for actively flying passengers:
1)	All positional data from unique devices within an airport was combined into a single visit by chaining positions together into microclusters by proximity and time. 
2)	Devices associated with microclusters which spanned longer than 8 hours were excluded.
3)	Devices which were not observed within 2 of the 378 geofenced primary airports within 24 hours were excluded. 

The resulting visits were aggregated to identify total GPS traffic observed at each airport for each week between 2019 and 2020.


**GPS Data Validation: **

Enplanement data published by the Bureau of Transportation Statistics (BTS) was used to validate the GPS traffic estimations by month and by airport. 

Seasonal Validation:

Seasonal accuracy was validated using the BTS total enplanements across all airports.  Resulting in an overall correlation of .92 and R2 value of .84.

The relationship between GPS traffic and BTS enplanements was notably weaker within the year of 2019, with a correlation of .37 and R2 value of .14. This was caused by significant underestimation of roughly 40% which occurred within the months of October, November and December. Similar underestimation of 30% was also observed in Q4 of 2020.
Additionally, the year of 2020 appears to show lower GPS capture rates compared to 2019 as seen in the months of January and February. Despite higher raw enplanement volume in the first 2 months of 2020, raw GPS capture is lower than the same months of 2019. 

Airport Validation: 

The relationship between GPS traffic and BTS enplanements at the airport level was consistently strong across both 2019 and 2020, with correlations of .945 and .936 and R2 values of .89 and .88 respectively. 

Regional groups were tested for estimation error, and while none was found at the regional level, GPS traffic at Orlando International was found to underestimate BTS enplanements by 96% in both 2019 and 2020. 


**GPS Traffic Scaling:** 

While estimates of enplanements based on mobile GPS traffic have been shown to be directionally accurate, only 47.5 MM (3.7%) of the 1.3 B total enplanements in 2019 and 2020 were captured by GPS data, resulting in significantly underestimated raw traffic. To account for this, a regression methodology was used to create a function which would fit monthly GPS traffic to monthly enplanement levels. The coefficient of this function (with the intercept set to 0) could be applied as a scaling coefficient to the weekly GPS traffic data and bring its estimation closer to the true enplanement levels. 

Differences in GPS capture rate between 2019 and 2020 identified in the monthly data validation were addressed here by creating individual regression equations for each year, bringing the scaled estimate more closely in line with true performance. Additionally, due to seasonal GPS error observed in the monthly validation made it necessary to remove Q4 months from the regression training data to prevent skewing of the regression function.  

Once GPS traffic was scaled at an annual level to enplanement levels, Q4 error could be addressed by utilizing an adjustment coefficient for each of the 3 months built to account for the average error observed in 2019 and 2020. After adjusting for excess error observed in Q4, MCO still showed underestimation of enplanements greater than 90%, so a similar adjustment coefficient was applied to MCO GPS data. 
    
