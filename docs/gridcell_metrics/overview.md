Grid Cell Metrics Measurement Methods
=====================================

This documentation will prodive further information about concepts and methods involved with the grid cell metrics measurement software.

## Related article
Some additional details and analyses run with this software can be found in this article.
<br>\[1\] R. G., R., Ascoli, G. A., Sutton, N. M., & Dannenberg, H. (2024). Spatial periodicity in grid cell firing is explained by a neural sequence code of 2-D trajectories. eLife, 13. [https://doi.org/10.7554/eLife.96627.1](https://doi.org/10.7554/eLife.96627.1)

## Example Usage Instructions
See [instructions](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/usage_instruct.html) for example usage directions.

## Orientation Angle Measurements
See [orientation angles](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/orientation_angles.html) for additional information.

## Configuration Options
See [config options](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/config_options.html) for information about configuration options for running GridMet.

## Metric Results File Data

See [metric results file](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/metric_results.html) for information on the data reported in metric_results.csv.

## Recreating (R. G. et al., 2024) Results

See [recreating results](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/recreating_results.html) for information on recreating results from this publication.

## GridMet Performance Tests

See [GridMet performance tests](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/performance_tests.html) for results of GridMet software performance tests.

## Parameters
Some selected parameters are listed below but more parameters are listed in [config options](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/config_options.html) and config.txt.

#### Required Parameters
`nonfld_filt_perc`: Threshold for non-field filtering to perform. Range is 0-1.
<br>`px2cm`: Conversion of each pixel to a centimeter lenth in an enviornment
<br>`load_plot_from_file`: Load neural data plot from individual file (1; plot_filepath)
<br>`plot_filepath`: Path to file that contains the neural data if load_plot_from_file = 1
<br>`heat_map_selection`: Used if load_plot_from_file = 0. Select cell to analyze (cells are numbered and file locations are stored in heat_maps_list.mat and heat_maps_ac_list.mat).

#### Optional Parameters
`use_ac`: use autocorrelogram (1) or rate map (0).
<br>`only_center_seven`: filter intended for autocorrelograms where fields other than the center 7 are filtered out.

#### Plotting Parameters
`plot_fields_detected`: plot fields the software detected.
<br>`plot_orig_firing`: plot original ratemap or autocorrelogram used as the source of data for the software.
<br>`plot_legend`: include legend in plot.

## General Info.
A method used in (R. G., 2024) was also included in the grid cell simulation article's analyses with this software. That method is that the size of a detected field did not affect its inclusion in metric measurements. Evaluating if a field was detected as large or small as a basis for exclusion was considered subjective and not included as a method for the benefit of reproducible methods.

A general note is that this software was tested with square environments and plots. It is expected to work with rectangle (non-square) environments but that has not been fully tested. It is unknown how it will work with non-rectangle environments. This software does not yet claim to work correctly with non-square environments unless further testing establishes that it can successfully handle those environments.

The runtime version of this software has been tested and observed working with MATLAB runtime software version 2022a. Other versions of the runtime software may work with it but this software does not make any claim to it working with other versions due to that being not yet tested.

## Metric Conversions
The value in `px2cm` is applied to measurements in pixels found in size and spacing values to report their values in centimeters. Mean size and spacing values are multiplied by the `px2cm` value to report them in centimeter instead of pixel length units.