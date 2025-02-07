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

One can then test each cell, starting at cell 1 and ending at cell 29 with the 31% threshold:
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
The threshold of a 0.19 HDgridScore was used as a minimum score to identify a cell as a grid cell. Cells with scores below that were discarded from analysis. Specifically, of cells n1-n29, cells n7 and n9 were excluded for having scores of 0.17.

All cells, at threshold 31%, n1-n29 (excluding n7 and n9) were indentified through visual detection to have field detection without errors as described in the article. This is with the exception of n3 and n29, which were found to have errors with their detection. One can view the field detection plots the software generates to recreate these results.