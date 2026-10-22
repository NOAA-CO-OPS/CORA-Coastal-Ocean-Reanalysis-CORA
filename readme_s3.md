# NOAA's Coastal Ocean Reanalysis (CORA)

## Description
NOAA's Coastal Ocean Reanalysis (CORA) provides re-analyzed wave and water level data and derived products for the U.S. coastlines. The data are produced by coupling ADCIRC (ADvanced CIRCulation Model) and SWAN (Simulating WAves Nearshore) and assimilating verified hourly water levels from the National Ocean Service’s Center of Operational Oceanographic Products & Services (CO-OPS).

## Domains
Data are available over multiple geographic domains:

* **GEC (CONUS East and Gulf Coasts, and Caribbean):**
  * **Time span:** 1979 - 2022
  * **Size:** 40 TB
  * **Domain bounds:** Latitude: 5.8 to 45.8 ; Longitude: -98.0 to -53.8 ; 1983 Contiguous USA Albers projection (EPSG:5070)
  * **Spatial resolution:** Approx. 500 m at the coast, spatially variable

* **PAC (CONUS West Coast, S.E. Alaska, and Hawaii):**
  * **Time span:** 1979 - 2025
  * **Size:** 40 TB
  * **Domain bounds:** Latitude: 9.7 to 61.5 ; Longitude: -187.6 to -110.0 ; 1983 Contiguous USA Albers projection (EPSG:5070)
  * **Spatial resolution:** Approx. 200 m at the coast, spatially variable

## Change Log 
| Description | Datasets affected | Date | Data Version | 
| ------ | ---- | ------ | ------ |
| Initial GEC data release | GEC water levels and waves | 2022 | 0.9 |
| GEC water level datasets re-computed by filling gaps in historical observation record using machine learning | GEC water levels | 2023 | 1.0 |
| GEC water level datasets re-computed to correct long-term trends | GEC water levels| 2024 | 1.1 |
| GEC wave datasets re-computed to improve amplitude and phase during extreme events. Data were also re-chunked to improve cloud-based access speed. | GEC waves | Jun. 2026 | 1.1.1 |
| S3 bucket directories re-structured | No change to data, only to access paths | Jun. 2026 | 1.1.1 |
| Initial PAC data release | PAC water levels and waves | Oct. 2026 | 1.1.1 |
| Intake catalog files updated to include Pacific domain and remove version dependence | Intake files | Oct. 2026 | 1.1.1 |

> [!NOTE]
> v0.9 is now deprecated. GEC water levels utilized fewer observations for assimilation.
>
> v1.0 is now deprecated. GEC water levels exhibited erroneous long-term trends.
>
> v1.1 is now deprecated. GEC waves exhibted a low-bias and phase lag during extreme events due to a loosely-set convergence parameter.

## Known Issues
1. Data for the GEC domain cover the period 1979-2022, while data for the PAC domain extend three additional years (1979-2025). 
2. Cloud-based access to GEC water levels is slow due to a sub-optimal data chunking strategy.
3. GEC wave data (Hs and Tp) flatline during the months of Nov 2001 and Aug 2004. The source of the issue is corrupted meteroloigcal forcing files during those time periods.
4. PAC water levels show an energetic oscillation with a period of ~16 hr. This artefact is present throughout the domain for the entire time period, though is particularly energetic - with amplitudes on the order of 0.25-0.5 m - during Jan 2011 and Jan 2019.
5. PAC water levels diverge from observations by 10s of cm at multiple gauges during Jun-Dec 2009 and Mar-Aug 2019.

