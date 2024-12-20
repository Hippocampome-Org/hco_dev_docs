Results Recreation
==================

This documentation will cover how to recreate plots observed in (Kopsick et al., 2024) from simulation results data.

## Obtaining Results Data

One should download cell_assembly_formation_retrieval_paper_data.zip from [doi.org/10.5281/zenodo.10870586](https://www.doi.org/10.5281/zenodo.10870586) if one wants to obtain previously generated simulation results that can be plotted.

## Plotting Software

One should download the code from [github.com/jkopsick/cell_assembly_formation_retrieval](https://www.github.com/jkopsick/cell_assembly_formation_retrieval) to obtain the software needed for plotting. A note is that this code requires the Mapping Toolbox in Matlab to work.

## Plotting Software file structure

Subfolders should be created to run the plotting software. The subfolders are:
<br>`figure_2_a_c_data`
<br>`figure_2_b_d_data`
<br>`figure_3_a_data`
<br>`figure_3_b_data`
<br>`figure_3_c_d_data`
<br>`figures_4_thru_7`
<br>`supplementary_figure_3_a_data`
<br>`supplementary_figure_3_b_data`

The Matlab code, e.g., createFigure2Panels.m, should be put into each subfolder that matches the figure name described in the filename. However, filenames that start with "run" should be left in the base directory.

Certain subfolders should have results directories that simulation results files should be moved into. Those subfolders are:
<br>`figure_2_a_c_data`
<br>`figure_2_b_d_data`
<br>`figure_3_a_data`
<br>`figure_3_c_d_data`
<br>`supplementary_figure_3_a_data`
<br>`supplementary_figure_3_b_data`

Each results folder should have the data from the corresponding folder name on Zenodo copied into it. The subfolder figure_3_b_data does not need a results folder but should have the data files network65p_assem275_divnorm.dat, pc_input_pat_1.csv, pc_input_pat_2.csv, and pc_input_pat_3.csv placed into it.

## Generating Plots

One should run CARLsim Offline Analysis Toolkit (OAT) initialization code to load libraries used in the plotting process. This is typically included in CARLsim projects. An example initialization file, initOAT.m, contains a single line: `addpath('path_to_carlsim/tools/offline_analysis_toolbox')` where path_to_carlsim is the path to CARLsim source code. CARLsim4's OAT code has been tested to work with the results files stored on Zenodo but CARLsim6's OAT code may work as well.

The Matlab "run" code in the base directory should be used to create the plots. Specifically, the files that should be used are:
<br>`runFigure2Creation.m`
<br>`runFigure3Creation.m`
<br>`runSupplementaryFigure1Creation.m`
<br>`runSupplementaryFigure3Creation.m`
<br>Running each of those files in Matlab will create the results plots that can be compared to the corresponding figure number in (Kopsick et al., 2024).

## Generating the Figures without Zenodo's Data

This section covers simulations that should be run to recreate the article's figures without using Zenodo's data. In general, the instructions in [simulation_run](https://hco-dev-docs.readthedocs.io/en/latest/pattern_comp_sep/simulation_run.html) should be used to run each simulation. The figures below refer to figures in (Kopsick et al., 2024).

### Figure 2 A and C

The non-overlapping assemblies training pattern should be created. This can be done by running the function `createBetaPatternNoOverlap(275)` in Matlab. The trainPat.csv file produced should be copied into the build folder for the formation simulation project. The line `ifstream is("trainPat.csv")` in the main file will look for that file. If it is not in the build folder then the path will need to be supplied in that line.

The formation simulation should be run with a 275 cell assembly size. The formation project's main file lines with numPyramidalFire and `k += `should have the value 275.

The results of the simulation should be copied into the `results` folder in the `figure_2_a_c_data` directory. For example, the `spk_CA3_Pyramidal.dat` and other .dat files should be copied into that folder. Eventually, runFigure2Creation.m will be run to create plots, but Figure 2 B and D data needs to be updated before that.

### Figure 2 B and D

These figure panels will use the results of the retrieval project. The retrieval project is run with the use of data from the formation project. In the project's main file, `std::ifstream is()` will need to be updated with the path to `trainPat.csv` that was used in the formation project if that file is not copied into the retrieval project's build project folder. `fId = fopen()` will need to be updated with the 65 pattern presentation dat file path, but that should be the version of that file that has been normalized. The steps in `Normalizing Data Structures` of [simulation_run](https://hco-dev-docs.readthedocs.io/en/latest/pattern_comp_sep/simulation_run.html) should be used for that process.

Once the main file is updated with the file paths, and the normalized dat file has been created, then the retrieval project should be run to create results files that will be used for the plots here. The resulting .dat files, e.g., spk_CA3_Basket.dat, should be placed in the `results` folder of the `figure_2_b_d_data` directory.

### runFigure2Creation.m

The file runFigure2Creation.m should be run. The first plot, for panel A, will probably need to be zoomed out to see the full number of cells and the repeating input pattern shown in the figure panel. Plot 4, that matches with panel C, has the inset added in postprocessing and does not appear in the original Matlab plot. This will produce the plots for all panels used in the article's Figure 2.

### Figure 3 A

The same results data created for Figure 2 A and C should be copied into the `results` folder for the `figure_3_a_data` folder. `runFigure3Creation.m` will eventually be run once the relevant data is copied into each figure data folder.

### Figure 3 B

The formation project's normalized 65 pattern presentation dat file should be copied into the `figure_3_b_data` folder. `pc_input_pat_1.csv`, `pc_input_pat_2.csv`, and `pc_input_pat_3.csv` created by the formation project should also be copied into `figure_3_b_data` folder. `createFigure3PanelB.m` that exists in the `figure_3_b_data` may need to have the file name of the normalized file updated if it has changed from the original name. This is done in the `fileNames = []` line.

### Figure C and D

The same results data created for Figure 2 B and D should be copied into the `results` folder for the `figure_3_c_d_data` folder.

### runFigure3Creation.m

The file runFigure3Creation.m should then be run. The plots 1 - 4 match the Figure 3 panels A - D.

### Standard results

Figures 2 and 3 show standard results from the non-overlapping assemblies version of the simulation. Figures 4 - 8 show parameter exploration of the simulation. This exploration requires many more experiments. The creation of these figures is not specifically documented here but is expected to be able to be recreated with the instructions here and in [simulation_run](https://hco-dev-docs.readthedocs.io/en/latest/pattern_comp_sep/simulation_run.html). A user can be considered to successfully be able to run the simulation with the standard settings if that user is able to recreate Figure 2 and 3.