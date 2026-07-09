## Statistical Climate Downscaling of Monthly Air Temperature in Greenwood, Nova Scotia

### Project description 
Global climate models and reanalysis products provide valuable climate information but often operate at spatial resolutions that are too coarse for local-scale applications. Statistical downscaling is a widely used technique that establishes empirical relationships between large-scale climate variables and local observations to generate more accurate site-specific climate estimates.
In this project I developed and evaluated a statistical downscaling model to estimate monthly air temperatures in Greenwood, Nova Scotia using ERA5 reanalysis data and Environment and Climate Change Canada weather station observations. A multiple linear regression model was trained using historical observations and validated using an independent holdout dataset to assess predictive performance.

### Objectives
- Develop a statistical relationship between ERA5 reanalysis temperatures and local station observations.
- Produce downscaled monthly temperature estimates for Greenwood, Nova Scotia.
- Evaluate model performance using leave-one-out cross-validation.
- Validate the model using an independent holdout period.
- Compare model performance against the original ERA5 dataset.

### Study Area
Greenwood is located in the Annapolis Valley of Nova Scotia and experiences a humid continental climate characterized by warm summers and cold winters. The surrounding topography and proximity to the Bay of Fundy influence local temperature patterns that are not always captured by coarse-resolution climate datasets. Because of these local influences, Greenwood provides an appropriate case study for evaluating statistical climate downscaling techniques.

### Methodology

### 1. Data Acquisition

Monthly ERA5 reanalysis air temperature data were downloaded in NetCDF format. Historical weather observations for Greenwood, Nova Scotia were obtained from Environment and Climate Change Canada. The ERA5 dataset was subset to the Greenwood grid cell and converted from Kelvin to degrees Celsius before analysis.

### 2. Data Preparation

Both datasets were formatted to a common monthly time series. Observation records and ERA5 predictor data were filtered to the 1981–2010 calibration period and matched by year and month to ensure temporal consistency.

### 3. Statistical Downscaling Model

A multiple linear regression model was developed using ERA5 monthly temperatures as the predictor and Greenwood station observations as the predictand.

To improve model robustness, a leave-one-out cross-validation procedure was implemented while accounting for temporal persistence through autocorrelation analysis. For each iteration, neighboring observations within the persistence window were excluded before estimating regression coefficients.

### 4. Model Calibration

Regression coefficients were calculated for each validation iteration before being averaged to produce the final calibration model. These mean coefficients were then applied to the complete ERA5 record to generate fitted temperature estimates.

### 5. Model Evaluation

Model performance was assessed using:

Root Mean Square Error (RMSE)
Correlation coefficient
Skill Score relative to the original ERA5 data

These metrics quantified improvements achieved through statistical downscaling over the raw reanalysis temperatures.

### 6. Holdout Validation

The calibrated model was applied to an independent dataset spanning 2011–2017 to evaluate predictive performance outside the training period. Downscaled temperatures were compared directly with observed station temperatures to assess the model's ability to generalize to unseen data.

### Sample Code 
Extract ERA5 Temperature
```data_variable <- ncvar_get(
  nc_era,
  "t2m",
  start = c(min(lon_indices), min(lat_indices), 1),
  count = c(length(lon_indices), length(lat_indices), -1)
)

tempc <- data_variable - 273.15
```
data_variable <- ncvar_get(
  nc_era,
  "t2m",
  start = c(min(lon_indices), min(lat_indices), 1),
  count = c(length(lon_indices), length(lat_indices), -1)
)

tempc <- data_variable - 273.15
Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
