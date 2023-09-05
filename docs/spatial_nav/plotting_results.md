Plotting and analyzing results
==============================

This documentation describes how to view plots and statistics for the results of the simulation.

## Rate map video

The Matlab script activity_movie_nrn_spc.m can be used to create a plot showing a rate map video of simulation results. The script will expect a project filename with a number at the end. This is intended to accomidate multiple projects, that test different experiments, with similar filenames, but different numbers at the end to distinguish them. The first part of the project filename should be entered in the variable fdr_prefix. The number at the end should be entered in the variable local_run. The variable local_path specifies where the results directory for the project is located.

The variable time_start specifies at what time in the experiment to start plotting. time_end specifies when to stop plotting. bin_size is the number of milliseconds that spiking can occur to report rates in each frame of the video. x_size and y_size are the size of the axes of the neural layer being plotted. set(cb, 'ylim', \[start_value end_value\]) sets the range of the colorbar. v = VideoWriter(...) sets the location and name of the video file that is created.

### initOAT

The function initOAT should be called to access CARLsim's Offline Analysis Toolkit. This is software designed for plotting. In initOAT.m, addpath(...) must include the path to the OAT software.