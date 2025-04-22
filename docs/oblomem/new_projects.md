New Project Creation and Installation
====================================

This documentation provides further information about creating and installing new project used for experiments in Oblomem.

### Create Project

A new CARLsim project should be created. This can be done by running the script general_tools/CS6_new_proj/init_cs6_supcomp.sh. That script can be run with no command line arguments (input) to get a description of arguments needed. Example code:
<br>`./init_cs6_supcomp.sh oblomem_form_ctx_1 /home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/projects /home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/.build gmu_username gmu_username@gmu.edu oblomem_form_ctx_1_results.txt`
<br>This creates the project name oblomem_form_ctx_1 in the CARLsim6 install folder CARLsim6-feat-ca3net-nm4cstp.
<br>One should make sure a compiled file with the correct project name is in the build folder. If it is not then one will need to manually run the `make` command.

### Copying Project Files

#### Copy Project Folder Files
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

#### Copy Build Folder Files
For the build folder copy:
<br>trainPat.csv
<br>trainPatPlaceCells.csv
<br>Note: these are found in the build folder of the Oblomem project folder source code.
<br>See [create_train_pat](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/create_train_pat.html). for more info. about creating new versions of these csv files if that is wanted.

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
#SBATCH --time=0-03:00:00
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

### Update Save State Loading If Needed
If the project being run needs to load a saved simulation state from another experiment then the location that saved state is located needs to be updated. Experiments that need this are typically phase 2 or later experiments that load results from phase 1. Phase 1 experiments may not contain code to load a saved state because they don't use a saved state. The location path is typically located in the project's main file in the line `file_path[]` in the if statement `p.load_saved_state == true`. This line should be updated to direct `file_path` to the folder with the `.dat` saved simulation state file. The variable `starting_pattern_pres`, in the file `general_params.cpp`, sets which numbered saved state to load in projects that load saved states. One should ensure the correct numbered saved state is set to be loaded.

### Running the Experiment
One should go to the project's build directory. For example:
<br>`/home/gmu_username/git/CARLsim6-feat-ca3net-nm4cstp/.build/projects/oblomem_form_ctx_1`
<br>Then submit the job to the supercomputer to run:
<br>`sbatch ./slurm_wrapper.sh`

### Plotting Results

One will need to download the results folder from the experiment when it has completed running. One can then plot the results according to the [plotting instructions](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/plotting.html).