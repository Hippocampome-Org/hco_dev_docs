Plotting and analyzing results
==============================

This documentation describes how to view plots and statistics for the results of the simulation.

## initOAT

The function initOAT should be called to access CARLsim's Offline Analysis Toolkit. This is software designed for plotting. In initOAT.m, addpath(...) must include the path to the OAT software.

## CMBhome

Some plotting scripts will made use of CMBHome software. It is added to the scripts with:
<br>$ addpath /path/to/CMBHome. 
<br>The CMBHome software should be downloaded ([link](https://github.com/hasselmonians/CMBHOME)) and stored in the folder that addpath directs to.

## Rate map video

The Matlab script activity_movie_nrn_spc.m can be used to create a plot showing a rate map video of neuron space simulation results. The script will expect a project filename with a number at the end. This is intended to accomidate multiple projects, that test different experiments, with similar filenames, but different numbers at the end to distinguish them. The first part of the project filename should be entered in the variable fdr_prefix. The number at the end should be entered in the variable local_run. The variable local_path specifies where the results directory for the project is located.

The variable time_start specifies at what time in the experiment to start plotting. time_end specifies when to stop plotting. bin_size is the number of milliseconds that spiking can occur to report rates in each frame of the video. x_size and y_size are the size of the axes of the neural layer being plotted. set(cb, 'ylim', \[start_value end_value\]) sets the range of the colorbar. v = VideoWriter(...) sets the location and name of the video file that is created.

## Trajectory map, rate map plot, grid score, and average firing rate

The script gridscore_sim_run_local.m can produce a trajectory map, physical space rate map plot, grid score, and average firing rate. Variables include:
<br>`fdr_prefix`: the first part of the CARLsim project filename to retreive results from.
<br>`local_run`: the second part of the CARLsim project filename to retreive results from. This is expected to be a number.
<br>`preloaded_spk_reader`: use data stored from a prior run of the script or retreive the data from the project's results.
<br>`restrict_time`: set number of milliseconds to plot. Use the value of 0 for no restriction.
<br>`sel_nrn`: set which neuron to use for plotting.
<br>`plot_spikes`: choose to plot spikes. If this is 0 then only the trajectory is plotted.
<br>`smaller_spk_ticks`: choose spike marker size.
<br>`save_plot`: automatically save rate map plot to a file.
<br>`save_traj_plot`: automatically save trajectory plot to a file.
<br>`save_gridscore_file`: automatically save grid score to a file.
<br>`save_firingrate_file`: automatically save firing rate to a file.
<br>`save_firingpeak_file`: automatically save firing peak to a file.
<br>`plot_subsect`: plot pubsection of neural layer.
<br>`grid_size`: neural layer size.
<br>`plot_size`: subsection size to plot.

gridscore_sim_run_local.m will call the scripts high_res_traj.m and activity_image_phys_spc_smooth.m.

Statistics on the grid score, average firing rate, and peak firing rate, will be printed to the Matlab console after the script is run. The trajectory and physical space rate map plots will be displayed.

## high_res_traj.m

This script will create the trajectory plot. This script will need to have the variable curr_dir set to the location of the CARLsim project's scripts folder. In the script, the location of CMBHome should be set in the line with "addpath" that includes CMBHome. The variable timestep should be set to the sampling rate of the recordings data. E.g., 1 recording every 20 ms would be timestep = 20, and this was the timestep value used in the article's data. Other variables include:
<br>`spk_bin_size`: bin size used in the spike reader. This is what number of milliseconds each sample of spikes will include from CARLsim's spike data. This affects the accuracy of the plotting of spikes. For example, if 100 ms is chosen, only every 100 ms along the trajectory the spikes will be plotted. Smaller times require more computational time but add accuracy. The article used the value of 10 because it was a good balance of time and accuracy.
<br>`plot_smooth_rm`: include smoothing in rate map plot that is based on the trajectory plot.

## activity_image_phys_spc_smooth.m

This script will create the physical space rate map plot. Some methods included in ([Dannenberg, 2020](https://elifesciences.org/articles/62500)) were included in the physical space rate map plotting to help with comparing simulated results to those obtained in that article's work. Some variables that enable use of those methods are:
<br>`occupancy_norm`: perform occupancy normalization. this adjusts rates by number of visits to locations.
<br>`omit_islands`: Takes matrix of occupancy values, calculates center of mass of pixels>0,
and then finds all disconnected pixels and sets them = 0.
<br>`omit_noocc`: set no occupancy to zero

Additional general variables that can be non-specific to (Dannenberg, 2020)'s plots include:
<br>`fs_video`: sampling rate from video (samples/sec)
<br>`real_env_size`: units of real animal environment from recordings
<br>`conversion_factor`: factor for converting from real recordings. This is applied to y-axis position values to set them toward a starting place of 0.
<br>`timestep`: recordings timestep of samples in ms

The color settings in the rate map plot can be adjusted after the rate map plot is created. This was done in the article to create the same color scale settings between Fig. 6 - source data 1 (SD1) of (Dannenberg et al., 2020) and new rate maps simulated. The original color settings from SD1 were adjusted in some cases to create a balanced graphical appearance with the new simulation results, but the same color settings were used for both plots in, for instance, Fig. 2's rate maps. Example rate map settings are availible in /scripts/settings_archive/plot_settings.txt, and these settings were used in the article's plots. The "caxis" settings in the file in the lines with "rm:" can be applied to rate map plots. First select the rate map plot, then run the caxis command (e.g., "caxis(\[0 14.23285\]"), to apply color adjustments to the plot in matlab. The last plot window selected in matlab will have any settings changes run in the command prompt, including the caxis command, applied to the plot.

## Autocorrelation plots

The script activity_image_autocorr.m can be used to create autocorrelation plots. A rate map matrix must be provided to the script to perform the plotting. This is stored in a variable named heat_map. The rate map can be provided in the matlab workspace by running gridscore_sim_run_local.m as described earlier to create a rate map. The script will use the heat_map variable generated to create the plot.

The activity_image_autocorr.m script uses the plot_rate_map_ac() function from CMBhome. Variables for root, cell_selection, spk_x, and spk_y are needed to run the function. However, plots in practice have been observed to be made with the heat_map variable data irregardless of what data is in the other variables to an extent. The root variable will need to be generated through running any animal recording data through the load() function.

A note is that an autocorrelation plot will be created and there will also be an error that "index exceeds the number of array elements". Code on the script did not correct for the error because the goal of creating the plot was achived. The error should not cause issues with plot creation.

Some elements in the plot will require adjustment. The colors in the plot should be adjusted. In the article this was done to match the colors of autocorrelation plots in SD1. The default matlab colors are different than those. Adjustment can be done with "colormap" and "caxis" functions. For example, run "colormap default; caxis(\[-0.51 1.01\])" in matlab after selecting the plot then returning to the matlab window. More examples of color adjustment can be found in the file /scripts/settings_archive/plot_settings.txt in the lines with "ac:". That file includes settings used to create plots in the article.

The autocorrelation plots generated should be flipped horizontally (so that the left side becomes the right side) if there is an intention of matching the display that SD1 plots have. It was observed that new simulation rate map plots that are similar to SD1 plots create autocorrelation plots (with the script in this software) that need that flip to be oriented in the correct way.

## Parameter exploration plotting

Grid scores from parameter testing can be plotted with the instructions in the "Parameter exploration plotting" documentation.

## Raster plot

The script plot_spikes_combined.m can create a raster plot of all spikes in a simulation as shown in the article's Fig. 4. The required data for this plot includes simulation results that ran for at least the "Te" variable time (end time to plot). Spike time results files for every neuron in the simulation are also needed. The can be produced by setting the "spk_mon_additional" variable to 1 in general_params.cpp, and then running the simulation.

The plot legend can be toggled with the "ShowLegend" variable. The legend may obscure the view of some spikes, so saving a version of the plot with it disabled may be wanted.

A user can set any additional variables wanted. For example:
<br>`Ne`: number of principal cells to plot in each principal cell group.
<br>`Nei`: number of interneurons to plot in each interneuron group.

## Voltage and current plots

The script plot_volt_or_current.m can be used to produce plots of voltage or current. Examples of such plots are shown in Fig. 5. Requirements for these plots include that data files from CARLsim's NeuronMonitor function are created for the neurons that the user would like to plot. This can be produced by setting the variable "monitor_voltage" or "monitor_voltage2" to 1 in general_params.cpp. The choice of what variable to set depends on what neuron is intended to be recorded. The neurons that each variable matches are listed in generate_config_state.cpp.

Users can also set additional variables as wanted, such as:
<br>`plot_voltage`: set to 1 for voltage and 0 for current plotting
<br>`include_in`: plot interneuron(s) in addition to the principal cell(s)
<br>`t_start`: start time to plot
<br>`t_end`: end time to plot
<br>`nrns_tot`: number of neurons to plot
<br>`rsz_peaks_active`: resize peak values. The Izhikevich model in CARLsim does not have peak voltage values from spikes that would be similar to peak values in real animal recordings. Activating this variable corrects for this by setting any value above a certain threshold to a "peak" value. This causes simulated voltage plots to appear more similar to real plots. The Izhikevich model potentially having lower peaks does not seem to cause much of an issue with neural activity fitting with biological realism because the process of "spiking" is still performed. The article's voltage plot uses this resizing correction and it is noted in the article.
<br>`min_spk_v`: threshold to detect voltage as a spike
<br>`new_peak`: new peak voltage once threshold is met
<br>`min_isi`: minimum inter-spike interval required from last resized spike voltage peak to next voltage peak to resize the next peak.

## Animated trajectory plot

The script activity_movie_animate_spike_traj.m can create a video of a trajectory plot being created over time including spikes. Some settings can be set with the variables:
<br>`start_time`: the start time to be plotted
<br>`restrict_time`: amount of time to plot. Use value 0 for no restriction on amount of time to plot (plots the full experiment time).
<br>`fdr_prefix`: first part of project name.
<br>`local_run`: end of project name (typically a number).
<br>`sel_nrn`: neuron index to plot.
<br>`spk_bin_size`: spike reader bin size. This is the number of millisecond in which to look for a spike. For instance, a spike occuring in every 10 ms bin can be chosen as the setting. Shorter bins require more processing time.

## Animated raster plot

The script activity_movie_plot_spikes_group.m can create a video of a raster plot of a group of neurons occuring over time. Some variables that control settings include:
<br>`N`: number of neurons to plot.
<br>`t_window`: amount of time (window of time) to plot as the experiment time increases.
<br>`t_start`: start time to plot
<br>`t_end`: end of plot time
<br>`use_selected_time`: supply a csv file with saved spike times rather than using ".dat" results files from CARLsim. This is useful if a user wants to plot time far after the start of an experiment, e.g., 1 hr after the start as shown in the second half of the article's supplemental material Fig. S5 video. Having to load data from such a long time in an experiment may produce memory issues on a computer. Creating the csv file through seperate processing of a ".dat" file from CARLsim's spike recording can help avoid the computer resource overload isse.
<br>`alter_plot_start_time`: alter time shown in the video by a value. This is useful when specifying a time to match the time present in a `use_selected_time` file.

## Animated voltage or current plot

The script activity_movie_volt_or_current.m can create a video of voltage or current occuring over time in an experiment. Many of the plot options are similar to the script plot_volt_or_current.m described above. Additional settings parameters include setting the name of a video file, e.g., "volt_movie.avi", in the line that includes VideoWriter.

## Combined real time signals animation

A video that combines rate map, trajectory, raster, and voltage plot videos can be created through video production. An example of such a video is the one shown in the article's supplemental material figure Fig S5. This video was created with the video editor Kdenlive (kdenlive.org; an open source tool) on Ubuntu Linux.

A part of the video production was changing the speed of the individual videos to match each other. The different videos showed time occuring at different rates and needed to be syncronized. Kdenlive has a function to duplicate a video clip with a speed change. That function was used to get the videos at least very close to matching each others' rate of time passing.

## References:

Dannenberg, H., Lazaro, H., Nambiar, P., Hoyland, A., & Hasselmo, M. E. (2020). Effects of visual inputs on neural dynamics for coding of location and running speed in medial entorhinal cortex. ELife, 9, e62500. https://doi.org/10.7554/eLife.62500