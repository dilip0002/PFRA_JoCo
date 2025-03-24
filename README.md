# PFRA - Johnson County, KS 
The PFRA approach used for Johnson County, KS utilizes the approach that was developed in [pfra-hydromet](https://dewberry.github.io/pfra-hydromet/)

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Dewberry/pfra-hydromet/master)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

---

# Description

This project utilizes the scripts developed by __pfra-hydromet__. This project uses tools for developing __pluvial__ (excess rainfall) scenarios for input to hydraulic models.

### Pluvial:
These tools ([jupyter notebooks](https://jupyter.org/) ) ingest data from the NOAA Hydrometeorological Design Studies Center ([HDSC](https://www.nws.noaa.gov/oh/hdsc/index.html)) and return unique, weighted excess rainfall events suitable for use in 2D hydraulic *rain-on-grid* models. This approach relies on:

  1. Meteorological data
  2. Random sampling
  3. Hydrologic transform
   

---

## Contents

### __notebooks__:

#### __pluvial__:
The notebooks that are used for this PFRA - Johnson County, KS project are explained below. These are the only notebooks that are used in preparing the input data required for probabilistic modeling.

- [__PrecipTable__](PrecipTable.ipynb): Retrieve NOAA Atlas 14 precipitation statisics for an Area of Interest (AOI).

- [__Hydro4_samples__](Hydro4_samples.ipynb): Prepares the shape of the 4 sampled Atlas 14 nested hyetographs.

- [__CN_hyetographs_updated__](CN_hyetographs_updated.ipynb): Creates a stratified sample of rainfall hyetographs given rainfall and maximum potential retention distributions. For each event and corresponding return interval, the event weight, CN values for wet and dry conditions, and rainfall values are calculated. This script is developed by modifying the EventsTable_Stratified notebook to produce the desirable outputs. The detailed workflow for this approach is described in the "Readme" file located under the "notebooks/pluvial" folder.

- [__EventsTable_Stratified__](EventsTable_Stratified.ipynb): Calculates a stratified sample of runoff events given rainfall and maximum potential retention distributions. For each each event and corresponding return interval, the event weight, runoff value, maximum potential retention value, and rainfall value are calculated. This (EventsTable_Stratified) notebook is the reference notebook that is used to create CN_hyetographs_updated notebook.





#### __DataRepository__:

- __Temporal_Distributions__: Folder containing csv files of temporal distributions of observed rainfall patterns broken down by volume, region, duration, and quartile [NOAA Published](https://hdsc.nws.noaa.gov/hdsc/pfds/pfds_temporal.html). Note that the original data were compiled into csv's for uniform formatting.

- __Temporal_Distributions_Plots__: Folder containing a Jupyter Notebook for each NOAA Atlas 14 volume with the plotted temporal distributions for each region, duration, and quartile.

- `NEH630_Table_10_1.json`: A formatted copy of Table 10-1 from the National Engineering Handbook [Chapter 10].(https://www.wcc.nrcs.usda.gov/ftpref/wntsc/H&H/NEHhydrology/ch10.pdf.) which lists the CN values for dry and wet antecedent moisture conditions.

- `NOAA_Atlas_Volume_Codes.json`: Metadata that maps the NOAA Atlas 14 volume number to the volume code. [Source](https://hdsc.nws.noaa.gov/hdsc/pfds/pfds_gis.html)

- `NOAA_Temporal_Areas_US.geojson`: geojson file containing the vector ploygons of the NOAA Atlas 14 temporal distribution areas. This file was constructed using the individual vector ploygons for each volume. [Source](https://hdsc.nws.noaa.gov/hdsc/pfds/pfds_temporal.html)

- `Temporal_Distribution_Data_Map.json`: Metadata used to extract the temporal distribution data from the csv files saved within the __Temporal_Distributions__ folder.

- `Temporal_Quartile_Ranks.xlsx`: Excel Workbook that contains the percentage of precipitation events whose temporal distributions are represented by those in each quartile of a specific volume/region/duration. [Source](https://www.nws.noaa.gov/oh/hdsc/currentpf.html)


*The ([CN Method](https://www.nrcs.usda.gov/Internet/FSE_DOCUMENTS/stelprdb1044171.pdf)) is currently the only transform method in use for this project. Other transforms are available and can be adopted into the tool with minor modifications.

---

### Documentation

This project utilizes the scripts developed by PFRA-hydromet. The complete documentation for PFRA-hydromet can be found in [read the docs](https://dewberry.github.io/pfra-hydromet/about/).
