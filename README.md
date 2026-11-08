# Colossus remote sensing study

This ES4304 project examines daytime land-surface temperature and vegetation around the Colossus facility in Memphis, Tennessee, with Brownsville as a provisional regional comparison. The repository also preserves the original ECOSTRESS nighttime analysis code.

## Files

| File | Purpose |
| --- | --- |
| [Colossus_Brownsville_Study_Area_Colab.ipynb](Colossus_Brownsville_Study_Area_Colab.ipynb) | Interactive satellite maps showing study zones and approximate site locations. |
| [NDVI_Colossus_Brownsville_Colab.ipynb](NDVI_Colossus_Brownsville_Colab.ipynb) | Sentinel-2 summer NDVI before, after and difference maps, with Google Drive exports. |
| [Colossus_Temperature_Maps.ipynb](Colossus_Temperature_Maps.ipynb) | Landsat summer temperature composites, spatial changes and zone summaries. |
| [Colossus_Timeline_2020_to_2026.ipynb](Colossus_Timeline_2020_to_2026.ipynb) | Monthly and yearly core/background temperature summaries, plus an optional summer map. |
| [Colossus_vs_Brownsville.ipynb](Colossus_vs_Brownsville.ipynb) | Daytime temperatures in the two locations' 500 m–2 km rings on matching observation dates. |
| [ECOSTRESS_Nighttime_Code.txt](ECOSTRESS_Nighttime_Code.txt) | Original Earth Engine JavaScript and MATLAB code for nighttime analysis, supplied together as reference code. |

## Run the notebooks in Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Choose **File → Open notebook → GitHub**, paste this repository's URL and select a notebook. Alternatively, download an `.ipynb` file and upload it to Colab.
3. Run the cells in order, starting with the dependency installation cell.
4. Complete the Google authentication prompt. The account must have Earth Engine access and permission to use the configured Google Cloud project.
5. If running under another account, replace the included project ID, `ee-project-ntu-512407`, with your own Earth Engine-enabled project ID.
6. Follow the notebook's export cells. Some results download as ZIP files; NDVI raster exports finish separately in Google Drive.

Interactive maps require execution in Colab; GitHub displays a static notebook preview. Internet access and an authenticated Earth Engine session are required. Colab runtime files are temporary, so download results before ending the session. Avoid running duplicate processing cells simultaneously.

## Data and comparison periods

- **Daytime temperature:** Landsat 8/9 Collection 2 Level-2, using the `ST_B10` surface-temperature band. Values are converted to degrees Celsius.
- **Vegetation:** Sentinel-2 Level-2A harmonized surface reflectance. NDVI is calculated as `(B8 - B4) / (B8 + B4)` and is dimensionless.
- **Nighttime reference code:** ECOSTRESS tiled Level-2 land-surface temperature and emissivity.

The main summer comparison uses **June–August 2022–2023** before startup and **June–August 2025–2026** after startup. The transition year, 2024, is excluded from that comparison.

The local timeline runs from January 2020 through the requested cutoff of 29 October 2026. The city comparison uses a window through 31 October 2026. The latest usable observation can be earlier than the requested cutoff. The optional map within the timeline notebook compares **2023 with 2025**, rather than the main four-summer comparison.

## How the summaries differ

The local timeline compares the mean surface temperature within 500 m of Colossus with the mean temperature in its 3–5 km background ring. The regional comparison uses equal-sized 500 m–2 km rings at Colossus and Brownsville and retains matching observation dates.

Temperature maps first calculate each pixel's median for each summer, then average the two summers in each period with equal year weighting. Pixels must meet the minimum observation requirement in all four summers. The NDVI script instead calculates one median across all images in each two-summer period. Maps and date-based temperature summaries therefore use different weighting and should not be treated as interchangeable estimates.

## Current limitations and reproducibility notes

- Surface temperature is not air temperature. These observations represent clear-sky satellite overpasses, not continuous daily monitoring.
- Before/after differences do not establish that data-centre operations caused the change. Weather, land cover, construction, nearby heat sources and varying valid-pixel coverage can affect the results.
- Brownsville is a provisional comparison, not a verified AI-free or fully matched industrial control.
- **NDVI cloud-mask limitation:** the original QA60 masking method is retained. QA60 is unavailable during the 2022–2023 summers in this collection, so before-period and difference outputs may be blank. The published script should not be assumed to reproduce valid before-period maps without addressing this limitation.
- **Monthly threshold mismatch:** the supplied local timeline code uses `MIN_VALID_DAYS_PER_MONTH = 1`, although some accompanying descriptions state two dates. The Brownsville comparison uses two. Interpret outputs according to the code setting used for the run.
- **Study-centre mismatch:** the study-area illustration uses latitude 35.0565, longitude −90.1455; the temperature and NDVI analyses use latitude 35.06000, longitude −90.15639. These locations have not been harmonized. Verify the intended centre before comparing zone boundaries across figures.
- Google sign-in, project permissions and Earth Engine memory or request limits can prevent execution. Inclusion in this repository does not certify that every notebook completes successfully in a new runtime.

## Nighttime reference code

`ECOSTRESS_Nighttime_Code.txt` contains two separate programs. Paste the JavaScript section into the [Earth Engine Code Editor](https://code.earthengine.google.com/). Save the MATLAB section as an `.m` file and run it in MATLAB with its required CSV inputs. The combined text file is not directly executable in Colab.

The attempted Colab nighttime workflow encountered an Earth Engine memory-limit error and has not been confirmed to complete successfully. The original code is included unchanged for transparency.

The MATLAB section expects `Night_scenes_Colossus.csv` and `Night_scenes_Brownsville.csv`, while the included JavaScript exports different, already summarized tables. The input files and aggregation steps need to be reconciled before treating these two sections as a single reproducible pipeline.

## Data sources

- [Landsat 8 Collection 2 Level-2](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC08_C02_T1_L2)
- [Landsat 9 Collection 2 Level-2](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2)
- [Sentinel-2 harmonized surface reflectance](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED)
- [ECOSTRESS Level-2 tiled surface temperature](https://developers.google.com/earth-engine/datasets/catalog/NASA_ECOSTRESS_L2T_LSTE_V2)

This repository supports a student research project. Results should be interpreted alongside the report's methods, observation counts and limitations.
