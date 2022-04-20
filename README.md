# Asset Predictive Maintenance 

### Contributers and Contact Information: Niranjani.Pandey (niranjani.pandey168@gmail.com)

## Problem Statement addressed: GRAPH FOR BETTER SYSTEMS

Due to COVID-19, Public Transport Operators have had decreased ridership and revenue. With significantly lower ridership and additional cost for sanitization of vehicles and platforms, Operators have been looking to lower their costs without impacting customer experience. We present an alternative maintenance scheduling strategy to reduce maintenance cost related to upkeep of transport vehicles. In this particular solution, we will be looking at Ferry Maintenance of BC Ferries, which run about 20 ferries over 12 routes and 8 terminals. 


### Description:
In fiscal 2021, traffic and revenue of BC Ferries were drastically impacted by the pandemic, including as a result of changed customer travel patterns, preventive health and safety measures and various travel restrictions. BC Ferries carried 6.7 million vehicles and 13.1 million passengers, decreases of 23.8% and 39.7% respectively, compared to the prior year. Revenue from vehicle and passenger traffic on the designated ferry routes in fiscal 2021 totalled $424.1 million, a decrease of $189.1 million from the prior year. (BC Ferries Annual Report to the BC Ferries Commissioner 2020-21).  Even with such significant changes, the annual maintenance cost was around $85 million in FY 2019 and FY 2020. This implies that the same scheduled maintenance routine was performed instead of a predictive maintenance routine. With hybrid work from home being the norm, we can expect the ridership annd revenue to remain lower tha pre-pandemic levels. 
![image](https://user-images.githubusercontent.com/66136976/164317143-abb50284-2eeb-4244-96e8-c05fec4474ec.png)
Predictive maintenance refers to the use of data-driven, proactive maintenance methods that are designed to analyze the condition of equipment and help predict when maintenance should be performed. **Our project makes these data-driven predictions about unplanned downtime or breakdowns at the terminal and of ferries for future ferry trips. The operators can then use these predictions to scheduled issue specific maintenance as well as adapt operations and logistics of ferries, routes and terminals.** 
![image](https://user-images.githubusercontent.com/66136976/164328707-bd1119fb-c60d-4e73-bc77-6b467022c2a6.png)
We utilized data provided from BC Ferries for CANSSI NCSC Ferry Delays Kaggle Competition. It included data involving records about the sailing of 61,880 sailings occurring between August 2016 and March 28 and an indicator is provided describing whether or not the sailing was delayed. By utilizing data from readily available and established sources also reduces the cost and effort of implementation of tracking sensors on ferry equipment which usually follows a switch to predictive maintenance. We used tigergraph to represent the relatioships within the data. The schema and schema diagram can be seen as below .  Then algorithms from the TigerGraph Data Science Library were used to predict a trip’s status ( ‘Status’ label attribute in the FerryTrip vertices). 

Vertex Types: 
  - VERTEX FerryTrip(PRIMARY_ID trip_id INT, Arrival_to_terminal STRING, Departure_to_terminal STRING, Vessel STRING, Status STRING, Date STRING, Time STRING, Delay_ind STRING, Trip_duration INT) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Termials(PRIMARY_ID terminal STRING, Route_area STRING, Short_term_parking_capacity INT, Long_term_parking_capacity STRING, TransLink STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Statuses(PRIMARY_ID Status STRING, Type STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Vessels(PRIMARY_ID Vessel STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Depart_time(PRIMARY_ID Time STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Days(PRIMARY_ID Day STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Months(PRIMARY_ID Month STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
  - VERTEX Terminals(PRIMARY_ID terminal STRING) WITH STATS="OUTDEGREE_BY_EDGETYPE", PRIMARY_ID_AS_ATTRIBUTE="true"
Edge Types: 
  - DIRECTED EDGE Arrival_terminal(FROM FerryTrip, TO Termials) WITH REVERSE_EDGE="reverse_Arrival_terminal"
  - DIRECTED EDGE reverse_Arrival_terminal(FROM Termials, TO FerryTrip) WITH REVERSE_EDGE="Arrival_terminal"
  - DIRECTED EDGE Departure_from(FROM Termials, TO FerryTrip) WITH REVERSE_EDGE="reverse_Departure_from"
  - DIRECTED EDGE reverse_Departure_from(FROM FerryTrip, TO Termials) WITH REVERSE_EDGE="Departure_from"
  - UNDIRECTED EDGE Ferry_status(FROM FerryTrip, TO Statuses)
  - UNDIRECTED EDGE Vessel_used(FROM FerryTrip, TO Vessels)
  - UNDIRECTED EDGE Departed_time(FROM Depart_time, TO FerryTrip)
  - UNDIRECTED EDGE trip_day(FROM FerryTrip, TO Days)
  - UNDIRECTED EDGE trip_month(FROM Months, TO FerryTrip)

Graphs: 
  - Graph Draft1(FerryTrip:v, Termials:v, Statuses:v, Vessels:v, Depart_time:v, Days:v, Months:v, Arrival_terminal:e, reverse_Arrival_terminal:e, Departure_from:e, reverse_Departure_from:e, Ferry_status:e, Vessel_used:e, Departed_time:e, trip_day:e, trip_month:e)
 ![image](https://user-images.githubusercontent.com/66136976/164337914-42ceceea-8a1e-4ed7-a21e-e7e38eddaa9d.png)

To get predictions on ‘Status’ label of a ferry trip, we first used a community detection algorithm on training and testing data from the Tiger Graph Data Science library to identify clusters of Trips and neighbouring vertices with some status labels such as On time, Operational Delays, Mechanical difficulties with vessel, Extreme tidal conditions,  Mechanical difficulties with terminal equipment, Heavy traffic volume, and Vessel start-up delays. After test and training data were clustered, we used a classification algorithm within the cluster to predict the ‘status’ label of test ferry trips. The output of algorithm produces a table with test trips as rows and predicted probablity of each Status type as columnns. For each test trip, the status is equal the highest predictied probablity for all Statuses.

This solution isn’t only applicable for ferries but also other transport vehicles. Using similar data, predictive issues with metro cars, streetcars, airplanes, buses and taxies can be found.  According to the McKinsey management consulting firm, predictive maintenance can generate substantial savings by **increasing production line availability by 5 to 15% and reducing maintenance costs by 18 to 25%**. A 18 to 25% reduction in maintenance cost for BC Ferries would lead to **savings of $15.3 million - $21.35 million every year**.

### Data: 
We utilized data provided from BC Ferries for [CANSSI NCSC Ferry Delays Kaggle Competition](https://www.kaggle.com/competitions/canssi-ncsc-ferry-delays/data)
train.csv This dataset is a comma separated variable file indicating details about the 49,504 sailings in the training set. Note that quotes are used in the column names throughout. The columns are as follows: The ferry dataset involves records about the sailing of 61,880 sailings occurring between August 2016 and March 2018 for routes starting or ending at one of Horseshoe Bay, Swartz Bay, Tsawwassen and Departure Bay. The information provided is described below. For the 49,504 sailings among training data, the actual duration of the sailing is provided and an indicator is provided describing whether or not the sailing was delayed. For the 12,376 sailings in the testing data, the actual duration of the sailing and the delay indicator are not provided and instead the delay indicator must be predicted.
- "Vessel.Name" The name of the ferry. Possible values include "Bowen Queen", "Coastal Celebration", "Coastal Inspiration", "Coastal Renaissance", "Island Sky", "Mayne Queen", "Queen of Alberni", "Queen of Capilano", "Queen of Coquitlam", "Queen of Cowichan", "Queen of Cumberland", "Queen of Nanaimo", "Queen of New Westminster", "Queen of Oak Bay", "Queen of Surrey", "Salish Eagle", "Salish Raven", "Skeena Queen", "Spirit of British Columbia", "Spirit of Vancouver Island". Note that quotations are used around the elements of this column, and throughout for string based elements. Quotes are not used around elements of columns with integer values.
- Scheduled.Departure" A string representing the time of day at which the sailing was scheduled for. This is a timestamp of the form "7:00 AM", "8:00 AM", etc.
- Status" A string indicating if the sailing was not delayed ("On Time") or delayed, with the reason for delay given ("Traffic delays", "Operational delay", etc).
- "Trip" A string indicating the departure harbour and the arrival harbour for the sail (e.g. "Tsawwassen to Swartz Bay" or "Tsawwassen to Southern Gulf Islands").
- "Trip.Duration" An integer indicating the length of the sailing, in minutes, or the string NA if this information was not available.
- "Day" A string indicating the day of the week of the sailing ("Monday", "Tuesday", etc).
- "Month" A string indicating the month of the sailing ("January", "February", etc).
- "Day.of.Month" An integer indicating the day of month of the sailing (1 for the first day of the month, 2 for the second day of the month, etc).
- "Year" An integer indicating the year of the sail (2016, 2017, etc).
- "Full.Date" A string indicating the date of the sail in the form "28 August 2016", "29 August 2016", etc. Note that this column contains information from which the previous 4 columns can be derived. These 4 columns are provided for convenience, obviating the need to cross list with calendars.
- "Delay.Indicator" An integer binary indicator (0 or 1) describing whether or not the sailing was delayed (0 = no delay, 1 = delay). **We did not predict the Delay Indicator. We are predicting Status in our project**

### Some information about the routes: 
Tsawwassen - Metro Vancouver (Tsawwassen) to Victoria - Vancouver Island (Swartz Bay)
- uses vessel Spirit of Vancouver Island made 1994, 167m long, 11,681 tonnes, 2100 passenger, 19.5 knots, 21394 hp
- uses vessel Spirit of British Columbia made 1993, 167m long, 11,642 tonnes, 2,100 passengers, 19.5 knots, 21,394 hp
- uses vessel Queen of New Westiminster 1964 build, 130m long, 6,129 tonnes displacement, 1332 passenger, 20 knots, 16800 hp
- uses vessel Coastal Celebration made 2007, 160m long, 10034 tonnes, 1604 passenger, 23 knots, 21,444 hp
- These are all big vessels.

West Vancouver – Metro Vancouver (Horseshoe Bay) to Nanaimo – Vancouver Island (Departure Bay)
- uses vessel Queen of Oak Bay built 1981, 139m long, 6,673 tonnes, 1494 passengers, 20.5 knots, 11,840 hp
- uses vessel Coastal Renaissance (this is a big vessel) goes @ 23 knots, hp of 21,444, around 160m long and 10 tonnes, built in 2007, 1604 passengers
- uses vessel Queen of Coquitlam built 1976, 139m long, 316 family sized car capacity people stay in cars, 1494 passengers, 20.5 knots, 11,860 hp
- uses vessel Queen of Cowichan goes @ 20.5 knots, 11,860 hp, 1,494 passengers, 6,508 tonnes, 139m long, built 1976
- These are all mid-sized vessels.

More routes are drawn here Major and Northern Routes vessel positions are shown here bcferries.com.

Here are the ones we are interested in for Vancouver-Vancouver Island Routes:
- Swartz Bay - Tsawwassen (around 1h30m bidirectional travel)Swartz Bay - Tsawwassen
- Horseshoe Bay - Departure Bay (around 1h45m bidirectional travel)Horseshoe Bay - Departure Bay
(*) The routes we are concerned with are only (2) of (9), we can't assume the other routes are causally independent
 
 ### Some information about ferry trips statuses:
- 88.2% BC Ferries departed on time or within 10 minutes of scheduled sailing times
- Sailing were late for several reasons
  - [58%] Heavy Traffic
  - [25%] Procedural Issues (i.e. marine traffic, accommodating passengers, fuelling, Transport Canada required drills)
  - deck space is given to a mix of passenger deck space (car/van/SUV) and mixed vehicle deck space (car/van/SUV/Truck/bus/RV), here is a graph
  - [5%] Mechanical
  - [6%] Incidents (i.e. medical and rescue emergencies, vehicles stalled or stuck onboard, customer-related incidents)
  BCFerries twitter feed is just a litany of disasters twitter of BCFerries which cause delays
  - [3%] Weather
  - [3%] Miscellaneous

# Installation
Instructions: 

### To determine predictions:
1. Clone repository
2. Import tar ball solution and data in csv files into TigerGraph using Import Existing Solution
4. Run installed queries in tg cloud or using Terminal(I used Terminal because it was easy to extract the output).



# Known Issues and Future Improvements
Some limitations: 
- The age of vessels is not taken into account. More information of lifecycle of a ferry and related issues with aging vessels can improve predictive maintenance scheduling. 
- The data used is from pre-pandemic levels, so the traffic congestion data provided by CANSSI will not lead to realistic measure of impact of traffic congestion on bridges nearby on traffic delays.


# Future Improvements: 
- Integrating traffic congestion levels on nearby bridges to predict traffic volume related issues. 
- Intergrating passeger volume information by utilisng TransLink(train) ridership and volume
- Using data with more detailed issues/breakdown not only when on a trip but also outside operational hours.
- A further improvement can be done by integrating at real-time tweets about ferries to improve prediction of traffic volume related delays/holdup. 

# Reflections


# References
https://www150.statcan.gc.ca/n1/pub/45-28-0001/2021001/article/00030-eng.htm
https://www.bcferries.com/our-company/investor-relations
https://www.kaggle.com/competitions/canssi-ncsc-ferry-delays/data
https://docs-legacy.tigergraph.com
https://www.bcferries.com/web_image/h81/hcb/8805916246046.pdf
https://www.kaggle.com/code/iainwong/0-0-2-iainwong-domainknowledge-ncsc-ferry-delays
