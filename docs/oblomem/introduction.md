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

## CARLsim6 Installation

These instructions will focus on installing the software on the GMU supercomputer Hopper. Methods in them can also be adapted to install the software on a local computer. These instructions assume that CARLsim6 is installed on the supercomputer. As stated above, CARLsim6 install instructions are [here](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/carlsim_installation.html).

## New Project Creation and Installation

Instructions for creating and installing new projects for experiments with Oblomem are [here](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/new_projects.html).

## References

Moghadam, F. F., Guzman, B. E. G., Zheng, X., Parsa, M., Hozyen, L. M., & Dannenberg, H. (2025). Cholinergic dynamics in the septo-hippocampal system provide phasic multiplexed signals for spatial novelty and correlate with behavioral states. BioRxiv, 2025-01.