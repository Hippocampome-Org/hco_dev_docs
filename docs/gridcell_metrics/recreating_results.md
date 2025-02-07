Recreating (R. G. et al., 2024) Results
=======================================

An analysis in (R. G. et al., 2024) evaluated which non-field value filter threshold parameter detected grid fields with the least errors. This documentation will describe how to recreate that analysis.

The GridMet software was used to find these results. Some of the later developed features in the software should be deactivated to recreate the results as reported in the article. Those features, which should be set to specific values as listed below in config.txt, are:

```
advanced_detection, 0
com_centroids, 0
outlier_removal, 0
spac_exclud, "[]"
size_exclud, "[]"
ang_exclud, "[]"
```

One can then test each cell, starting at 1 with the 31% threshold:
```
plot_filepath, "heat_maps_real/n1.mat" (update n1.mat for each cell tested)
use_binary_input, 1
use_ac, 1
convert_to_ac, 1
```

One can check the grid scores for each cell with:
<br>`report_gridscore, 1`
<br>but one must set config.txt to not convert a rate map file imported into an autocorrelogram.
This is done with:
```
use_ac, 0
convert_to_ac, 0
```
The threshold of a 0.19 HDgridScore was used as a minimum score to identify a cell as a grid cell. Cells with scores below that were discarded from analysis.