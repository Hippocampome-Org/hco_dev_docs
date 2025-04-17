Introduction to Oblomem
=======================

This documentation provides an introduction to working with the Oblomem software.

## Requirements

One needs to use the ca3net_nm4cstp branch of CARLsim if one plans to use the features of STP neuromodulation with setNM4STP() and updating neuromodulation levels at runtime with updateNM4Levels(). This branch is located [here](https://github.com/nmsutton/CARLsim6/tree/feat/ca3net_nm4cstp).

One needs to use the ca3net_more_stdp branch of CARLsim if one wants to use the features of presynaptic centered nearest neighbor STDP (CARLSIM_PRESYN_CENT_STDP in CMakeLists.txt) and updateNM4Levels(). This branch is located [here](https://github.com/nmsutton/CARLsim6/tree/feat/ca3net_more_stdp).

In general:
<br>The context cells theory is currently looking most promising (as of April '25) and therefore it is recommended to use the ca3net_nm4cstp for investigation of that. In general, the ca3net_nm4cstp branch is recommended for use. The ca3net_more_stdp can be used for a more specialty purpose currently, and is not recommended for use unless its specific features are needed.

One can have more than one installation of CARLsim6 as long as the source code, build, and install directories are in different locations. For example, one can have both the ca3net_nm4cstp and ca3net_more_stdp branches installed on one's Hopper account as long as these directories are different for each branch. One can then run projects with each branch's projects directory and have them run using the branch's version of CARLsim.

Instructions for installing CARLsim6 in general are [here](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/carlsim_installation.html).

## Organization

Oblomem can be downloaded from [here](https://github.com/Hippocampome-Org/oblomem).

The software is separated into folders representing different experiments and tools. The folder structure, along with descriptions, currently is:
<table>
<tr><td>Folder Name</td><td>Description</td></tr>
<tr><td>behavior_metrics</td><td>Software for converting animal recordings into a format that can be processed in simulations.</td></tr>
<tr><td>formation</td><td>Formation, aka training, experiment without context cells. Currently this project focuses on STDP modulation.</td></tr>
<tr><td>formation_ctx_1</td><td>Formation, aka training, experiment with context cells. This is the phase 1 experiment.</td></tr>
<tr><td>formation_ctx_2</td><td>Formation, aka training, experiment with context cells. This is the phase 2 experiment.</td></tr>
<tr><td>formation_fullscale_ctx_1</td><td>Full scale version of the formation, aka training, experiment with context cells. This is the phase 1 experiment.</td></tr>
<tr><td>general_analyses</td><td>These files are a variety of scripts used to create plots and statistics.</td></tr>
<tr><td>general_tools</td><td>These files are a variety of tools to help with experimentation.</td></tr>
<tr><td>retrieval</td><td>Retrieval, aka testing, experiment without context cells.</td></tr>
<tr><td>retrieval_ctx</td><td>Retrieval, aka testing, experiment with context cells.</td></tr>
<tr><td>retrieval_fullscale_ctx_1</td><td>Fullscale version of Retrieval, aka testing, experiment with context cells.</td></tr>
</table>

## Terminology

The sample experiment in (Moghadam et al., 2025) is the phase 1 experiment version in Oblomem. The test experiment in (Moghadam et al., 2025) is the phase 2 experiment. This should not be confused with Oblomem's test experiment that is a manually controlled pattern presentation and short time experiment (e.g., 2 seconds wall clock time) that is used to test pattern completion.

## Installation

These instructions will focus on installing the software on the GMU supercomputer Hopper. Methods in them can also be adapted to install the software on a local computer. These instructions assume that CARLsim6 is installed on the supercomputer. As stated above, CARLsim6 install instructions are [here](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/carlsim_installation.html).

### Create Project

A new CARLsim project should be created. This can be done by running the script general_tools/CS6_new_proj/init_cs6_supcomp.sh. That script can be run with no command line arguments (input) to get a description of arguments needed. Example code:
<br>`./init_cs6_supcomp.sh oblomem_form_ctx_1 /home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/projects /home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/.build gmu_username gmu_username@gmu.edu oblomem_form_ctx_1_results.txt`
<br>This creates the project name oblomem_form_ctx_1 in the CARLsim6 install folder CARLsim6-feat-ca3net-nm4cstp.
<br>One should make sure a compiled file with the correct project name is in the build folder. If it is not then one will need to manually run the `make` command.

### Copying Project Files

One should copy some files/folders from the Oblomem downloaded code into the new Hopper project folder (using "oblomem_form_ctx_1" as an example).

For the project folder copy:
<br>general_functs.cpp
<br>general_params.cpp
<br>generateCONFIGStateSTP.h
<br>generateSETUPStateSTP.h
<br>/src/main_oblomem_form_ctx_1.cpp
<br>/data/

If any files of the same name are in the new project folder then they should be overwritten with the copied files data.

Note: if the main_project_name.cpp file is a different name in the new project folder then one needs to rename that main file once it is copied to Hopper. The file name of the copied file should be the main file name on Hopper, and the copied file should replace the main file.

A "softlink" should be made to the project's build directory from the project directory. For example:
<br>`ln -s /home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/.build/projects/oblomem_form_ctx_1/ build_dir`

For the build folder copy:
<br>trainPat.csv
<br>trainPatPlaceCells.csv
<br>Note: these are found in the build folder of the Oblomem project folder source code.

Create a rebuild.sh file:
<br>`echo "make clean && make && ./oblomem_form_ctx_1" > rebuild.sh`
<br>`chmod +x ./rebuild.sh`
Note: oblomem_form_ctx_1 should be replaced with the project name used.

Create a slurm_wrapper.sh file. An example of this is:
```
#!/bin/bash
#SBATCH --partition=gpuq
#SBATCH --qos=gpu
#SBATCH --gres=gpu:1g.10gb:1
#SBATCH --job-name="olmform1"
#SBATCH --time=0-01:00:00
#SBATCH --output /scratch/gmu_username/ach_sim/oblomem_form_1_results.txt
#SBATCH --mem=5G
./rebuild.sh
```

An output folder for the Oblomem project should be made if it does not already exist. Go to one's scratch folder:
<br>`cd /scratch/gmu_username/`
<br>Create the output folder:
<br>`mkdir ach_sim`
<br>One should note that in the slurm_wrapper.sh file the `--output` line directs to this new output folder. The results log of running the software will be stored in this txt file. If one wants to view that file, one will want to view this file in software such as notepad++ in Windows or Sublime Text in Linux to make scrolling through the file easier.

An additional output folder should be made specifically for the project:
<br>`cd ach_sim`
<br>`mkdir oblomem_form_ctx_1`
<br>Note: oblomem_form_ctx_1 should be replaced with one's project name

### Specifying Network Save Files Output
One should update a line in general_functs.cpp in the project directory. This line is:
<br>`oss << "network" << pat_pres_num << "p.dat";`
<br>This should be updated to direct to the new project output folder. For example:
<br>`oss << "/scratch/gmu_username/ach_sim/oblomem_form_ctx_1/network" << pat_pres_num << "p.dat";`
<br>This will be helpful for storing large output files on the scratch drive instead of one's home directory. One's home directory can run out of space if files taking up too much memory are stored there.

### Softlink to Data Directory
Create a softlink in the project's build directory to the project's data directory. For example:
<br>`ln -s /home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/projects/oblomem_form_ctx_1/data/ data`

### Check that lat_inh_conns.csv Can Be Found
Make a folder in your home git directory named ach_sim_conns. E.g., `/home/gmu_username/git/ach_sim_conns`. The file `lat_inh_conns.csv` is produced by the `oblomem_connections.m` script and specifies lateral inhibition connectivity. It is located at `/oblomem/general_analyses/lat_inhib_conns/lat_inh_conns.csv`. Copy the file `lat_inh_conns.csv` into the new `ach_sim_conns` directory.
<br>The line with lat_inh_conns.csv in the main project file should be changed to this:
<br>`ParseCSV("/home/gmu_username/git/ach_sim_conns/lat_inh_conns.csv", &lat_inh_conns);`

### Running the Experiment
One should go to the project's build directory. For example:
<br>`/home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/.build/projects/oblomem_form_ctx_1`
<br>Then submit the job to the supercomputer to run:
<br>`sbatch ./slurm_wrapper.sh`

## References

Moghadam, F. F., Guzman, B. E. G., Zheng, X., Parsa, M., Hozyen, L. M., & Dannenberg, H. (2025). Cholinergic dynamics in the septo-hippocampal system provide phasic multiplexed signals for spatial novelty and correlate with behavioral states. BioRxiv, 2025-01.