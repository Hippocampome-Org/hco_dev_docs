Metric Results File
===================

This document describes data that can be in the contents of the metric_results.csv file that is automatically created when GridMet is run.

`mean_field_sizes`

The mean of field sizes in pixels as a unit.

`mean_field_sizes_px2cm`

The mean of field sizes in centimeters as a unit.

`mean_field_dists`

The mean of field distances in pixels as a unit.

`mean_field_dists_px2cm`

The mean of field distances in centimeters as a unit.

`minimum_angle`

The minimum angle relative to the x- or y-axis of the most center field in the plot to its closest field.

`matching_centroid_number`

The centroid id number that is the closest field to the most center field. This field is used in the minimum_angle value reported.

`grid_fields_reported`

The number of grid fields that were detected and can be included in statistics. This value does not take into account any fields excluded from statistics with manual_field_exclud.

`size_space_ratio`

A ratio value of the mean size of fields vs. the spacing of fields.

`in_out_ratio`

A ratio value of the firing (or correlation) levels within fields compared to outside of them.

`cov_mean`

A value that is the mean of the coefficient of variation of the firing rates (or correlation values in the case of autocorrelograms) across fields.

`gs_orientation`

The orientation angle reported using opexebo's methods.

`gs_orientations_std`

The standard deviation of orientation angles reported using opexebo's methods.

`field_1_x`, etc.

These are x- and y-coordinates of each field's centroid. This is reported when report_centroid_positions is enabled.