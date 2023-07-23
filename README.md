# Creating Logistics Centers across Turkey for Transport Companies with Artificial Intelligence and Linear Programming

## Introduction and Problem Definition
This project addresses the challenge of determining the optimal number and locations of logistics centers for a transportation company across Turkey. By treating it as a capacitated facility location problem and employing linear programming, factors like population, demand, warehouse capacity, rent, personnel, and transportation costs are considered during the selection process. The objective is to establish an efficient and profitable logistics layout model, benefiting transportation companies by providing valuable insights into identifying suitable warehouse locations. Logistics centers play a vital role in decision-making processes for businesses, particularly in transportation, impacting customer satisfaction and cost reduction. Utilizing artificial intelligence to optimize warehouse locations aims to enhance logistics efficiency and profitability for the transportation company's operations.

## Data Set, Data Characteristics, Attributes
The data used in this project is collected from various sources. Latitude and longitude information of cities and districts are retrieved from [GitHub](https://gist.github.com/abdullahoguk/ee03c26a23dca6eda9c480b4967e77b6). The data includes 81 cities and 957 districts. Additionally, each city and district have attributes such as ID, plate code, name, latitude, and longitude. Population data is obtained from the Turkish Statistical Institute's (TÜİK) address-based population census [database](https://biruni.tuik.gov.tr/medas/?kn=95&locale=tr) for the year 2022. This data contains 81 cities, 973 districts, and respective population values. The surface areas of cities are obtained from [tech-worm.com](https://www.tech-worm.com/turkiye-illerinin-yuzolcumu-buyuklugune-gore-siralamasi/). The data from the Turkey Statistical Regions Classification (Türkiye İBBS) is also acquired from [TÜİK](https://tr.wikipedia.org/wiki/T%C3%BCrkiye%27nin_%C4%B0BBS%27si), using İBBS-II (26 sub-regions) data.

The data is preprocessed to fit the problem requirements. The preprocessing steps include:
- Cleaning city and district names, converting them to uppercase with proper formatting.
- Excluding city centers from the district data.
- Modifying district names for easy understanding of their related cities.
- Adding population information to city and district data.
- Combining city and district tables.
- Adding a region column based on İBBS-II regions.
- Removing districts that are closer than one-quarter of their radius to their city center using haversine distance.
- Calculating customer demand by multiplying the population of cities and districts by a certain ratio and adding random errors.

After preprocessing, the data set contains 769 samples with 7 attributes. The attributes include:
- city_code (plate code): The plate code of the city or district. A whole number ranging from 1 to 81.
- name (city/district name): The name of the city or district. Each name is unique. Example city name: Ankara, example district name: Ankara_Beypazarı
- region: The region to which the city or district belongs based on İBBS-II data. Example: TR51_Ankara_sub-region
- lat (latitude): The latitude value of the city or district center in degrees. Example: 39.9334
- lon (longitude): The longitude value of the city or district center in degrees. Example: 32.85411
- population: The population of the city or district. Example: 5,782,285
- demand: The total number of products demanded by customers in the city or district. Example: 115,636

![dataset sample](https://github.com/hasanerdemak/AI-Logistics-Centers-Turkey-LP/assets/70165677/d3df263f-c293-4793-9a52-ad1db329b636)

## Methodology
The project is implemented using Python and Jupyter Notebook. Various libraries are used, such as pandas for data manipulation, geopandas for map plotting, matplotlib for graph visualization, math for advanced mathematical operations, and pulp for solving optimization problem. The detailed implementation of the project is explained below.

### A. Dataset Reading, Potential Warehouse, and Customer Locations
First, the data set obtained from preprocessing is read. Then, a random 20% of the data set (154 samples) is selected as potential warehouse locations, and 90% of the data set (692 samples) is considered as customer locations. Separate data frames are created for these locations, and a geometry column is added using latitude and longitude information and the geopandas library. The map below shows the potential warehouse and customer locations.
![potential warehouse and customer locations](https://github.com/hasanerdemak/AI-Logistics-Centers-Turkey-LP/assets/70165677/5f70882a-51f6-48ef-a28d-e19e029c9df1)

### B. Yearly Demand per Region
In addition, the total demand for each of the 26 regions, considered according to İBBS-II system, is visualized on the map below.
![Yearly Demand per Region](https://github.com/hasanerdemak/AI-Logistics-Centers-Turkey-LP/assets/70165677/27ba3270-28e8-463a-a15a-90a64a3663ed)

### C. Supply and Fixed Costs
While determining the optimal number and locations of warehouses, factors such as the capacity of each warehouse (supply), yearly rent and personnel costs for each warehouse are considered. For warehouses located in cities, the yearly rent cost is set to 1,500,000 TL and personnel cost to 2,100,000 TL. For warehouses located in districts, the yearly rent cost is set to 500,000 TL and personnel cost to 700,000 TL. The total number of products each warehouse can supply is set to be three times the average demand of customers in its region.

### D. Transportation Costs
Transportation costs are calculated based on the haversine formula, which calculates the distance between two locations on Earth using latitude and longitude values. For warehouses in cities, transportation costs are considered as the haversine distance between the warehouse and its customer. For warehouses in districts, transportation costs are set to twice this distance.

### E. Optimization
The problem is formulated as a linear optimization problem with constraints. The objective function aims to minimize the total costs, considering fixed costs and transportation costs. The constraints ensure that each warehouse's capacity is not exceeded, and each customer's demand is met. The binary variable yj is used to determine whether a warehouse will be built (1) or not (0).

The problem is solved using the pulp library, providing an optimal solution.

## Experimental Results
In the optimal solution of the problem, the total cost is calculated as 193,782,902.25 TL. This result represents the minimum possible cost under the given constraints. Any other choice of warehouse number or locations would result in a higher value for the objective function. Additionally, the analysis suggests that 27 out of 154 potential warehouse locations are sufficient.

Warehouse locations are strategically placed in districts rather than cities, reducing overall costs. Out of 27 warehouses, 11 are located in cities, and the remaining 16 are in districts.

The map below displays the locations of the established warehouses and their corresponding customers.
![established warehouses and their corresponding customers](https://github.com/hasanerdemak/AI-Logistics-Centers-Turkey-LP/assets/70165677/da8e9f76-6aa0-49c8-8f82-6195ac9a0bee)

## Conclusion
In conclusion, this project presents an artificial intelligence agent designed to determine the optimal number and locations of logistics centers for transportation companies across Turkey. The capacitated facility location problem is treated as a mathematical optimization problem and solved using linear programming. This method provides a roadmap for creating more efficient and profitable logistics layout models for transportation companies, taking factors such as rent, personnel, and transportation costs into account. The results have significantly reduced logistics costs for transportation companies, leading to increased customer satisfaction. The map and database of the most suitable warehouse locations offer valuable insights for transportation companies. Future work may focus on further refining the method and exploring its applicability to other sectors.

