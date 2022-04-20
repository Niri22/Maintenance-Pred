# Asset Maintenance Analysis

### Contributers and Contact Information: Niranjani.Pandey (niranjani.pandey168@gmail.com)

## Problem Statement addressed: GRAPH FOR BETTER SYSTEMS

Due to COVID-19, Public Transport Operators have had decreased ridership and revenue. With significatly lower ridership and additional cost for sanitisation of vehicles and platforms, Operators have been looking to lower their costs without impacting customer experience. We present an alternnative maintenance scheduling strategy to reduce maintance cost related to upkeep of transport vehicles. In this particular solution, we will be looking at Ferry Maintenance of BC Ferries, which run about 20 ferries over 12 routes and 8 termials. 

### Description:
In fiscal 2021, traffic and revenue of BC Ferries were drastically impacted by the pandemic, including as a result of changed customer travel patterns, preventive health and safety measures and various travel restrictions. BC Ferries carried 6.7 million vehicles and 13.1 million passengers, decreases of 23.8% and 39.7% respectively, compared to the prior year. Revenue from vehicle and passenger traffic on the designated ferry routes in fiscal 2021 totalled $424.1 million, a decrease of $189.1 million from the prior year. (BC Ferries Annual Report to the BC Ferries Commissioner 2020-21).  Even with such significant changes, the annual maintenance cost was around $85 million in FY 2019 and FY 2020. This implies that the same scheduled maintenance routine was performed instead of a predictive maintenance routine. With hybrid work from home being the norm, we can expect the ridership annd revenue to remain lower tha pre-pandemic levels. 
![image](https://user-images.githubusercontent.com/66136976/164317143-abb50284-2eeb-4244-96e8-c05fec4474ec.png)
Predictive maintenance refers to the use of data-driven, proactive maintenance methods that are designed to analyze the condition of equipment and help predict when maintenance should be performed. Our project makes these data-driven predictions about unplanned downtime or breakdowns at the terminal and of ferries for future ferry trips. The operators can then use these predictions to scheduled issue specific maintenance as well as adapt operations and logistics of ferries, routes and terminals. 

We utilized data provided from BC Ferries for CANSSI NCSC Ferry Delays Kaggle Competition. It included data involving records about the sailing of 61,880 sailings occurring between August 2016 and March 28 and an indicator is provided describing whether or not the sailing was delayed. By utilizing data from readily available and established sources also reduces the cost and effort of implementation of tracking sensors on ferry equipment which usually follows a switch to predictive maintenance. We used graph technology to combine this data with a time series of temperature and humidity from Vancouver Harbour and a time series of temperature, humidity, pressure, wind speed, and wind direction. We then used the complex graph with algorithms to predict a trip’s status ( ‘Status’ label attribute in the FerryTrip vertices). 


To get predictions on ‘Status’ label of a ferry trip, we first used a community detection algorithm on training and testing data from the Tiger Graph Data Science library to identify clusters of Trips and neighbouring vertices with some status labels such as On time, Operational Delays, Mechanical difficulties with vessel, Extreme tidal conditions,  Mechanical difficulties with terminal equipment, Heavy traffic volume, and Vessel start-up delays. After test and training data were clustered, we used a classification algorithm within the cluster to predict the ‘delay_indicator’ label of test ferry trips. If the indicator was 1, then trip would have a status of the cluster. If the indicator was 0, then the trip would have a status label of the cluster.

This solution isn’t only applicable for ferries but also other transport vehicles. Using similar data, predictive issues with metro cars, streetcars, airplanes, buses and taxies can be found.  According to the McKinsey management consulting firm, predictive maintenance can generate substantial savings by increasing production line availability by 5 to 15% and reducing maintenance costs by 18 to 25%. A 18 to 25% reduction in maintenance cost for BC Ferries would lead to a saving of $15.3 million - $21.35 million every year.

### Data: Give context for the dataset used and give full access to judges if publicly available or metadata otherwise.
Technology Stack:  Describe technologies and programming languages used.
Visuals: Feel free to include other images or videos to better demonstrate your work.
Link websites or applications if needed to demonstrate your work.
According to Transit Data collected from public transit agencies in April 2020, the ridership levels across all public transit modes have been decreased by 73% in the United States (Transitapp, 2020, EBP, 2021).
