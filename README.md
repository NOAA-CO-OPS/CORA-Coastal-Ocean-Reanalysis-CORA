# NOAA's Coastal Ocean Reanalysis (CORA)

[![Explore the Data](https://img.shields.io/badge/Explore_the_Data-CORA_on_AWS-0052cc?style=for-the-badge&logo=databricks&logoColor=white)](https://registry.opendata.aws/noaa-nos-cora/)

## Overview
NOAA's Coastal Ocean Reanalysis (CORA) provides re-analyzed wave and water level data and derived products for the U.S. coastlines. The data are produced by coupling ADCIRC (ADvanced CIRCulation Model) and SWAN (Simulating WAves Nearshore) and assimilating verified hourly water levels from the National Ocean Service’s Center of Operational Oceanographic Products & Services (CO-OPS). This repository provides Jupyter Notebooks that demonstrate working with this cloud-optimized, cloud-stored dataset.

## Usage
An `environment.yml` file is included to create an Anaconda environment in which to run the notebooks. To run the notebooks, you can use the following steps:
```bash
git clone https://github.com/NOAA-CO-OPS/CORA-Coastal-Ocean-ReAnalysis-CORA.git
cd CORA-Coastal-Ocean-ReAnalysis-CORA
conda env create -f environment.yml
conda activate cora
jupyter notebook
```

## Notebooks:
### 1.  CORA_accessing_data.ipynb
This notebook demonstrates access to CORA data. For a use case example, significant wave height data is extracted at the nearest model node to user provided geographic coordinates and the data is displayed in a time series plot.

### 2.  CORA_bounding_box.ipynb
This notebook demonstrates how CORA water level data can be extrated within a bounding box and plotted.

### 3.  CORA_compare_with_observations.ipynb
This notebook demonstrates how CORA water level data can be compared with observations at selected NOAA CO-OPS NWLON stations. 

### 4.  CORA_convert_datums.ipynb
This notebook demonstrates how to compute datums from CORA water level data using NOAA's Tidal Analysis Datum Calculator and convert the data to a different datum.

### 5.  CORA_plot_mesh.ipynb
This notebook demonstrates how to plot the native_grid mesh and topobathy underlying the CORA-GEC domain.

### 6.  CORA_visualize_water_level_maxima.ipynb
This notebook demonstrates how to extract the maximum water level over a selected time slice at every CORA-GEC native_grid node and plot it.

## Additional Contact Information:
  - Landing Page: https://tidesandcurrents.noaa.gov/cora.html
  - NOAA's [Center for Operational Oceanographic Products and Services](https://tidesandcurrents.noaa.gov/)
    User Services: tide.predictions@noaa.gov

##  Partners
  - NOAA’s [Office for Coast Management](https://coast.noaa.gov/) - Website: [Contact Form](https://coast.noaa.gov/contactform/)
  - NOAA’s [Integrated Ocean Observing System](https://ioos.noaa.gov/) - Email: noaa.ioos.webmaster@noaa.gov
  - [Tetratech’s RPS](https://www.rpsgroup.com/) 
  - University of North Carolina’s [Renaissance Computing Institute](https://renci.org/)
  - [University of Hawaii Sea Level Center](https://uhslc.soest.hawaii.edu/)

##  How to Cite 
   - NOAA's Coastal Ocean Reanalysis (CORA) Dataset was accessed on DATE from https://registry.opendata.aws/noaa-nos-cora using notebooks from https://github.com/NOAA-CO-OPS/CORA-Coastal-Ocean-Reanalysis-CORA

##  Additional Reference codes: 
   - [Dasher Triangular Meshes](https://github.com/holoviz/datashader/blob/f23de596f9adcb8188d48e6b163c36c913cd9912/examples/user_guide/6_Trimesh.ipynb#L11)
   - [Reproducable Notebooks for Maximum Water Levels](https://github.com/reproducible-notebooks/hurricane-ike-water-levels/tree/master)

## NOAA Open Source Disclaimer

This repository is a scientific product and is not official communication of the National Oceanic and Atmospheric Administration, or the United States Department of Commerce. All NOAA GitHub project code is provided on an 'as is' basis and the user assumes responsibility for its use. Any claims against the Department of Commerce or Department of Commerce bureaus stemming from the use of this GitHub project will be governed by all applicable Federal law. Any reference to specific commercial products, processes, or services by service mark, trademark, manufacturer, or otherwise, does not constitute or imply their endorsement, recommendation or favoring by the Department of Commerce. The Department of Commerce seal and logo, or the seal and logo of a DOC bureau, shall not be used in any manner to imply endorsement of any commercial product or activity by DOC or the United States Government.

## License

Software code created by U.S. Government employees is not subject to copyright in the United States (17 U.S.C. �105). The United States/Department of Commerce reserves all rights to seek and obtain copyright protection in countries other than the United States for Software authored in its entirety by the Department of Commerce. To this end, the Department of Commerce hereby grants to Recipient a royalty-free, nonexclusive license to use, copy, and create derivative works of the Software outside of the United States.
