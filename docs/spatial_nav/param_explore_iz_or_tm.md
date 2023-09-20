Izhikevich or Tsodyks-Markram model parameter exploration
=========================================================

This documentation is for running parameter exploration with Izhikevich model (IM) or Tsodyks-Markram model (TM) parameters and testing the grid scores that are produced by the parameters.

## Update params_config

The main script used for TM parameter exploration is auto_mod_tm.sh and its settings are in params_config_tm.sh. For IM, the files are auto_mod_tm.sh and params_config_iz.sh. If it is needed, update the params_config file for the parameter values that are intended to be included. The param_pattern variables are pattern matching strings for C++'s regex library to find the data in the the project's .cpp file and update it to the intended values. The param_vals variables include the values to test. param_file specifies which .cpp file the values should be changed in.

## Example settings files

In the scripts/param_explore/config_files/archive directory are example params_config.sh files. The files named params_config_iz and params_config_tm are for Izhikevich model and Tsodyks-Markram model parameters, respectively. The files named params_config_fp_iz are for seperately testing parameters with firing patterns, and is covered in the "testing firing patterns" documentation. The example files were ones used in the article to create parameter exploration plots. The last part in the filename lists what parameters are explored with the settings file. For example, the file "params_config_iz_a_k.sh" includes settings for exploring Izhikevich a and k parameters.

## Creating a CARLsim project

For running this parameter exploration a unique CARLsim project should be created. This is recommended to be separate from the main spatial navigation project because the parameters in this project will be automatically altered. To avoid accidental changes to parameters, it is useful to have this experiment use a seperate CARLsim project where its parameter changes will only affect its project. Instructions for creating a project are in the "new CARLsim project" documentation. 

It is assumed in the param_config file that the project name will end in a number. This is for the purpose of having multiple similarly named projects, that can test different parameters in each one, and share a name but are listed differently by a number at the end. The first part of the new project filename should be entered into fdr_prefix in the params_config file. The number at the end of the filename is specified with local_run. 

## Clear any old results

Any old plots and text files generated from previous runs of the project should be removed to clean up the directories for the new results to be added.

<br>Delete all ratemap_time_date.png files from /scripts folder
<br>Delete all traj_time_date.png files from /scripts/high_res_traj folder
<br>Delete gridness_score.txt and param_records.txt from /scripts/param_explore/output

## Preparing parameters

Every line of parameters that is to be altered should be formatted to be on a single line instead of listed across multiple lines in the .cpp file. This is to accomidate the pattern matching software finding and changing the parameters at the right places in the code. For example,
<br>sim.setNeuronParameters(MEC_LII_Stellate, 118.000000f, 0.0f, ... 0.0f, 0.0f, 0);
<br>sim.setSTP(MEC_LII_Stellate, EC_LII_Axo_Axonic, true, STPu(0.1672, 0.0f), ... );
<br>grc_to_in_g_fast = 1.050074411;
<br>A note about grc_to_in_g_fast is that it is typically set with a formula but should not be coded in that way for parameter exploration. It should be coded as a number with a whole number and fraction and semicolon after it, e.g., "1.050074411;", for pattern recognition.
<br>
<br>The parameters reformatted in this way for the parameter exploration in the article were setNeuronParameters for MEC_LII_Stellate, setSTP for each grid cell to interneuron connection, and grc_to_in_g_fast. This covered IM parameters for grid cells and TM parameters for grid cell to interneuron connections.
<br>
<br>Parameters should be reset to their standard state before any automated exploration changes occur. It can be useful to save a copy of the file that will have its parameters altered as filename_copy.cpp, and have the parameters at the standard values in the file. Before a new run of parameter exploration occurs the file copy should be used to overwrite the content in the original file which resets the parameters back to the original versions. The same file copy can be used for both IM and TM exploration if it includes standard values for each parameter type. Resetting the values clears out any changes made from previous runs of parameter exploration.

## Backing up settings

It is recommended to put a copy of the params_config file in a backup directory along with a description of what parameters were tested. This can be helpful is re-running the experiment or modifying settings to run it again.

## Settings for the project

Simulation settings should be configured for the CARLsim project to be tested. In the article, as shown in a figure in the main text, settings for the medium-grid scale simulation shown in Fig. 2B were used for parameter explorations. 

### File copies

