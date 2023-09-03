Converting animal recordings data for simulation use
====================================================

## Find data files containing grid cells

Data files can be batch processed with scripting to identify which cell files contain grid cell firing. Grid cell plots can be previewed with [CMBHome's](https://github.com/hasselmonians/CMBHOME) Visualize Matlab script. If using data from ([Dannenburg et al., 2020](https://elifesciences.org/articles/62500)), then grid patterns of interest can be previewed in the supp. mat. plots ([link](https://cdn.elifesciences.org/articles/62500/elife-62500-fig6-data1-v2.pdf)).
<br><details>
<summary>Filenames used in the article</summary>
In the article's Fig 2., filenames used for experiments were: 
<br>* 191108_S1_lightVSdarkness_cells11and12.mat tetrode 1 cell 9 (T1C9) for small-grid scale.
<br>* merged_sessions_ArchTChAT#22_cell1.mat T2C1 for medium-grid scale.
<br>* GCaMP6fChAT10_gridCell_mergedSessions.mat T2C1 for large-grid scale.
<br>
<br>Any users wanting to use this specific data should please contact us for it.
</details>
<br>
## Converting data files into a format for the simulation

### moves_reporter

Use moves_reporter.m to convert animal movements in recording files into datasets of animal angles and speeds that can be processed by the simulation. In moves_reporter.m, set file_to_reformat to the animal data file location and set reformat_traj_data = 1. Set which tetrode and cell to use in the file with cell_selection = \[\<tetrode\>,\<cell\>\]. Select to output csv files by write_csv_file = 1. Set CMBHome_path to where [CMBHome](https://github.com/hasselmonians/CMBHOME) is located.	Run the script. The reformatted data will be output in to the output directory as angels and speeds csv files.

### moves_converter

Additional reformatting should be done by running moves_converter.m. Any parameters wanted should be set. For instance, speed_mult will scale the speed values. This will have an effect of rescaling the trajectory of movement that is used in the simulation. A value of 1 creates no scaling. Maximum speed limiting is disabled by default with use_speed_limit = 0. Run the script. Files generated are: anim_angles.csv, anim_speeds.csv, anim_angles.cpp, anim_speeds.cpp. These files should be copied to the "data" directory in the CARLsim project base directory.

## Find experiment time in milliseconds

Use moves_get_time.m to get experiment time in milliseconds. This will be used later for informing the simulation of how long to run the experiment with the animal movement data. The technical way this script does this is by counting the number of entries in CMBHome's root.ts.

<br><details>
<summary>Alternative methods</summary>
Alternative 1
<br>An alternative way to find this is to use CMBHome's Visualize script and plot the trajectory. From plot data, calculate floor((1/\<num_spikes_per_sec\>) * \<total_spikes\>) = \<seconds_in_epoch\>. E.g., a large-grid scale cell was found to have 8553ms.
<br>
<br>Alternative 2
<br>Open CMBHome's root object in the workspace. Open root.ts. Add up all timesteps listed there (num_entries of timesteps). Then (num_entries * timestep)/1000 = seconds in sim. For example, 8553860 ms has been found for a large-grid scale cell. 1440140 ms has been found for a small-grid scale cell. Alt. 2 gives a more exact count but can be confirmed with alt. 1.
</details><br>

## Center trajectory

A useful starting position for the virtual animal should be found to center the animal movement trajectory for plots and other reasons. The starting position for the virtual animal is set with pos\[2\] in general_params.cpp, and set for the bump with bpos\[2\]. The movement function move_animal_onlypos can be used to help find a good starting position. Details about using the function are in the movement paths documentation. 

Plotting of the trajectory can be done with the script gridscore_sim_run_local.m. This presents how the trajectory is processed compared to the grid cell layer. Some trial and error may be needed to find the correct starting point. The matlab plots allow moving the cursor over points to see their coordinates. In a 40x40 sized layer, it is useful to have the trajectory in the bounds of \[4.5,4.5\] to \[35.5,35.5\]. 