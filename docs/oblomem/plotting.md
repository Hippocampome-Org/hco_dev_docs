Creating Plots
==============

This documentation provides further information about plotting results of Oblomem experiments.

## Plotting Folders

The folders included for creating plots are in /general_analyses/plotting/. One will need to copy or link a CARLsim results folder into each subfolder within this directory to create plots. The main plot of interest is created with the createFigure3PanelD.m script. These files are adapted from github.com/jkopsick/cell_assembly_formation_retrieval as a part of (Kopsick et al., 2024).

## Example Plot Creation

### initOAT.m

One must initialize CARLsim's Offline Analysis Toolkit (OAT) to use functions that process CARLsim data. This is done by the line `initOAT` in `runFigure2Creation.m` and `runFigure3Creation.m`. It is recommended to use the CARLsim4 version of OAT, because that has better compatibility with these plotting scripts. One should aquire the CARLsim4 OAT by downloading the CARLsim4 source code from [here](https://github.com/UCI-CARL/CARLsim4). The folder /tools/offline_analysis_toolbox/ contains the OAT. One does not need to keep any other CARLsim4 files than the offline_analysis_toolbox folder. One should keep the offline_analysis_toolbox folder in a place on their computer for perminant storage. One then needs to edit the `addpath()` line in initOAT.m in the `plotting` folder to direct to the location of the offline_analysis_toolbox folder. This folder's files will then be loaded when the initOAT script is run.

### Generating a Plot

One should copy or link (e.g., softlink) a CARLsim results folder into the `figure_3_c_d_data` folder. One then runs `runFigure3Creation.m` from within Matlab and the plot should be created.

A default parameter is `assemSize = 150;`. This assumes the ensemble size plotted will be 150 neurons per ensemble. Another default is `number_of_patterns = 8;`, and this will effect the number of patterns plotted. `number_of_patterns` can alternatively be set to `3` or `6`.

Many lines with neuron types other than CA3_Pyramidal or CA3_Basket are commented out. This is because the settings by default are set to use the small scale experiment version's neuron types. These settings are compatible with the small or large scale experiment results, but will be missing some neuron types from the plot if the experiment contains more neuron types. One can uncomment these additional lines containing other neuron types, if the results contain those, and those neuron types are wanted to be plotted.

### Other Analyses and Plots

By default, `createFigure3PanelB` is commented out. One can uncomment this to use that code to inspect synapse connections and weights in Matlab if wanted. That script will use a CARLsim results folder in the figure_3_b_data folder.

If wanted, one can create other plots with this software. See (Kopsick et al., 2024) for details, and figure examples, of other plots that can optionally be created.

## Testing Half Pattern Presentation

Some versions of the retrieval (test) simulation have a `present_full_pattern` parameter that can be set to `false` to test the presentation of half patterns. In the function present_pattern(), code such as:
```
	for (int j = g; j < g + numIrrG; j++) {
		...
		else {
			if (p->present_full_pattern == false && randPyrCell < k + floor(p->assembly_size/2)) {
				pc_current->at(randPyrCell) = p->pattern_current_val;
			}
			else if (p->present_full_pattern == true) {
				pc_current->at(randPyrCell) = p->pattern_current_val;
			}
		}
	}
```
can be used to implement half pattern presentation.

## Reference

Kopsick, J. D., Kilgore, J. A., Adam, G. C., & Ascoli, G. A. (2024). Formation and retrieval of cell assemblies in a biologically realistic spiking neural network model of area CA3 in the mouse hippocampus. Journal of Computational Neuroscience, 52(4), 303-321.