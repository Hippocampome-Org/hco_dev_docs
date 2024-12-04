Configuration Options
=====================

This document describes information about configuration options for GridMet. Please see the other documentation pages for GridMet to have its recommended use explained. For example, the [overview](https://hco-dev-docs.readthedocs.io/en/latest/gridcell_metrics/overview.html) describes pages that are a part of the documentation.

## Config.txt options

`px2cm`

Conversion of each pixel to a centimeter lenth in an environment.

`nonfld_filt_perc`

Non-field filter percentage threshold.

`load_plot_from_file`

Load a neural data plot from individual file (1 plot_filepath) or instead pick a file from a numbered list (0 heat_maps_list or heat_maps_ac_list). A note is that one must run the source code, not runtime version of GridMet, to edit these lists.

`plot_filepath`

File path to plot to load from a file when load_plot_from_file is enabled.

`use_binary_input`

Select to use (1) or not (0) binary input files as opposed to text input files.

`heat_map_selection`

Index number of plot file on heat_maps_list or heat_maps_ac_list to use with this software if load_plot_from_file is set to 0.

`use_ac`

Choose to use autocorrelogram (ac) (1) or standard rate map (0).

`convert_to_ac`

Automatically convert rate map input into an autocorrelogram. If this option is not selected and use_ac is set to 1 then a user must supply a previously generated autocorrelogram data file as the plot to load.

`use_dist_thresh`

This is an experimental feature that may not yet work correctly. This is designed to filter out fields that are too distant from other fields

`dist_thresh`

Distance threshold parameter in pixels for use_dist_thresh

`use_fld_sz_thresh`

This is an experimental feature that may not yet work correctly. This is designed to automatically filter out fields at plot borders without enough pixel points in their area. This can try to automatically detect fields the are cut off too much at the border of plots. This could aid in automatically excluding fields in addition to a user being able to manually exclude fields.

`fld_sz_thresh`

This is a threshold for use_fld_sz_thresh of the minimum number of pixels in a feild's area to allow not filtering the field out.

`manual_field_exclud`

Choose to enable (1) or disable (0) manually excluding fields from statistical analyses.

`load_px2cm_conv`

Load saved px2cm settings based on file number. saved_px2cm_conv.m is the file the settings are saved in.

`dbscan_epsilon`

DBSCAN's epsilon parameter value. See https://www.mathworks.com/help/stats/dbscan.html for more info.

`dbscan_min_pts`

DBSCAN's minpts (minimum number of neighbors required for core point) parameter value. See https://www.mathworks.com/help/stats/dbscan.html for more info.

`load_custom_rm`

Load rate map data from the heat_maps_real_custom_list.mat file. This is an alternative to using load_plot_from_file to load data.

`load_custom_ac`

Load autocorrelogram data from the heat_maps_ac_custom_list.mat file. This is an alternative to using load_plot_from_file to load data.

`only_center_seven`

Filter out fields except for the seven closest to the center of the plot.

`only_center_seven_inout_excl` 

Filter out fields detected beyond the center seven in the calculation of the in_out_fields_ratio. Note: this does not use advanced field detection to detect the fields outside the central seven. This option can only be used if only_center_seven is enabled.

`use_tophat_filter`

This is an experimental feature that may not yet work correctly. This feature is designed to filter neural firing through a tophat filter. 

`use_centsurr_filter`

This is an experimental feature that may not yet work correctly. This feature is designed to filter neural firing through a center-surround filter. 

`minimal_plotting_mode`

Preset plotting window size configuration

`auto_export_plots`

Automatically save generated plots

`plot_fields_detected`

Enables the plotting of detected fields.

`plot_orig_firing`

Enables the plotting of the original cell firing data.

`plot_legend`

Include a legend in the plot.

`print_angles`

This is an experimental feature that may not yet work correctly. This is designed to print the frequency of angles that are found between field centroids.

`control_window_size`

Preset plotting window size configuration.

`custom`

A group of custom configuration settings.

`custom2`

A group of custom configuration settings.

`custom3`

A group of custom configuration settings.

`custom4`

A group of custom configuration settings.

`sml_ang_cnt_fld`

Smallest angle from the center field. Default value for this is 361, which is beyond 360, and that is intended to be beyond the highest angle possible.

`sml_ang_cent_num`

Centriod number of sml_ang_cnt_fld match.

`advanced_detection`

Enable (1) or disable (0) advanced field detection.

`advanced_detection_maxdist`

Maximum advanced detection distance to test. This is the distance from the centroid to a point beyond that, e.g., 50 pixels. This is used in the scanning of each angle from the centroid to find where field boundaries are detected.

`advanced_detection_ang_inc`

Angle to increment radar scan in advanced grid field detection.

`com_centroids`

Use center of mass to find centroids.

`in_out_fields_ratio`

Report the ratio of firing intensity within fields compared to outside of them.

`cov_between_fields_reporting`

Report the mean of the coefficient of variation of the firing rate (or correlation values in the case of autocorrelograms) across fields.

`report_gridscore`

Report grid score metrics. "HD grid score" is a score previously introduced to quantify experimental
data in Dannenberg, H.; Lazaro, H.; Nambiar, P.; Hoyland, A.; Hasselmo, M.E. Effects of Visual Inputs on Neural Dynamics for Coding of Location and Running Speed in Medial Entorhinal Cortex. eLife 2020. https://doi.org/10.7554/eLife.62500. The Gridness3Score is based on a grid score metric found in the software CMBHome at https://github.com/hasselmonians/CMBHOME. 

`report_orientation2`

Report Opexebo methods based orientation measurement.

`min_orientation2`

Value of the minimum angle used in the orientation2 measurement.

`report_centroid_positions`

Report on the coordinates of centroid positions.

`report_centroid_positions_python`

Report on the coordinates of centroid positions in a Python syntax format. This can be used to test comparing results with Opexebo, which is in Python.

`outlier_removal`

Enable automatic removal of outlier points in fields. This can only be used if advanced_detection is enabled.

`outlier_allowance`

This is a parameter value for use with outlier_removal. This is an amount, as a percentage, of outlier distance from each median field radius to field boundary points to allow. Value is relative to 1.0 being 100%, e.g., 0.25 is 25% and 2 is 200%. Boundary points found beyond this value are converted into the closest maximum value this parameter allows. For instance, a boundary coordinate being 200% beyond the median with a limit of 25% will be converted into being a point 25% beyond the median.

`spac_exclud`

Grid feilds to exclude from field spacing statistics. Format should be "1|2|3" that is delimited by "|". No exclusion should be described as "\[\]". 

`size_exclud`

Grid feilds to exclude from field sizes statistics. Format should be "1|2|3" that is delimited by "|". No exclusion should be described as "\[\]". 

`ang_exclud`

Grid feilds to exclude from field angles statistics. Format should be "1|2|3" that is delimited by "|". No exclusion should be described as "\[\]". 