# Alignment to check before numerical replication

| Topic | Manuscript description | Supplied notebook / prepared code | Action |
| --- | --- | --- | --- |
| Plot footprint | Mean predictor values in a 17.84 m radius circular plot. | Supplied notebook used `sampleRegions` at point locations. Prepared extraction uses `reduceRegions` over 17.84 m buffers. | Re-extract and compare outputs; decide which method produced the reported results. |
| S1 handcrafted variables | Four pass-specific VV/VH backscatter and two NDI summaries, plus four textures per bin. | Supplied S1 code merges passes and exports `VV_gamma0`, `VH_gamma0`, `S1_ND`, and `S1_VV_over_VH` with textures. | Verify original analysis features; implement documented pass-specific bands and summary definitions if the manuscript is authoritative. Update DNN feature mapping. |
| S2 handcrafted variables | 10 spectral bands, EVI and RaoQ summaries, four EVI textures; table lists 13 variables for the spectral plus EVI/RaoQ set. | Supplied spectral export has 10 bands + EVI + 4 textures; RaoQ export also contains EVI and RaoQ. Prepared join drops repeated EVI. | Resolve the table's 13th variable and map the actual RaoQ name in the DNN configuration. Do not invent a missing band. |
| TESSERA | Annual 128-dimensional 2023 representation. | Tiles obtained by separate request; no end-to-end public extraction code. | Document tile identifier, version, footprint aggregation, and approved access route. |
| Spatial evaluation | Four management-zone blocks, ten DNN initializations each. | Original public notebook lacks zone labels and DNN; companion notebook requires real zone labels. | Supply verified private assignment; check results against the manuscript. |
| Software and stochastic details | Architecture and stopping rules specified; exact dependency versions, seed values, inner holdout choice, and permutation repeat count not specified. | Companion implements documented architecture and explicitly records these implementation choices. | Recover original choices where possible; freeze versions and retain run provenance. |

The supplied manuscript is the source for stated methodology; the supplied notebook is the source for observed code. A difference in this table is **not** evidence that one is correct. The authors must resolve it against the scripts and exports used for the published figures.
