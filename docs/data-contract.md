# Private inputs and output contract

## Inputs

| Input | Required schema | Handling |
| --- | --- | --- |
| `private/plots.csv` | Unique `PLOTID`, WGS84 `Longitude` and `Latitude`, genuine `management_zone` with four values, and the three targets `shannon_H`, `species_richness`, `dbh_std`. Optional `QMD`, `n_trees` for error diagnostics. | Restricted NFI data; never commit. |
| `private/study_area.gpkg` | Valid vector study-area geometry with a declared CRS. | Check provider's sharing conditions. |
| `private/TESSERA_2023_plot_means.csv` | One row per `PLOTID`, exactly 128 numeric embedding columns, mean within the same plot footprint. | Obtained separately by authorized request. Rename columns to the model's configured convention after checking actual export names. |
| Earth Engine project | Cloud project enabled for Earth Engine; user's authentication. | Supply through `EE_PROJECT`; never embed tokens. |

Notebook 01 writes one CSV **per nonempty 16-day bin** for each of `S1_2023`, `S2_2023`, and `RaoQ_2023`, plus `AEF_2023_plot_means.csv`, to the chosen Drive folder. Download these into `private/exports/`. It checks for 23 bin files per time-series product; a missing file can represent a pending/failed task or no imagery, so investigate it before joining. It joins on `PLOTID` and `bin_start`, then writes the wide table to `DIVERSITY_WIDE_CSV`.

Notebook 02 requires one row per plot, a four-zone `management_zone` field, all three targets, 23 date-suffixed temporal predictors per configured band, 64 AEF dimensions `A01`–`A64`, and 128 TESSERA dimensions. It stops if those names or counts are missing. **The model-side placeholder S1 and S2 group names need mapping to the actual exported bands before a full run.** The field attributes and geographic coordinates must not be included as predictors.

## Local sanity checks

- Confirm identical plot IDs and a single row per plot/period across exports.
- Check 23 temporal bins, dates, missing-value fractions, and pixel support within buffers.
- Confirm the 2023 product versions, S1 orbit handling, S2 cloud masks, RaoQ definition, and TESSERA dimensionality.
- Validate the four zones against the management boundaries in the manuscript; plot IDs do not encode spatial folds.
- Run a small authorized data smoke test before the model experiment.
