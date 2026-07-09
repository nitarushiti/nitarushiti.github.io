## This can be your internal website page / project page

**Project Overview:** Urban heat islands occur when built-up areas absorb and retain more heat than surrounding natural environments, resulting in elevated surface temperatures. These temperature differences can contribute to increased energy consumption, reduced outdoor comfort, and greater heat-related health risks. This project maps urban heat island patterns across Kitchener, Ontario, using satellite-derived land surface temperature and municipal land cover data to identify areas that may benefit from increased vegetation and other cooling strategies.

### 1. Objectives

- Map land surface temperature across Kitchener. 
- Identify urban heat hotspot locations. 
- Compare hotspot distribution with tree canopy and impervious surfaces. 
- Identify potential priority areas for urban cooling initiatives.

```javascript
if (isAwesome){
  return true
}
```

### 2. Study Area
Kitchener is the largest city in the Region of Waterloo, located in southwestern Ontario, Canada. The city has a population of approximately 257,000 residents (2021 Census) and has experienced significant urban growth driven by residential, commercial, and industrial development. Despite this expansion, Kitchener maintains an extensive network of parks, natural areas, and trails that are anchored by the Grand River, which flows through the eastern portion of the city and forms the backbone of its natural heritage system. The city's landscape consists of a mixture of densely developed urban neighbourhoods, transportation corridors, industrial lands, agricultural areas, and protected green spaces. Kitchener experiences a humid continental climate, characterized by warm, humid summers and cold winters.
<img src="images/Studyarea.jpg?raw=true"/>
```javascript
if (isAwesome){
  return true
}
```

### 3. Data

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Methodology

1. Data Acquisition
Landsat 8 Collection 2 Level-2 Surface Temperature (ST_B10) imagery acquired on July 7, 2025 was downloaded from the USGS EarthExplorer platform. An image with minimal cloud cover was selected to reduce the influence of atmospheric interference on surface temperature measurements. Municipal boundary and land cover datasets for the City of Kitchener were obtained from the Kitchener GeoHub Portal.
2. Land Surface Temperature Processing
The surface temperature raster was converted from scaled Kelvin values to degrees Celsius using the Raster Calculator tool in ArcGIS Pro with the following equation:
("ST_B10"×0.00341802+149.0)-273.15
3. Study Area Extraction
The Clip Raster tool was used to clip the land surface temperature raster to the Kitchener municipal boundary, ensuring that all subsequent analyses were limited to the study area. 
4. Temperature Visualization
The clipped temperature raster was symbolized using a blue-to-red colour ramp to represent cooler and warmer surface temperatures across the city. 
5. Hotspot Classification
Surface temperatures were classified into three categories using the Raster Calculator: 
	Normal: Below the city-wide mean temperature 
	Hotspot: Greater than the mean temperature plus one standard deviation 
	Extreme Hotspot: Greater than 48°C 
6. Hotspot Polygon Creation
The classified hotspot raster was converted to polygon features using the Raster to Polygon tool, allowing hotspot areas to be visualized and compared with other vector datasets. 
7. Land Cover Comparison
The hotspot polygons were compared with the City of Kitchener land cover dataset to visually assess the relationship between elevated surface temperatures and different land cover types. 
8. Map Production
Final cartographic layouts were produced in ArcGIS Pro, including maps of land surface temperature, identified hotspots, and land cover, with legends, scale bars, north arrows, and locator maps to support interpretation.
 
### 4. Results and Discussion 
Figure 1 illustrates the spatial distribution of land surface temperatures across the City of Kitchener on July 7, 2025, derived from Landsat 8 Collection 2 Level-2 Surface Temperature data. The highest surface temperatures are concentrated primarily within the central and southern portions of the city, with additional isolated hotspots scattered throughout the urban area. The observed land surface temperatures range from approximately 27°C to 57°C, representing a difference of nearly 30°C across the study area. This considerable variation highlights the strong influence of land cover on surface heating within the urban environment. While the map represents surface temperature rather than air temperature, the spatial patterns provide valuable insight into the distribution of urban heat islands across Kitchener. <img src="images/Surface_temp.png?raw=true"/> Figure 2a identifies areas classified as high and very high temperature hotspots using a threshold based on the mean land surface temperature plus one standard deviation. These hotspot polygons represent locations where surface temperatures are substantially higher than the city-wide average and therefore indicate potential urban heat island (UHI) effects. Although hotspots are dispersed throughout the municipality, they are not randomly distributed. Instead, they form clusters that suggest land cover characteristics play an important role in influencing local surface temperatures. Figure 2b illustrates the distribution of major land cover types across Kitchener, including tree canopy, grass and shrub vegetation, bare earth, water, buildings, roads, and other paved surfaces. Approximately 57% of the identified hotspots overlap areas characterized by a high proportion of buildings, roads, and other impervious surfaces. These materials have relatively low albedo and high heat storage capacity, allowing them to absorb solar radiation during the day and re-radiate heat over extended periods. As a result, these developed areas consistently exhibit higher surface temperatures than surrounding landscapes. <img src="images/land_cover_hotspots.png?raw=true"/> 
Conversely, relatively few hotspots occur within areas dominated by tree canopy (11.5%) or grasslands (26%). Vegetated areas provide shade and promote evaporative cooling through evapotranspiration, while water bodies possess a higher heat capacity that moderates surface temperatures. The Grand River corridor and larger park systems are particularly evident as cooler zones throughout the study area.
Overall, the comparison suggests that land cover is a major factor influencing the spatial distribution of surface temperatures within Kitchener and identifies several developed areas where increased tree planting or other green infrastructure initiatives could help reduce urban heat island intensity.


For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
