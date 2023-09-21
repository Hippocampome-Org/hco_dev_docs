Parameter exploration plotting
==============================

These are instructions for plotting parameter exploration results.

## Plotting

Plotting the results can be created with scripts/param_explore/plotting/plot_params.m. This plotting script uses the values reported from the exploration and interpolates values between those values for the purposes of plotting a colored surface of values. 

The plotting script will need to have the variables plot_rotation_factor_1, plot_rotation_factor_2, and maybe plot_rotation_factor_3 altered to set the 3d camera location to a preferred place for capturing the plot. The plot is of a 3d surface but the article's plots have the camera directly overhead of them which turns them into 2d plots. 

The downsample_amount setting allows for controlling the number of colors used to represent values in the plot. Setting the downsampling to 1 allows a wide range of colors. This can be used in an image editor such as [GIMP](https://www.gimp.org/) with the "select by color" tool to find the line on the plot that represents the score threshold of interest. That line can be then overlayed on a plot with downsampling of 10 that helps show different color sections with groups of values. This was done in plots in the article.

## Firing pattern fit results

The results need to be formatted for use in the plotting software. The file reformat_ea.sh is designed to create this reformatting. A user needs to set param_column_a and param_column_b to the columns in the results file that are for the parameters of interest. The reformatting script replaces the extra low value -3.40E38 with -20.0 to aid with plotting. A user will need to manually replace other extra high/low scores, e.g., 1000.0, with lower scores, e.g., 20.0, to aid with plotting. Given that such scores are far beyond the levels tested for, and that the plot is much easier to read with the rescaled scores, rescaling them is useful. A file gridness_score_ea_iz.txt will be generated that can be used for plotting.

Work in the article used a score threshold of 200% of the top ranked parameters reported on Hippocampome.org. For example, for neuron 6003 the fitness score is approximately -2.9, and 200% of that is appox. -5.8. This fitness score was found through setting the parameters in the json file to those displayed for subtype 1 on Hippocampome.org's neuron id 6003 neuron page in the Izhikevich model section. Then startEAbatch.sh was run to retreive the scores.

Parameters that are chosen to be tested in this software can be reapplied in testing grid scores. In the article, plots with the firing patten score threshold line and grid score threshold line were included. To generate those plots it was useful to test the same Izhikevich parameters in both the firing patterns and grid score parameter exploration tests. 

## Example settings

Some example settings for plotting based on animal data results are in /scripts/param_explore/plotting/plot_settings.txt. These settings were used in plots in the article. Example saved grid score results files are in the directory /scripts/param_explore/plotting/archive/. Those grid score files can be run with plot_params.m along with settings from plot_settings.txt to produce similar plots to those seen in the article (some formatting may be a little different). The beginning of the filename in the files in the archive directory describe if the parameters were IM or TM models or if they were im_fp which represents firing pattern tests. The last part of the filenames indicate which parameters were tested.