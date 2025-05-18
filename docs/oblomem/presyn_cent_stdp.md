Presynaptic Centered STDP Development Documentation
===================================================

This documentation provides development documentation about a feature providing presynaptic-centered STDP that was created for the Oblomem neuromodulation research.

## Branches

This feature exists as a feature branch for the Hippocampome branch of CARLsim6, and also a separate feature branch of the main CARLsim6 code base. A version was developed for the main CARLsim6 code base because it could be added as a general feature to CARLsim in the future. Anyone working on the Hippocampome Oblomem project will want to use the Hippocampome branch version if they want to use this feature.

Hippocampome version is [here](https://github.com/nmsutton/CARLsim6/tree/feat/ca3net_more_stdp). CARLsim6 main branch version is [here](https://github.com/nmsutton/CARLsim6/tree/feat/presyn_cent_stdp).

## Multiple CARLsim Branch Installations

A note about the Oblomem Hippocampome research project is that multiple CARLsim6 branches can be installed on the same computer as long as the source code and installation directories are different directories. This allows for simulations within a branch such as this (ca3net_more_stdp) to be run, and a different CARLsim installation (branch) to be present with different features (e.g., nm4stdp) that can run different experiments. The experiments should be run from each CARLsim version's separate "projects" folder and the results will be created in those versions' separate "build" folders. One should be sure to include a different `DCMAKE_INSTALL_PREFIX` directory for the installation directory target with the cmake parameters for building in each CARLsim branch that is installed with cmake. This will cause the different branches to be installed in different directories on one's computer, and avoid any installation overwriting another installation's files.

## Concepts
"It is important to note that CARLsim uses nearest neighbor STDP (Morrison et al. 2008)..." (Beyeler, 2023).

Fig. 7 in (Morrison et al. 2008) shows the difference between symmetric-nearest-neighbor STDP (panel A) and presynaptic-centered-nearest-neighbor STDP (panel B).

The CARLsim user guide's description "...each presynaptic spike is paired with the last postsynaptic spike and each postsynaptic spike is paired with the last presynaptic spike to calculate the final weight change due to STDP." (Beyeler, 2023) indicates that symmetric-nearest-neighbor STDP is included in the official branch of CARLsim. This is due to that quoted description matching very closely to the description in Fig. 7A's caption of (Morrison et al. 2008) that describes symmetric-nearest-neighbor STDP.

Symmetric-nearest-neighbor STDP is not to be confused with symmetric and asymmetric STDP in general. Symmetric in this case specifically refers to the spikes relative to each other in the nearest-neighbor system. Symmetric and asymmetric STDP in general refers to pre before post and post before pre each having their own effects on plasticity.

## Usage and Programming Design

The flag `CARLSIM_PRESYN_CENT_STDP` in CMakeLists.txt in the base CARLsim source code directory should be set to `ON` to enable this presynaptic-centered STDP feature. CARLsim needs to be rebuilt and reinstalled to use this feature once that is set to `ON`. The Github branch for this feature currently has the setting `ON` by default. It is recommended to delete all files in the build directory if CARLsim is to be rebuilt. This will ensure that the reinstallation of CARLsim is done with the intended code included and active.

Once the flag `CARLSIM_PRESYN_CENT_STDP` is set to `ON`, and CARLsim has been rebuilt and reinstalled, STDP that uses EXP_CURVE and ESTDP will use presynaptic centered STDP. The programming and reasons for this are described further below. Currently, other types of STDP are not designed to use presynaptic centered STDP. Further details about using ESTDP with EXP_CURVE are available [here](https://uci-carl.github.io/CARLsim6/ch20_neuromodulation.html#ch20s3_pka_plc_stdp).

## CARLsim6 main Branch Version vs Hippocampome Branch

I will describe methods used to implement the CARLsim6 main branch version of this feature. This can help with reviewing this feature for inclusion as a new feature in a later general CARLsim release. The feature branch for this method is located [here](https://github.com/nmsutton/CARLsim6/tree/feat/presyn_cent_stdp). The version for the Hippocampome branch is [here](https://github.com/nmsutton/CARLsim6/tree/feat/ca3net_more_stdp). The code used to implement the feature is somewhat different in the branches but should implement the feature in the same way.

An important note is that this feature currently is only designed to work with the EXP_CURVE form of STDP and excitatory STDP (ESTDP). Future work can integrate this into other forms of STDP. It perhaps would be a reasonably simple process to extend this work to those other STDP forms.

Code was added to the following files to implement this feature:
```
/carlsim/kernel/src/snn_cpu_module.cpp
/carlsim/kernel/src/gpu_module/snn_gpu_module.cu
/carlsim/kernel/inc/snn_datastructures.h
```
Specifically, the lines with the flag CARLSIM_PRESYN_CENT_STDP are the ones that include the code for this feature. 

The functions `setPostSpikesValue()` and `getPostSpikesPtr()` are designed to store the spike time that a neuron had a postsynaptic spike before a current detected postsynaptic spike.

The function `updateLTP()` typically includes computation of the wtChange due to presynaptic spike time before postsynaptic spike times (pre before post) in STDP. wtChange is a variable involved in computing synapse connection weight changes for neuron pairs due to STDP. This feature adds computation of post before pre spike times and its contribution to wtChange to `updateLTP()`. Specifically, this function computes the most recent post before pre spike time difference when an instance of pre before post spikes is detected in `updateLTP()`. The "presynaptic centered" approach then updates wtChange based on the post time that occured closest to the pre time in both the pre before post and post before pre timings.

This feature disables the processing of wtChange with post before pre spikes in `generatePostSynapticSpike` for ESTDP EXP_CURVE spikes because that is now handled in `updateLTP()`. The default methods of CARLsim is designed for, it appears, symmetric nearest neighbor STDP. Specifically, processing post before pre spikes effecting wtChange in `generatePostSynapticSpike` accomidates that. This feature's alternative STDP method is made possible by handling that wtChange processing in `updateLTP()`.

## Unit Test

A unit test for this feature was created and is located in /carlsim/test6/presyn_cent_stdp.cpp.

## References

Beyeler, M. (2023) Chapter 5: Synaptic Plasticity. https://uci-carl.github.io/CARLsim6/ch5_synaptic_plasticity.html

Morrison, A., Diesmann, M., & Gerstner, W. (2008). Phenomenological models of synaptic plasticity based on spike timing. Biological cybernetics, 98, 459-478.