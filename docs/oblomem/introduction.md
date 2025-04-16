Introduction to Oblomem
=======================

This documentation provides an introduction to working with the Oblomem software.

## Organization

Oblomem can be downloaded from [here](https://github.com/Hippocampome-Org/oblomem).

The software is separated into folders representing different experiments and tools. The folder structure, along with descriptions, currently is:
<table>
<tr><td>folder_name</td><td>description</td></tr>
<tr><td>behavior_metrics</td><td>Software for converting animal recordings into a format that can be processed in simulations.</td></tr>
<tr><td>formation</td><td>Formation, aka training, experiment without context cells. Currently this project focuses on STDP modulation.</td></tr>
<tr><td>formation_ctx_1</td><td>Formation, aka training, experiment with context cells. This is the phase 1 experiment.</td></tr>
<tr><td>formation_ctx_2</td><td>Formation, aka training, experiment with context cells. This is the phase 2 experiment.</td></tr>
<tr><td>formation_fullscale_ctx_1</td><td>Full scale version of the formation, aka training, experiment with context cells. This is the phase 1 experiment.</td></tr>
<tr><td>general_analyses</td><td>These are a variety of scripts used to create plots and statistics.</td></tr>
<tr><td>retrieval</td><td>Retrieval, aka testing, experiment without context cells.</td></tr>
<tr><td>retrieval_ctx</td><td>Retrieval, aka testing, experiment with context cells.</td></tr>
<tr><td>retrieval_fullscale_ctx_1</td><td>Fullscale version of Retrieval, aka testing, experiment with context cells.</td></tr>
</table>

## Terminology

The sample experiment in (Moghadam et al., 2025) is the phase 1 experiment version in Oblomem. The test experiment in (Moghadam et al., 2025) is the phase 2 experiment. This should not be confused with Oblomem's test experiment that is a manually controlled pattern presentation and short time experiment (e.g., 2 seconds wall clock time) that is used to test pattern completion.

## Installation



References:

Moghadam, F. F., Guzman, B. E. G., Zheng, X., Parsa, M., Hozyen, L. M., & Dannenberg, H. (2025). Cholinergic dynamics in the septo-hippocampal system provide phasic multiplexed signals for spatial novelty and correlate with behavioral states. BioRxiv, 2025-01.