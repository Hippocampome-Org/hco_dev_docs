Configuration Options
=====================

This document describes information about configuration options for GridMet

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

use_dist_thresh

dist_thresh

use_fld_sz_thresh

fld_sz_thresh

manual_field_exclud

load_px2cm_conv

dbscan_epsilon

dbscan_min_pts

load_custom_rm

load_custom_ac

only_center_seven

only_center_seven_inout_excl

use_tophat_filter

use_centsurr_filter

minimal_plotting_mode

auto_export_plots

plot_fields_detected

plot_orig_firing

plot_legend

print_angles

control_window_size

custom

custom2

custom3

custom4

sml_ang_cnt_fld

sml_ang_cent_num

advanced_detection

advanced_detection_maxdist

advanced_detection_ang_inc

com_centroids

in_out_fields_ratio

cov_between_fields_reporting

report_gridscore

report_orientation2

min_orientation2

report_centroid_positions

report_centroid_positions_python

outlier_removal

outlier_allowance

spac_exclud

size_exclud

ang_exclud