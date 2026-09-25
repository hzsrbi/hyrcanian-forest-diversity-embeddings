# Geospatial foundation model embeddings for Hyrcanian forest diversity

Companion code for **“Geospatial Foundation Models for forest diversity mapping in structurally complex, species-rich forests”** (manuscript version 6; publication details and DOI to be added when available). The project compares 2023 AlphaEarth Foundation (AEF) and TESSERA embeddings with handcrafted Sentinel-1 and Sentinel-2 predictors for Shannon diversity, species richness, and the standard deviation of tree diameter at breast height (DBH).

**Repository status: methodological draft.** The notebooks are organized for public release and pass Python syntax checks, but have **not** been executed end to end against the restricted inventory. Differences between the supplied extraction code and the manuscript are documented in [method alignment](docs/method-alignment.md). Do not describe this repository as an exact reproduction of the manuscript's tables until the exported bands, plot aggregation, spatial folds, and results have been verified.

## Contents

| File | Purpose |
| --- | --- |
| [`notebooks/01_earth_engine_extraction.ipynb`](notebooks/01_earth_engine_extraction.ipynb) | Load private plot locations, extract Sentinel-1, Sentinel-2, RaoQ, and AEF features with Google Earth Engine; merge with separately acquired TESSERA plot embeddings. |
| [`notebooks/02_spatial_dnn_analysis.ipynb`](notebooks/02_spatial_dnn_analysis.ipynb) | Fit and assess 12 predictor sets and three targets with zone-held-out deep neural networks. |
| [`docs/data-contract.md`](docs/data-contract.md) | Input files, required fields, export names, and sharing restrictions. |
| [`docs/method-alignment.md`](docs/method-alignment.md) | Checks needed to align this implementation with the manuscript. |

## Getting started

1. Create a private Python environment and install the packages listed in [`requirements.txt`](requirements.txt). Record the exact installed versions in a lockfile for any reported run. You need an Earth Engine-enabled Google Cloud project and permission to access the inventory and TESSERA tiles.
2. Supply private inputs described in [`docs/data-contract.md`](docs/data-contract.md). Keep them outside this public repository or under the ignored `private/` directory. The inventory owner restricts access to the field data.
3. Set `EE_PROJECT` to your own Earth Engine project. Run notebook **01** in order. Wait until all Earth Engine Drive exports finish, then download them into `private/exports/`. Supply the authorized TESSERA plot-mean table and run the local assembly cells.
4. Inspect the exported column names and resolve the [method alignment](docs/method-alignment.md) items. Notebook **02** requires the real four-zone management assignment and the correct predictor names. Run it only after those checks.

On macOS/Linux, example environment settings are:

```bash
export EE_PROJECT='your-ee-project'
export DIVERSITY_PLOTS_CSV='private/plots.csv'
export DIVERSITY_AOI_FILE='private/study_area.gpkg'
export DIVERSITY_EXPORTS_DIR='private/exports'
export DIVERSITY_TESSERA_CSV='private/TESSERA_2023_plot_means.csv'
export DIVERSITY_WIDE_CSV='private/MODELING_DATASET_2023_WIDE.csv'
export DIVERSITY_BLOCK_COLUMN='management_zone'
```

Equivalent environment variables can be set in PowerShell. Earth Engine exports are sent to the user's own Google Drive folder (`EarthEngineExports` by default); set `DIVERSITY_EXPORT_FOLDER` to change it. The notebooks do not contain credentials.

## Methods at a glance

Earth Engine provides 16-day Sentinel composites and annual AEF embeddings for 2023. The extraction notebook computes mean predictor values in circular buffers of radius **17.84 m** around plots. Annual TESSERA embeddings require separate authorized access. The modeling notebook uses a Keras feed-forward network (Normalization → 64 ReLU → 64 ReLU → one output), training-only median imputation, four management-zone holdout folds, and ten seeds per fold. Its intended full run is **1,440 fits**, plus permutation importance calculations.

Each held-out zone remains outside imputation, normalization, network training, and early-stopping validation. Early stopping uses a subset of the other three zones. Reported results are saved locally under `outputs/` and excluded from Git; they are **not** supplied here.

## Data availability and privacy

National Forest Inventory data used in the manuscript are owned in part by the Natural Resources and Watershed Management Organization of Iran and are not publicly available without the provider's permission. This repository contains code only. Do not commit plot locations, field attributes, zone membership by plot, Earth Engine exports, downloaded TESSERA tiles, derived plot-level tables, notebook outputs containing data, or authentication files. Public satellite catalogs have their own access and use terms; TESSERA tile availability depends on its data provider.

## Citation and reuse

Please cite the associated paper once its bibliographic details are final. Add a `CITATION.cff` with the final DOI and full approved author list. **No software license is asserted by this draft repository:** the rights holders should select one before inviting reuse or contributions.

## Known limitations

- Original extraction and manuscript differ in plot support and Sentinel-1 predictor definitions. The current public extraction uses plot buffers and retains the supplied preprocessing logic; see [method alignment](docs/method-alignment.md).
- The two notebooks will intentionally stop when required inputs or feature names are missing. TESSERA downloads are not automated because the original study obtained tiles by request.
- An Earth Engine account, private inventory, TESSERA data, and a completed export run are needed to reproduce numerical results.