Typically the files that will need to be copied into the parameter exploration project from a reference porject are:
<br>* All files in the data folder
<br>* All files in the src folder
<br>* Files from the base directory: general_funct.cpp, general_params.cpp, generate_config_state.cpp, move_path.cpp, place_cells.cpp
<br>
Once the project file in the src folder is copied it should be renamed to main_[param_explore_proj_name].cpp where [param_explore_proj_name] is the new project name.

### Experiment settings

Typically the movement path function that will be useful to use for parameter exploration will be move_fullspace. This was used in the article to create parameter exploration plots. Setting the speed that the virtual animal will travel will affect how long the move_fullspace function takes to cover the environment with movement. In the move_fullspace function, setting the variable "speed", or using move_increment in general_params.cpp to automatically set the variable, will specify the speed to be used in the function. In animal data used in the article, a speed of 5 moves per second (mps) was found to be a good balance of (1) time animals were observed to move at that speed in real trajectories and (2) a speed that covers the environment in a reasonable time. The article used the function with a speed of 5 mps with 250000 ms simulated to test the grid score results of each parameter. 

It is potentially useful to create a general_params_copy.cpp file that saves the move_increment setting which can overwrite the project's general_params.cpp file to ensure the correct speed is used in tests. Movement can be set to 5mps by setting move_increment = 0.005.

### Set the correct neuron to be recorded

The correct neuron should be chosen to base plotting on that produces an intended grid pattern firing with move_fullspace. This can be done by setting "sel_nrn" in gridscore_sim_run_local.m. The grid pattern results can be viewed by running the standard project parameters with gridscore_sim_run_local.m. This also reports a gridscore and other statistics. The plot can be used, along with testing different neurons to base the plot on, to see which one creates a grid pattern that is wanted.

### Set place cell input

The pc_to_grc_g_fast and pc_level parameters should be set to appropriate levels for using in the intended parameter exploration grid pattern plot.

## Run the parameter exploration script

After the settings have been configured, the auto_mod_iz.sh or auto_mod_tm.sh script should be run. Optionally, a user can check some files to ensure the process is running as expected. For instance, with a text editor that updates documents in real time, e.g., Sublime Text, a user can open generate_config_state.cpp with that editor (assuming parameters selected are ones that will change in that file), and observe the software change the parameters in real time as the software runs its experiments. Also, the results files, such as gridness_score.txt can be viewed to see the results accumulate over time.

The results that can be created are grid scores in gridness_score.txt, trajectory plots in the high_res_traj directory, rate map plots in the scripts directory, and parameter records in param_records.txt. To save the plots to files ensure the options save_plot and save_traj_plot in gridscore_sim_run_local.m are set to 1. To save the grid scores to the txt file, ensure save_gridscore_file is set 1.

## Plotting

The file gridness_score.txt can be used for plotting grid score results. The script add_gridscore_params.sh will add the parameter values tested to the gridness_score.txt file. A user needs to copy param1_vals and param2_vals from the params_config file in the add_gridscore_params.sh script to prepare it for entering the values. Once that is done the script should be run. gridness_score.txt will then contain the parameter values and can be plotted using the instructions in the parameter exploration plotting documentation.

<details>
<summary>Example supercomputer script</summary>
An example script that could be used to run parameter exploration on a supercomputer is:
<br>filename: slurm_wrapper.sh
<br>#!/bin/bash
<br>#SBATCH --partition=gpuq
<br>#SBATCH --qos=gpu
<br>#SBATCH --gres=gpu:1g.10gb:1
<br>#SBATCH --ntasks-per-node=1
<br>#SBATCH --job-name="pe_tm_1"
<br>#SBATCH --time=0-24:00:00
<br>#SBATCH --output /scratch/username/gc_sim/param_explore_tm_1_results.txt
<br>#SBATCH --mem=5G
<br>./auto_mod_tm.sh
<br>
<br>This computing job is submitted from the directory with auto_mod_tm.sh on the supercomputer so that it will run auto_mod_tm.sh from the directory. For example, the directory could be proj_dir/param_explore_tm_1/scripts/param_explore/.
<br>
<br>The job was submitted by:
<br>$ sbatch ./slurm_wrapper.sh
<br>
<br>Results were checked to see that it was working with:
<br>$ tail /scratch/username/gc_sim/param_explore_tm_1_results.txt
</details>