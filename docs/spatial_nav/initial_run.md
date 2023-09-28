Initial run instructions
========================

These are instructions for setting up the software for an initial run

## CARLsim installation

Users need to install the branch named "ca3net" (aka the Hippocampome of CSTP branch) of CARLsim6 ([link](https://github.com/UCI-CARL/CARLsim6/tree/feat/ca3net)) to run this simulation. This was required at the time this documentation was made. Possibly, the Hippocampome branch will be merged with the main branch in the future but there is further development to be done before that. Some conditions should be known about using the hippocampome branch.

A quick overview is:
<br>COBA is enabled by default and should always be used (see [issue #27](https://github.com/UCI-CARL/CARLsim6/issues/27)).
<br>Do not include setConductances() in a line of code (see [issue #27](https://github.com/UCI-CARL/CARLsim6/issues/27)).
<br>setSTP() needs to be set to "true" for any synaptic connection to work (see [issue #27](https://github.com/UCI-CARL/CARLsim6/issues/27)). The 9-parameter and not 3-parameter version of setSTP() should be used.
<br>Spike generator code may not work (see [issue #29](https://github.com/UCI-CARL/CARLsim6/issues/29)).
<br>If using a refractory period (e.g., setting its value to 1 or more in setNeuronParameters()) then a user should only use the RUNGE_KUTTA4 integration method (see [issue #24](https://github.com/UCI-CARL/CARLsim6/issues/24)).
<br>In connect() and other synaptic connection statements a user should only use a delay of 1 (see [issue #20](https://github.com/UCI-CARL/CARLsim6/issues/20)). Setting connection delays greater than one are allowed in the CSTP branch unlike in the main branch CARLsim code. However, there is indicated to be bugs with any delay greater than 1 so that should not be used.
<br>Grid3D neuron sizes are not processed correctly (see [issue #18](https://github.com/UCI-CARL/CARLsim6/issues/18)).
<br>Only the GPU version of CARLsim's processing should be used. For example, when including sim(), a user should only use GPU_MODE not CPU_MODE (see [issue #18](https://github.com/UCI-CARL/CARLsim6/issues/18)). Code for the Hippocampome features has only been developed for GPUs and will not work correctly with CPU.
<br>Only use CARLsim6 not a different CARLsim version (see [issue #18](https://github.com/UCI-CARL/CARLsim6/issues/18)). The code has not been developed for any other version.
<br>
<br>While these issues exist, a user can just avoid using code that they instruct to avoid, and experiments such as those in the article can be performed correctly.

## Directory structure

Some additional folders will need to be created that are omitted from the Github files to save storage space. The directories for the project should be ("\*" represents a new directory):

Project directory:
<br>/build_dir \*
<br>/data
<br>/output \*
<br>/output/spikes \*
<br>/results \*
<br>/scripts
<br>/scripts/videos \*
<br>/src
<br>/standalone

The new directories should be located in a place in a user's hard drive where sufficient space for up to gigabytes of results will be stored. Standard running of the simulation will only produce limited megabytes of results output. Running the simulation with neuron monitors that record voltage and current for up to 128 neurons, including running the simulation for hours of simulated time, can result in large results files being created. Users can choose a storage folder for such large data on their machines and create softlinks to the folders in the simulation project directory so that the data will be stored appropriately. This can be especially useful when multiple copies of this project are created for running seperate experiments and a storage location is wanted where sufficient space is availible.

The build_dir directory should be a softlink to were the build directory (as opposed to the project directory) is located for the project in CARLsim.

The output directory will store virtual animal movement position records. A subdirectory "spikes" will be required to be made in this directory. A softlink can be made to where this directory should be stored on a user's computer. 

The results directory will store output files such as spike recordings and neuron reader output. A softlink should be made to where this is located.

The videos directory will store plot videos created by the Matlab software. It can be a softlink to another storage directory.

Build directory:
<br>/data \*
<br>/output \*
<br>/results \*

The data directory should be a softlink to the data folder in the project directory.

The output and results directories should be the same softlinks as in the project folder. A user has the option of making these folders located in the build directory instead of softlinks to another storage location if wanted.

## File structure

Some additional files are needed than what are included in the Github files, and those were omitted to save storage space. Additional files needed:

<br>/data/anim_angles.cpp \*
<br>/data/anim_angles.csv \*
<br>/data/anim_speeds.cpp \*
<br>/data/anim_speeds.csv \*
<br>/data/ext_dir.cpp
<br>/data/ext_dir_initial.cpp
<br>/data/ext_dir_initial_rot_l.cpp
<br>/data/ext_dir_initial_rot_r.cpp
<br>/data/grc_to_in_conns.csv \*
<br>/data/init_firings.cpp
<br>/data/synapse_weights.csv \*

anim_angles.cpp, anim_angles.csv, anim_speeds.cpp, and anim_speeds.csv are reformatted data files that store data on animal movement speeds and angles.

grc_to_in_conns.csv is a file that lists which GrC -> IN layer connections should be present. It can be automatically generated by the simulation but saves time in running the software for it to be precomputed.

synapse_weights.csv is a file that likes weight values for IN -> GrC connections. It is generated through a Matlab weight distribution builder script.

## New project

A new CARLsim project should be created for this software. A script is provided to automate the creation of the project. Instructions for creating a project are in the "new CARLsim project" documentation.

## Animal data

Note: "import_animal_data" in general_params.cpp should be set to "#define import_animal_data 0" if real animal data is not included for the software. Otherwise, a file import error will occur.

## Building required files

See the building required files documentation to build files needed to run the simulation.

## Data directory link

A user should create a softlink to the data directory in the project folder in CARLsim's build folder. In linux this can be done by:
<br>$ ln -s project_data_directory build_directory

## Output directory

In the build folder a directory named "output" is needed. That folder needs to have a directory named "spikes" in it.

## Output, results, and build_dir links

The project folder must contain links named "output" and "results" to folders with those same names in the build directory. Some scripts may require a softlink named "build_dir" in the project directory that links to the build directory.