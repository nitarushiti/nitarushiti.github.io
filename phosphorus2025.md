## Ontario PWQMN Total Phosphorus Analysis, 2025

### Project Overview

This project analyzes 2025 total phosphorus measurements from Ontario’s Provincial Water Quality Monitoring Network (PWQMN). The goal was to build a reproducible workflow for cleaning environmental monitoring data, storing and querying it in a relational database, and examining spatial and temporal patterns in phosphorus concentrations.
The workflow combined Python and Pandas for data cleaning and exploratory analysis, SQLite and SQL for database management and station-level queries, and QGIS for spatial visualization.

### Methods and Data Preparation

### 1. Data Preparation and Quality Control

The original PWQMN dataset contained multiple analytes, monitoring stations, sampling dates, laboratory identifiers, detection limits and result qualifiers. Total phosphorus records were extracted into a separate analytical dataset. Numeric values were preserved alongside the original reported result so that values such as <0.5 were not incorrectly interpreted as measured concentrations of 0.5 µg/L. Collection Site identifiers were retained as text because many station IDs contain leading zeroes. The dataset was also checked for missing values, potential duplicate records, variation in detection limits and consistency of laboratory result classifications. Potential duplicate environmental measurements were not automatically removed because repeated records can represent legitimate field duplicates, laboratory replicates or separate samples. For the main concentration analysis, only records where "Result Call" = "Detected" were included. This resulted in 3,744 detected total phosphorus measurements.

### 3. Database and Statistical Analysis

The cleaned total phosphorus dataset was imported into a SQLite database together with station-coordinate information. SQL was used to perform quality checks, summarize measurements by monitoring station, examine monthly patterns, and join station-level results with geographic coordinates. Python was used for descriptive and exploratory analysis. Summary statistics included the mean, median, standard deviation, percentiles, minimum and maximum detected concentrations. Because the concentration distribution was strongly right-skewed, the median was used as the primary statistic for comparing monitoring stations and monthly patterns. Station-level summaries included detected sample count, mean, median, minimum and maximum total phosphorus concentration. Monthly summaries were used to compare median phosphorus concentrations and sampling intensity throughout 2025.

### 4. Spatial Analysis

Station-level results were imported into QGIS using latitude and longitude coordinates. Monitoring locations were mapped and symbolized according to average detected total phosphorus concentration. Of the 449 monitoring sites with detected total phosphorus measurements, 443 were successfully matched to coordinate records and included in the spatial analysis. Sites without matching coordinates were retained in the statistical analysis but excluded from the map. A continuous interpolated phosphorus surface was not created because PWQMN stations represent discrete stream-monitoring locations rather than a regular spatial sampling grid. Interpolation could therefore imply phosphorus conditions between stations that were not directly supported by the available data.

