Full Scale Experiment
=====================

This documentation provides further information about working with the full scale versions of experiments.

## Overall

In general, the full scale project is similar to the small scale project. However, there are some particular differences.

## Differences

#### Lognormal Distributions

The full scale project includes 8 neuron types in contrast to the small scale experiment often only containing 2 neuron types. Each of the 8 neuron types has its own lognormal distribution. This is present in the main file.

#### Group Numbers

The full scale experiment contains additional code in the main file to track the group number of neuron type groups. This is in lines such as `p.CA3_QuadD_LM = CA3_QuadD_LM;`. This becomes useful in functions that want to apply operations to neuron type groups and needs to have the group ids provided for it.

#### Additional Objects Passed to Functions

Functions such as `non_pattern_pres_run` will have several more current vectors and lognormal distribution generator objects passed to it. This is due to the additional neuron types' resources needing to be accounted for in the function's processing. Some of these objects and vectors are stored in the struct named `p` which generally is named for "parameters".

#### Settings for 8 Neuron Types' Properties

The file `generateCONFIGStateSTP.h` there are many settings for neuron type properties for the additional neuron types as compared to the small scale version with 2 neuron types. Neuron properties for the 8 neuron type groups are based on those found in (Kopsick et al., 2024).

#### Difference in Property

(Kopsick et al., 2024) appears to have set `STPu(1.4*0.192566137f` for `setSTP(CA3_Pyramidal, CA3_Pyramidal)`. This is beyond two standard deviations from the mean of Hippocampome's reported STP U value with the conditions: Species: Mice; Gender: Male; Age: P56; Temp: T32; RecMode: Voltage-clamp. The author of this documentation has not heard from Jeffrey about why he set the parameter that way. The parameter is set to the mean, `STPu(0.192566137f`, in the Oblomem simulations, to align it with Hippocampome's parameter values. It seems the simulations perform well enough with that mean value.

#### Scaling Amount

In the file `generateCONFIGStateSTP.h`, the variable `scaling_amount` helps control the scaling of the small- or full-scale sized properties. Setting the `scaling_amount` at `1.0` causes the neuron counts per neuron type group to match the full scale numbers used in (Kopsick et al., 2024). The small scale experiment typically uses 4% of the neuron counts found in the full scale version. This is set by assigning `scaling_amount = 0.04`.

#### Connection Selected for Normalization

Many more synaptic connections exist between neuron type groups given that there are 8 neuron types in this experiment. The normalization code uses a selected connection to perform normalization. That connection should be CA3_Pyramidal -> CA3_Pyramidal. The parameter `norm_weights_connId` in `general_params.cpp` sets which connection number should receive normalization. In the full scale experiment, this connection will typically be number 50. In the small scale experiment, this will typically be connection number 3. Setting this as the wrong connection number will normalize the wrong synapse weights.

#### Computer Resources Needed to Run Full Scale Simulation

Different settings in the slurm file, compared to the small scale version, are needed to run the simulation in full scale. An example slurm_wrapper.sh file used to run the experiment on GMU's supercomputer is:

```
#!/bin/bash
#SBATCH --partition=gpuq
#SBATCH --qos=gpu
#SBATCH --gres=gpu:A100.80gb:1
#SBATCH --job-name="fullfc1"
#SBATCH --time=5-00:00:00
#SBATCH --output /scratch/gmu_user_name/ach_sim/oblomem_fullscale_form_ctx_1_results.txt
#SBATCH --mem=120G
./rebuild.sh
```

In this example, a 80 GB RAM video memory GPU card is used and 120 GB of standard ram is used.

#### Running Experiments in Two Parts

The supercomputer takes approximately 4 days (simulation time) to simulate 7.5 minutes (wall clock time) of the full scale simulation. GMU has a 5-day time limit on continuous running of any software on the Hopper supercomputer. Due to this, completing a 15-min simulated experiment involves loading a stored simulation state after per se 7.5 min, and running the final 7.5 min of simulation in a second Hopper task.

The parameter `load_saved_state` in `general_params.cpp` is included in the full scale experiment to accomidate loading a prior saved state. The parameter `starting_pattern_pres` is used to specify the pattern presentations in the file name used for loading the stored state. The pattern presentations number in the filename no longer directly matches the number of patterns presented in the simulation, and is computed by the formula `pat_pres_num = (t / 1000) * 3`, where `t` is time in milliseconds. This formula was based on a run state index in (Kopsick et al., 2024)'s code but updates in the code no longer make it match the number of patterns presented. Nonetheless, it can be a number used to track the time that the experiment has simulated.

Given the formula above, 15 minutes of wall clock time that has been simulated will have the pattern presentation number of 2700. In practice, the animal recording may have been a small amount more than 15 minutes, specifically ~2 seconds more than 15 minutes. Therefore the pattern number saved in the last save state filename can be 2703. The experiment automatically runs for a number of milliseconds that matches the time points in the animal recording location data `location_x`. It converts each time step into a set number of milliseconds using the `real_to_sim_hz_conversion` parameter. By default, this is ~33.333 milliseconds per location time step (30 Hz sampling rate), and this can be verified or altered given any animal experiment data used in the simulation.

Let's say we want to load a saved state from 7.5 minutes of simulation. In milliseconds, that is 450000 ms. The pattern number will be `(450000/1000)/3 = 1350`. We then set `starting_pattern_pres = 1350` and it will load the saved state from 7.5 minutes of simulation. The line `char file_path[] = "...";`in the main file sets where the filepath of the saved state files are located.

#### Location of Save State Files

Each saved state file can be per se ~5.7 GB of data because the full scale network is so large. It is recommended to save these files on your scratch drive folder in the Hopper computer to avoid taking too much space in your home folder. For instance, one can save them in the path `/scratch/gnu_user_name/ach_sim/oblomem_form_fullscale/network`. The above mentioned `file_path[]` can then point to this directory.

The directory to save saved state files in is set in the line `oss << "file_path" << pat_pres_num << "p.dat";`in general_functs.cpp, where file_path is the path to the saved state files. It is recommended that one make a directory on one's scratch folder for each project that one wants to store saved state files with. This should be done before running the experiment. For example, one can create a "ach_sim" directory in one's `/scratch/gmu_user_name/` folder. Within the "ach_sim" directory, one can create a directory such as `oblomem_form_fullscale` for a fullscale formation experiment. This can be used to store simulation saved state files. In this document author's experiment, the scratch files may be deleted periodically (files older than 90 days) but the folders typically are not. Therefore, one may need to re-run experiments if the files are deleted but the folders typically will not need to be re-created.

## Retrieval aka Test Experiment

A seperate experiment project for full sized simulations has been created to accomidate running test experiments with full- instead of small-scale settings. This has been uploaded to the Github repository in the form of an experiment with context cells, and is named `retrieval_fullscale_ctx_1`. This code can be adapted to test the non-context cell version of the fullscale project, or prior github commits as referenced in work_report.docx (e.g., fig 2) can be used for code to create a non-context-cells retrieval version of a full scale project.

## References

Kopsick, J. D., Kilgore, J. A., Adam, G. C., & Ascoli, G. A. (2024). Formation and retrieval of cell assemblies in a biologically realistic spiking neural network model of area CA3 in the mouse hippocampus. Journal of Computational Neuroscience, 52(4), 303-321.