## Directory Structure and Dataset Description
```text
.
├── cora_gec/                                     # CONUS East and Gulf Coasts, and Caribbean domain
│   ├── native_grid/                              # ADCIRC-native irregular triangular grid
│   │   ├── water_levels/                         # Water level data relative to MSL
│   │   │   ├── fort.63_[TIME].[nc|zarr]          # Hourly water levels
│   │   │   └── maxele.63_[TIME].[nc|zarr]        # Yearly maximum water levels
│   │   ├── waves/                                # Bulk wave parameter data
│   │   │   ├── swan_DIR.63_[TIME]_[CHUNK].[nc|zarr] # Hourly mean wave direction
│   │   │   ├── swan_HS.63_[TIME]_[CHUNK].[nc|zarr]  # Hourly significant wave height
│   │   │   └── swan_TPS.63_[TIME]_[CHUNK].[nc|zarr] # Hourly spectral peak wave period
│   │   └── fort.14                               # Grid file describing mesh and bathymetry
│   │
│   ├── 500m_grid/                                # Re-interpolated 500 m resolution regular grid
│   │   ├── derived_products/                     # Data products derived from the data.
│   │   │   ├── daily_max/                        # Daily max water levels (by YYYYMM)
│   │   │   ├── monthly_statistics/               # Monthly tidal stats (by YYYY)
│   │   │   └── peaks_over_threshold/             # Water level exceedances above multiple thresholds.
│   │   └── water_levels/                         # Water level data relative to MSL
│   │       └── 500m_grid_zeta_YYYYMMDD.nc        # Hourly water levels by date
│   │
│   └── native_grid_shoreline/                    # Data along a shoreline extracted from the native_grid.
│       ├── derived_products/                     # Data products derived from the data.
│       │   ├── htf_predictions/                  # Daily liklihoods of high tide flooding from CO-OPS's Experimental High Resolution Monthly High Tide Flooding Outlook.
│       │   ├── beta.h5                           # Sea level anomaly adjustment factor
│       │   └── tide_constits.h5                  # UTide-derived tidal constituents
│       └── water_levels/                         # Water level data relative to MSL
│           └── [STATE].nc                        # State-level hourly water levels
│           └── [STATE]_mm_[CHUNK].zarr           # State-level hourly water levels in integer mm units.
│
├── cora_pac/                                     # CONUS West Coast, S.E. Alaska, and Hawaii domain
│   └── native_grid/                              # ADCIRC-native irregular triangular grid
│       ├── water_levels/                         # Water level data relative to MSL
│       │   ├── fort.63_[TIME]_[CHUNK].[nc|zarr]  # Hourly water levels
│       │   └── maxele.63_[TIME].[nc|zarr]        # Yearly maximum water levels
│       └── waves/                                # Bulk wave parameter data
│           ├── swan_DIR.63_[TIME]_[CHUNK].[nc|zarr] # Hourly mean wave direction
│           ├── swan_HS.63_[TIME]_[CHUNK].[nc|zarr]  # Hourly significant wave height
│           └── swan_TPS.63_[TIME]_[CHUNK].[nc|zarr] # Hourly spectral peak wave period
│
├── CORA_V1.1_intake.yml                          # Inake catalog to be used with the intake Python package for accessing cloud-optimized datasets with zarr v2. See the GitHub repo for usage examples: https://github.com/NOAA-CO-OPS/CORA-Coastal-Ocean-Reanalysis-CORA
└── CORA_V1.1_intake_zarr3.yml                    # Inake catalog to be used with the intake Python package for accessing cloud-optimized datasets with zarr v3 or higher. See the GitHub repo for usage examples: https://github.com/NOAA-CO-OPS/CORA-Coastal-Ocean-Reanalysis-CORA

*Note: [TIME] represents YYYY, YYYY-YYYY+3, or full spans. [CHUNK] represents _map or _timeseries.*
```

## References and Further Documentation
* [CORA homepage](https://tidesandcurrents.noaa.gov/cora.html#usecase) by NOAA's Center for Operational Oceanographic Products and Services
* [CORA user guide](https://tidesandcurrents.noaa.gov/cora/data/Accessing_and_Downloading_Coastal_Ocean_Reanalysis_Data_User_Guide.pdf) by NOAA's Center for Operational Oceanographic Products and Services
* [CORA GitHub](https://github.com/NOAA-CO-OPS/CORA-Coastal-Ocean-Reanalysis-CORA) including tutorials for working with the data  by NOAA's Center for Operational Oceanographic Products and Services
* [NOAA Technical Report NOS CO-OPS 108](https://repository.library.noaa.gov/view/noaa/66833): NOAA’s Coastal Ocean Reanalysis: Gulf of Mexico, Atlantic, and Caribbean (January 2025) by Keeney, Analise; Dusek, Gregory; Callahan, John; Ratcliff, John; Jima, Tigist; Brooks, William; Marcy, Doug; Blanton, Brian; Tilson, Jeffrey; Asher, Taylor G.; Leuttich, Richard A.; Widlansky, Matthew J.; Rose, Linta; Morse, Cheryl; Haddad, Jana; & Waring, Blake
* [Assessment of water levels from 43 years of NOAA’s Coastal Ocean Reanalysis (CORA) for the Gulf of Mexico and East Coasts](https://doi.org/10.3389/fmars.2024.1381228) by Rose, Linta; Widlansky, Matthew J.; Feng, Xue; Thompson, Thompson; Asher, Taylor G.; Dusek, Gregory; Blanton, Blanton; Luettich, Richard A. Jr.; Callahan, John; Brooks, William; Keeney, Analise; Haddad, Jana; Sweet, William; Genz, Ayesha; Hovenga, Paige; Marra, John & Tilson, Jeffrey
* [Filling the gaps between tide gauges: Demonstrating high-resolution seasonal high tide flooding predictions using NOAA’s Coastal Ocean Reanalysis](https://doi.org/10.1371/journal.pone.0344695) by Conlin, Matthew; Dusek, Gregory; Ratcliff, John; Callahan, John; Kavanaugh, Karen; Brooks, William; Waring, Blake; Keeney, Analise; Sweet, William, & Widlansky, Matthew

## How to cite
NOAA's Coastal Ocean Reanalysis (CORA) was accessed on `DATE` from https://registry.opendata.aws/noaa-nos-cora.

## License
NOAA data disseminated through NODD are open to the public and can be used as desired.NOAA makes data openly available to ensure maximum use of our data, and to spur and encourage exploration and innovation throughout the industry. NOAA requests attribution for the use or dissemination of unaltered NOAA data. However, it is not permissible to state or imply endorsement by or affiliation with NOAA. If you modify NOAA data, you may not state or imply that it is original, unaltered NOAA data.

## Contact
For questions regarding data content or quality, email CO-OPS.UserServices@noaa.gov This data is made available to the public through the NOAA Open Data Dissemination (NODD) Program. For questions regarding this program, email nodd@noaa.gov. We also seek to identify case studies on how NOAA data is being used and will be featuring those stories in joint publications and in upcoming events. If you are interested in seeing your story highlighted, please share it with the NOAA NODD team at nodd@noaa.gov.


