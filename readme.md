# Downscaling Precipitation Extremes

This code was developed for the DOE SBIR "UVDAT" (Urban Visualization and Data Analysis Toolkit) with Northeastern University as the subcontractor to Kitware, Inc. This code is publicly available under the MIT license. Written by August Posch, Northeastern University, February-March 2025.

## Instructions
1. Download these three LOCA products into /data/LOCA_downscaled/
  - pr.CESM2-LENS.ssp370.r1i1p1f1.2015-2044.LOCA_16thdeg_v20240915.n_east.nc
  - pr.CESM2-LENS.ssp370.r1i1p1f1.2045-2074.LOCA_16thdeg_v20240915.n_east.nc
  - pr.CESM2-LENS.ssp370.r1i1p1f1.2075-2100.LOCA_16thdeg_v20240915.n_east.nc
  - They can be found here: https://cirrus.ucsd.edu/~pierce/LOCA2/CONUS_regions_split/CESM2-LENS/n_east/0p0625deg/r1i1p1f1/ssp370/pr/
2. Run *get_all_latlons.ipynb*
  - This notebook reads in a downscaled product and saves the latitudes and longitudes of its gridcells as .npy files.
3. Run *visualize_LOCA_HUC_choose_gridcells.ipynb*
  - This notebook reads in HUC-10 watershed shapefile, latitudes and longitudes of LOCA gridcell centers, and a land shapefile. It identifies the gridcells of interest and saves them as a .npy file. Along the way it saves two figures showing a map with gridcells of interest starred as well as the watersheds and land.
4. Run *postproc_downsc_extr.ipynb*
  - This notebook takes in downscaled gridded projections and a set of gridcells of interest, and returns extreme levels for two time periods of interest: midcentury (2031-2050) and end of century (2081-2100). Along the way it also saves the predicted daily precipitation time series and the associated annual precipitation max time series.

## Information

This version provides downscaled precipitation extreme value projections for the Greater Boston Area. For the UVDAT project, the extreme values serve as forcings to simulate flooding with urban rail disruptions and recovery.

The current version postprocesses the LOCA downscaled product for Boston urban rail watersheds and time periods and return periods of interest. Already, the climate model, ensemble member, and emissions scenario can be swapped out to obtain new estimates. In future versions, the user will select any location and run a custom AI downscaling model.

- Downscaling technique:
  - Climate Model: CESM2-LENS
  - Ensemble member: r1i1p1f1
  - Emissions scenario: 370
  - Downscaling method: LOCA (Localized Constructed Analogs) to daily 1/16 degree
  - Postprocessing: NU aggregated to Boston urban rail watersheds and fitted GEV distribution
- Time periods: 2031-2050 and 2081-2100
- Return periods: 100 years (1% annual probability) and 25 years (4% annual probability)

- Watersheds
  - Five watersheds (HUC-10):
    - Lower Charles River
    - Neponset River
    - Boston Harbor-Massachusetts Bay*
    - Mystic River
    - Hingham Bay
  - *consists of many hydrologically independent pieces - we obtained precipitation forcing based on only the parts relevant to the urban rail network.
  - The code is currently hard-coded to use these watersheds. Ability to select watersheds for any city is coming soon.

- Results
  - 2031-2050, 25-year: 80 mm
  - 2031-2050, 100-year: 85 mm
  - 2081-2100, 25-year: 115 mm
  - 2081-2100, 100-year: 135 mm
  - All of the above are 1-day duration events.

## Citations
(Need to formalize the citations.)

Datasets to cite:
- Community Earth System Model 2 (CESM2) - climate model
- Localized Constructed Analogs (LOCA) - downscaled product
- Livneh precipitation dataset
- HUC-10 watershed shapefiles