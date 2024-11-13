Results Recreation
==================

This documentation will cover how to recreate plots observed in (Kopsick et al., 2024) from simulation results data.

# Obtaining Results Data

One should download cell_assembly_formation_retrieval_paper_data.zip from [doi.org/10.5281/zenodo.10870586](doi.org/10.5281/zenodo.10870586) if one wants to obtain previously generated simulation results that can be plotted.

# Plotting Software

One should download the code from [github.com/jkopsick/cell_assembly_formation_retrieval](github.com/jkopsick/cell_assembly_formation_retrieval) to obtain the software needed for plotting.

# Plotting Software file structure

Subfolders should be created to run the plotting software. The subfolders are:
`figure_2_a_c_data`
<br>`figure_2_b_d_data`
<br>`figure_3_a_data`
<br>`figure_3_b_data`
<br>`figure_3_c_d_data`
<br>`figures_4_thru_7`
<br>`supplementary_figure_3_a_data`
<br>`supplementary_figure_3_b_data`

The Matlab code, e.g., createFigure2Panels.m, should be put into each subfolder that matches the figure name described in the filename. However, filenames that start with "run" should be left in the base directory.

Certain subfolders should have results directories that simulation results files should be moved into. Those subfolders are:
`figure_2_a_c_data
figure_2_b_d_data
figure_3_a_data
figure_3_c_d_data
supplementary_figure_3_a_data
supplementary_figure_3_b_data`

Each results folder should have the data from the corresponding folder name on Zenodo copied into it. The subfolder figure_3_b_data does not need a results folder but should have the data files network65p_assem275_divnorm.dat, pc_input_pat_1.csv, pc_input_pat_2.csv, and pc_input_pat_3.csv placed into it.

# Generating Plots

One should run CARLsim Offline Analysis Toolkit (OAT) initialization code to load libraries used in the plotting process. This is typically included in CARLsim projects. An example initialization file, initOAT.m, contains a single line:
`addpath('path_to_carlsim/tools/offline_analysis_toolbox')`
where path_to_carlsim is the path to CARLsim source code. CARLsim4's OAT code has been tested to work with the results files stored on Zenodo but CARLsim6's OAT code may work as well.

The Matlab "run" code in the base directory should be used to create the plots. Specifically, the files that should be used are:
`runFigure2Creation.m
runFigure3Creation.m
runSupplementaryFigure1Creation.m
runSupplementaryFigure3Creation.m`
Running each of those files in Matlab will create the results plots that can be compared to the corresponding figure number in (Kopsick et al., 2024).