### 5. Sample Code
Example SQL query used to join monitoring results with station coordinates and create the station-level dataset used for spatial analysis in QGIS.
```sql
CREATE TABLE phosphorus_by_station AS
SELECT
    m."Collection Site",
    s.LATITUDE,
    s.LONGITUDE,
    COUNT(*) AS detected_samples,
    AVG(m."Result Numeric") AS avg_total_phosphorus
FROM measurements AS m
JOIN stations AS s
    ON m."Collection Site" = s."Collection Site"
WHERE m."Result Call" = 'Detected'
GROUP BY
    m."Collection Site",
    s.LATITUDE,
    s.LONGITUDE
ORDER BY avg_total_phosphorus DESC;
### Results and Discussion 

### 1. Overall Phosphorus Distribution
*Table 1. Summary statistics for phosphorus concentrations in Ontario, 2025.*

| Statistic                           | Total phosphorus   | 
| Number of detected measurements     | 3744               | 
| Mean                                | 48.9 µg/L          | 
| Median                              | 14.2 µg/L          | 
| 25th percentile                     | 6.7 µg/L           | 
| 75th percentile                     | 32.8 µg/L          | 
| 90th percentile                     | 81.5 µg/L          | 
| 95th percentile                     | 149 µg/L           | 
| 99th percentile                     | 495.0 µg/L         | 
| Maximum                             | 10,300 µg/L        | 

<img src="images/phosphorus_distribution.png?raw=true"/>
*Figure 1. Distribution of total phosphorus measurements across Ontario, 2025.*
The large difference between the mean and median shows that a relatively small number of high measurements substantially increase the arithmetic mean. Half of all detected measurements were at or below 14.2 µg/L, while 95% were below 149 µg/L. In comparison, the maximum observed concentration was 10,300 µg/L. Several of the highest measurements were also observed repeatedly at the same monitoring locations rather than occurring as a single isolated value. For example, Bearbrook Station recorded values of 10,300 µg/L and 9,710 µg/L on separate sampling dates. Other stations also contained multiple measurements above 1,000 µg/L. These observations were retained because the earlier data-quality review did not provide a basis for treating them as erroneous. Because concentrations span several orders of magnitude, a logarithmic scale provides a more useful representation of the distribution than a conventional linear histogram.     


### 2. Station-Level Patterns                

Station-level statistics were calculated from the detected measurements using the median as the primary comparison metric. Median concentration was chosen because it is substantially less affected by the extreme upper tail than the mean. The highest median detected phosphorus concentrations occurred at monitoring stations 16002700102 and 16003000302, both of which had several measurements supporting their station-level medians rather than a single observation. Station 16002700102 had eight detected measurements and the highest median concentration in the analysis, while station 16003000302 had fourteen detected measurements and the second-highest median. The third-highest station, 04001000302, also remained substantially elevated relative to most of the remaining top-ranked sites. Below the first several stations, median concentrations declined considerably.

<img src="images/top_15_stations_median_phosphorus_with_counts.png?raw=true"/> 
*Figure 2. Top 15 stations with highest median phosphorus concentrations.*

Sample count is important when interpreting this ranking. For example, station 00000000000 appears among the highest median concentrations but has only one detected measurement. A median calculated from a single measurement should not be interpreted as evidence of consistently elevated phosphorus at that location. Including sample counts directly beside the bars helps distinguish stations supported by repeated monitoring from those represented by limited data. 

### 3. Temporal Patterns
Monthly median detected phosphorus concentrations varied throughout 2025. Median concentrations were highest early in the year, with February showing the largest monthly median at approximately 27 µg/L. Concentrations decreased substantially during April and May, increased again through June, July and August, and then declined during September and October before increasing moderately toward the end of the year.

<img src="images/monthly_median_phosphorus.png?raw=true"/> 
*Figure 3. Monthly median phosphorus concentrations across Ontario, 2025.*

The monthly sample-count analysis shows that monitoring coverage was not uniform throughout the year. Relatively few detected samples were available during January, February and December, while substantially more observations were available between April and November. May had the greatest number of detected measurements, with more than 450 observations.
<img src="images/monthly_sample_counts.png?raw=true"/> 
*Figure 3. Number of phosphorus measurements taken each month in 2025.*

These differences mean that the monthly concentration pattern should be interpreted cautiously. The analysis demonstrates variation among monthly detected measurements, but it does not by itself establish a seasonal phosphorus cycle. Differences in the stations sampled, sampling frequency, hydrological conditions and the proportion of measurements below detection limits could all affect the monthly results. A multi-year dataset would be more appropriate for testing whether the observed pattern is consistently seasonal.

### 4. Spatial Analysis
There were 449 monitoring sites with detected total phosphorus measurements in the analytical dataset. Of these, 443 sites were successfully matched to latitude and longitude records and mapped in QGIS.
<img src="images/Average Detected Total P at PWQMN Monitoring Stations, ON, 2025.png?raw=true"/> 
*Figure 4. Average total phosphorus measurements across PWQMN stations, 2025.*
The station map shows that PWQMN monitoring locations are concentrated heavily in southern Ontario, particularly around the Great Lakes and major populated and agricultural regions. A smaller number of monitoring locations occur across northern Ontario. Stations were symbolized according to their average detected total phosphorus concentration. The resulting map shows substantial spatial variation, with a relatively small number of stations exhibiting much higher average concentrations than the majority of the network. Six detected monitoring sites could not be matched to the coordinate dataset and were therefore excluded from the spatial visualization while remaining in the statistical analysis. These included Bearbrook Station, Inkerman and Scotch River, all of which contained some high phosphorus measurements. The QGIS map should therefore not be interpreted as containing every extreme concentration observed in the dataset.

### 5. Key Findings

The 2025 PWQMN data show a highly uneven distribution of detected total phosphorus concentrations. Most detected measurements were relatively low compared with a small upper tail of very high observations. The median concentration of 14.2 µg/L provides a better description of a typical detected measurement than the mean of 48.9 µg/L. Several monitoring stations showed substantially higher median phosphorus concentrations than the wider network, and some of these rankings were supported by repeated measurements rather than isolated observations. At the same time, sample counts varied considerably between stations, making it important to interpret locations with very few measurements cautiously. Detected phosphorus concentrations also varied throughout the year, but unequal monthly sample counts and the use of a single year of data prevent strong conclusions about seasonality. The spatial analysis similarly identifies where elevated station averages occurred but does not explain the causes of those concentrations.

### Limitations
This analysis covers only 2025, so it cannot determine whether the observed station or monthly patterns persist across multiple years. The primary statistical analysis includes only measurements classified as detected, meaning results below the analytical detection limit are not incorporated into the concentration summaries. The spatial analysis includes 443 of 449 stations with detected results because six monitoring sites could not be matched to coordinate records. Sampling intensity also varied considerably among stations and months. Station medians based on many observations are more informative about typical conditions than medians based on only one or two samples. Finally, the analysis identifies spatial and temporal patterns but does not establish their causes. Additional information on watershed characteristics, land use, precipitation, streamflow and agricultural activity would be required to investigate potential drivers of elevated phosphorus concentrations..

### References
Provincial (Stream) Water Quality Monitoring Network https://data.ontario.ca/en/dataset/provincial-stream-water-quality-monitoring-network 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
