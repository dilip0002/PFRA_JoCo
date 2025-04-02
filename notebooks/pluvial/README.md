# Description
These tools ([jupyter notebooks](https://jupyter.org/) ) ingest data from the NOAA Hydrometeorological Design Studies Center ([HDSC](https://www.nws.noaa.gov/oh/hdsc/index.html)) and return unique, weighted excess rainfall events suitable for use in 2D hydraulic *rain-on-grid* models. This approach relies on:

  1. Meteorological data
  2. Random sampling
  3. Hydrologic transform
  4. Convolution algorithm for grouping

---

## Contents

The notebooks that are used for this PFRA - Johnson County, KS project are explained below. These are the only notebooks that are used in preparing the input data required for probabilistic modeling.

- [__PrecipTable__](PrecipTable.ipynb): Retrieve NOAA Atlas 14 precipitation statisics for an Area of Interest (AOI).

- [__Hydro4_samples__](Hydro4_samples.ipynb): Prepares the shape of the 4 sampled Atlas 14 nested hyetographs.

- [__CN_hyetographs_updated__](CN_hyetographs_updated.ipynb): Creates a stratified sample of rainfall hyetographs given rainfall and maximum potential retention distributions. For each event and corresponding return interval, the event weight, CN values for wet and dry conditions, and rainfall values are calculated. This script is developed by modifying the [__EventsTable_Stratified__](EventsTable_Stratified.ipynb) notebook to produce the desirable outputs. The detailed workflow for this approach is described in the "Readme" file located under the "notebooks/pluvial" folder.

- [__EventsTable_Stratified__](EventsTable_Stratified.ipynb): Calculates a stratified sample of runoff events given rainfall and maximum potential retention distributions. For each each event and corresponding return interval, the event weight, runoff value, maximum potential retention value, and rainfall value are calculated. This (EventsTable_Stratified) notebook is the reference notebook that is used to create CN_hyetographs_updated notebook.


*The ([CN Method](https://www.nrcs.usda.gov/Internet/FSE_DOCUMENTS/stelprdb1044171.pdf)) is currently the only transform method in use for this project. Other transforms are available and can be adopted into the tool with minor modifications.

---


## Workflow

1. Run [PrecipTable](PrecipTable.ipynb) in order to calculate the area-averaged precipitation frequency table for the specified durations as well as to determine the NOAA Atlas 14 volume and region.
    ```
      Inputs:
        1. A vector polygon of the area of interest, i.e. the pluvial domain.
        2. Optional/as needed: 
            - Precipitation event durations; the duration used in this project is 12 hour.
            - The polygon's projection as a string if it cannot be determined automatically.
      Outputs:
        1. A spreadsheet with the area-averaged precipitation frequency table for each duration, along with the NOAA Atlas 14 volume and region numbers.
    ```
    
2. Run [Hydro4_samples](Hydro4_sample.ipynb) which prepares the shape of the 4 sampled NOAA Atlas 14 temporal hyetographs.

3. Run [CN_hyetographs_updated](CN_hyetographs_updated.ipynb) in order to calculate the rainfall hyetographs for all the events (return intervals), and the curve number (CN) values for wet and dry condition for all the events.
     ```
        Inputs:
          1. PrecipTable.xlsx from step 1, which contains precipitation frequency tables and the NOAA Atlas 14 volume and region number. Note that the volume and region number may also be entered manually.
          2. 4 sampled NOAA Atlas 14 temporal hyetographs from step 2
          3. Shapefile containing the CN values for the study area
          4. Storm durations
          5. Filenames and paths for outputs
          6. EventsTable.ipynb
  
        Outputs:
          1. Rainfall hyetographs for each event
          2. Event weights
          3. CN values for wet and dry conditions for each event
      ```



## Description of the modified approach
### Modification of Script:

The [EventsTable_Stratified](EventsTable_Stratified.ipynb) notebook is modified to create [CN_hyetographs_updated](CN_hyetographs_updated.ipynb) notebook. The approach used while modifying the script to fit the project's requirement are explained below.

- The rainfall is developed using Hydrology 2. And the rainfall hyetographs are developed using the NOAA Atlas 14 temporal distributions (four different quartiles).

- To account for the spatial variability of the CNs within the basin, we calculate the CNs for wet and dry condition for each unique CN values within the basin.

- In Hydrology 3, for each return period, the original "Events Table Stratified" script calculates the average max potential retention value for upper 50% and lower 50%. These avg. S values are then converted to CN values for wet and dry conditions.
The conversion is done using the formula: S = 1000/CN - 10

- These CN values for wet and dry conditions are then provided as an input to the HEC-RAS model. This way the RAS model acccounts for the spatial variability of the losses associated with different land use conditions throughout the basin.

- All the other calculation steps are same as described in the original [EventsTable_Stratified](EventsTable_Stratified.ipynb) script.

### CN calculation approach
- Steps involved:

    1. First, all the unique Curve Number (CN) values are extracted from the basin shapefile (We use a gridded CN value across the study area). This is done such that we can account for the spatial variability of the CNs within a basin. Eventually, we will have wet and dry condition CN values for all the unique CNs within the basin.
    2. For each unique CN value, the similar process as in Hydrology 3 is conducted. Here, for each CN, it extracts wet and dry soil moisture condition, and then fits into a beta distribution.
    3. Then, the runoff data is fitted into a GEV distribution.
    4. For each Return Period, the average maximum potential retention (S) values are calculated for lower 50% and upper 50%.
    5. These two "S" values for each Return Period are then converted into CN by using the formula: S = 1000/CN - 10
    6. In this way, we will have two sets of CN values for wet and dry conditions for each of the Return Periods. The CN values thus obtained are spatially varied instead of a single average CN value for the entire watershed.

