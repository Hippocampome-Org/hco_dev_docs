Presynaptic Centered STDP Development Documentation
===================================================

This documentation provides development documentation about a feature providing presynaptic-centered STDP that was created for the Oblomem neuromodulation research.

## Concepts
"It is important to note that CARLsim uses nearest neighbor STDP (Morrison et al. 2008)..." (Beyeler, 2023).

Fig. 7 in (Morrison et al. 2008) shows the difference between symmetric-nearest-neighbor STDP (panel A) and presynaptic-centered-nearest-neighbor STDP (panel B).

The CARLsim user guide's description "...each presynaptic spike is paired with the last postsynaptic spike and each postsynaptic spike is paired with the last presynaptic spike to calculate the final weight change due to STDP." (Beyeler, 2023) indicates that symmetric-nearest-neighbor STDP is included in the official branch of CARLsim. This is due to that quoted description matching very closely to the description in Fig. 7A's caption of (Morrison et al. 2008) that describes symmetric-nearest-neighbor STDP.

Symmetric-nearest-neighbor STDP is not to be confused with symmetric and asymmetric STDP in general. Symmetric in this case specifically refers to the spikes relative to each other in the nearest-neighbor system. Symmetric and asymmetric STDP in general refers to pre before post and post before pre each having their own effects on plasticity.

## Usage
<br>TODO: I will work on creating more of this documentation soon.

## Programming Design

The flag `CARLSIM_PRESYN_CENT_STDP` in CMakeLists.txt in the base CARLsim source code directory should be set to `ON` to enable this presynaptic-centered STDP feature. CARLsim needs to be rebuilt and reinstalled to use this feature once that is set to `ON`. The Github branch for this feature currently has the setting `ON` by default.

## CARLsim6 main Branch Version vs Hippocampome Branch

I will describe methods used to implement the CARLsim6 main branch version of this feature. This can help with reviewing this feature for inclusion as a new feature in a later general CARLsim release. The feature branch for this method is located [here](https://github.com/nmsutton/CARLsim6/tree/feat/presyn_cent_stdp). The version for the Hippocampome branch is [here](https://github.com/nmsutton/CARLsim6/tree/feat/ca3net_more_stdp). The code used to implement the feature is somewhat different in the branches but should implement the feature in the same way.

An important note is that this feature currently is only designed to work with the EXP_CURVE form of STDP and excitatory STDP (ESTDP). Future work can intrgrate this into other forms of STDP. It perhaps would be a reasonably simple process to extend this work to those other STDP forms.

Code was added to the following files to implement this feature:
```
/carlsim/kernel/src/snn_cpu_module.cpp
/carlsim/kernel/src/gpu_module/snn_gpu_module.cu
/carlsim/kernel/inc/snn_datastructures.h
```
Specifically, the lines with the flag CARLSIM_PRESYN_CENT_STDP are the ones that include the code for this feature. 

The functions `setPostSpikesValue()` and `getPostSpikesPtr()` are designed to store the spike time that a neuron had a postsynaptic spike before a current detected postsynaptic spike.

The function `updateLTP()` typically includes computation of the wtChange due to presynaptic spike time before postsynaptic spike times (pre before post) in STDP. wtChange is a variable involved in computing synapse connection weight changes for neuron pairs due to STDP. This feature adds computation of post before pre spike times and its contribution to wtChange to `updateLTP()`. Specifically, this function computes the most recent post before pre spike time difference when an instance of pre before post spikes is detected in `updateLTP()`. The "presynaptic centered" approach then updates wtChange based on the post time that occured closest to the pre time in both the pre before post and post before pre timings.

This feature disables the processing of wtChange with post before pre spikes in `generatePostSynapticSpike` for ESTDP EXP_CURVE spikes because that is now handled in `updateLTP()`. The default methods of CARLsim as designed for, it appears, symmetric nearest neighbor STDP. Specifically, processing post before pre spikes effecting wtChange in `generatePostSynapticSpike` accomidates that. This feature's alternative STDP method is made possible by handling that wtChange processing in `updateLTP()`.

## Unit Test

A unit test for this feature was created and is located in /carlsim/test6/presyn_cent_stdp.cpp.

## References

Beyeler, M. (2023) Chapter 5: Synaptic Plasticity. https://uci-carl.github.io/CARLsim6/ch5_synaptic_plasticity.html

Morrison, A., Diesmann, M., & Gerstner, W. (2008). Phenomenological models of synaptic plasticity based on spike timing. Biological cybernetics, 98, 459-478.