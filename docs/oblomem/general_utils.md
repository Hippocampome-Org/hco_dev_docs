General Utilities
=================

This documentation contains descriptions about general utilities within Oblomem. These utilities are used for a variety of tasks such as analyses, plotting, and resource generation for the simulation.

## Overview

These files are included in the folders general_tools and general_analyses.

## behavior_metrics

This utility produces files that are needed in the simulation's data directory. This software formats animal recording measurements for use in the simulation. This software needs csv (comma spaced value) versions of data provided by the team member that performed animal recordings. The recordings originally being in xlsx (Excel) format can be converted into csv through Excel or similar spreadsheet software.

## create_train_pat

See training pattern files creation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/create_train_pat.html)) for details about this utility.

## CS6_new_proj

See "Create Project" in the new projects documentation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/new_projects.html)) for details about this utility.

## lat_inhib_conns

See "Lateral Inhibition" documentation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/lateral_inhibition.html)) for some further details.

See "Check that lat_inh_conns.csv Can Be Found" in the new projects documentation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/new_projects.html#check-that-lat-inh-conns-csv-can-be-found)) for some details.

## lognormal_analyses

This utility is used to analyze lognormal distribution statistics for replacing part of that signal with neuromodulation-based current. The parameters here can be used to test reducing lognormal distribution levels. Specifically, this can test if converting Acetylcholine neuromodulation levels into current can bring the total current level up to a similar amount that existed before the lognormal distribution current was reduced. This can be useful if using the "Acetylcholine (ACH) Levels as Current" method described in other documentation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/general_comp.html)).

## plot_obj_exp

This utility reports statistics on times an animal spent exploring objects and not exploring objects. This can be informative of how frequently patterns will be presented in neural network training based on an animal's behaviors. For example, how often training patterns will be presented that represent an animal exploring a non-stationary object. The frequency of pattern presentation among concept cells, e.g., object cells, can affect the strength of synapses formed in association with the concepts and at what times during the training process these synapse changes occur. One may want to be aware of cases were many more concepts of a particular type, e.g., places, are presented relative to another concept, e.g., objects. Specifically, an animal is always in a place, but may much less frequently be exploring an object. Possibly training parameters and methods should be balanced to ensure an intended amount of training (not too much or little) of concepts, even when they are presented at different frequencies.

It appears that lines in the main project file can be uncommented to produce the `obj_nonstat_exp_times.csv` and `obj_stat_exp_times.csv` files needed by this Matlab script. The `obexp_time_file` appears to collect the data of interest. One may need to edit the `obexp_time.csv` that file object creates to have `1` for object exploration and `0` for non-exploration for input into plot_obj_exp.m. One can also verify that this file object produces the right data, but a guess is that it does based on reading the source code.

## plot_quads

This utility plots a histogram of place quadrant visits. This can be used for similar purposes for places as the plot_obj_exp utility is used for object exploration analysis. The main project file has commented out code with the file object `quads_file` that appears it can be uncommented to produce a file, `quads.csv` recording the quad locations visited during a simulation. These locations can be based on animal movements if animal locations are input into the simulation to generate movement of a virtual animal. The term "quad" refers to 4 "quadrants" that represent 4 square locations of equal size within a square environment that animal movements were recorded in.

One can test to confirm that uncommenting those lines produces a file with the data wanted, but it appears that it does.

## plot_speeds

This utility provides a plot of speeds that an animal had during a simulation. The `speed` column in the `Full_GCaMP7sChAT_611915_210426_Rec1_15min_Learning_data.csv` file can be used as a data source for the `speeds.mat` file needed by this utility software. One should extract the data from that column and save it as a Matlab matrix named `speeds.mat`.

## plotting

This code contains modified versions of plotting software from (Kopsick et al., 2024). See the "Creating Plots" documentation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/plotting.html)) for more instructions about plotting.

## report_objexp_quads

This utility reports on the co-occurence of object exploration with location quadrants. Uncommenting lines that produce `quads_obj_exp.csv`, `quads_time.csv`, and `obexp_time.csv` in the project's main file appears to produce the data needed for this utility. There are `report_objexp_quads.m` and `report_objexp_quads2.m` programs that each report statistics. The author of this documentation does not remember why a `objexp_quads.mat` file was created as input to `report_objexp_quads.m` instead of using `quads_obj_exp.csv`. One using this software, either `.m` file, should verify that the csv data is sufficient as input to these programs. Also, one can invesigate if `quads_obj_exp.csv` can be used or for some reason a different input dataset (e.g., `objexp_quads.mat`) is needed.

One way that this utility can be useful is confirmation that the animal recording data has the majority of its measurements with an object in one quadrant and another object in a different quadrant.

## rhythms

The utility software `rhythm_reporting.m` can be used to report rhythm-related measurements. Much of its code is repurposed from a grid cell simulation project, and still may have code referencing that such as `MEC_LII_Stellate` and `gc_can_`. One should update the line `local_path` to CARLsim binary spike data from the Oblomem experiment to measure that data. For example, a `spk_CA3_Pyramidal.dat` file measuring Pyramidal cell spikes. The line `initOAT` loads CARLsim's Offline Analysis Toolkit code. See "initOAT.m" in documentation [here](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/plotting.html) for more information about this. work_report.docx's "Fig. 23. Power spectral density of CA3 Pyramidal Cells..." shows an example of plotting that can be created from this software.

## stdp_analyses

This utility is designed to analyze STDP-based processing of spike times reported by an Oblomem experiment. See "STDP Analyses Methods" documentation ([link](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/stdp_analyses.html)) for more details about how to use the `spike_times_analyses.m` program. Example csv input files are not provided in the Github repository for Oblomem because the file size of the csv files can be large (e.g., 30 to 40 mb per file). The format of data expected in the input files is described in the "STDP Analyses Methods" documentation.

## Reference

Kopsick, J. D., Kilgore, J. A., Adam, G. C., & Ascoli, G. A. (2024). Formation and retrieval of cell assemblies in a biologically realistic spiking neural network model of area CA3 in the mouse hippocampus. Journal of Computational Neuroscience, 52(4), 303-321.
