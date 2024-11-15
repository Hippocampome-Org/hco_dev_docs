Simulation Running
==================

This documentation covers methods to use when running the pattern formation and retrieval simulation. The source code for the projects available [here](https://github.com/jkopsick/simulate_formation_and_retrieval). Anyone who has feedback on it should contact Dr. Jeffrey Kopsick [link](https://www.linkedin.com/in/jeffrey-kopsick-phd-315795b3/), Dr. Nate Sutton [link](https://www.linkedin.com/in/nate-sutton-0a814a45/), or coauthors on the (Kopsick et al., 2024) article. Github's Issues page can be use in the case that any possible issues arise.

## Projects

The simulation code is separated into two projects. One is the pattern formation code and the other is the pattern retrieval code. The names of the projects could be, for instance, `ca3_lognormalI_09_05_23_assem_275` for the formation code and `ca3_test_patcomp_lognormalI_09_05_23_assem_275` for the retrieval code.

## Generating Pattern Formation Data

numPyramidalFire sets the target assembly size in the main C++ file, e.g., main_ca3_lognormalI_09_05_23_assem_275.cpp, of the pattern formation simulation. It needs to be updated in each line it exists in the main file if one wants to change the assembly size. The value it is set equal to is the intended assembly size.

The number after the word "network" in the .dat filename specifies the number of times a pattern is presented to cell assemblies. For example, network95p.dat describes the file containing the simulation state after 95 pattern presentations.

### Pattern Overlaps

createBetaPatternOverlap.m or createBetaPatternNoOverlap.m from the [analysis code](https://github.com/jkopsick/cell_assembly_formation_retrieval/tree/main/matlab_analysis_visualization_code) can create a csv file that is used for training the simulation. These generate the file `trainPat.csv` and the overlap version creates more csv files for use in the retrieval project.

createBetaPatternOverlap.m's run arguments are patSize and ovPct. patSize sets the assembly size. ovPct sets the overlap percentage. ovPct should be set with a value between 0 and 1, e.g., 3% overlap is set with 0.03. The lines with `ceil(patSize*ovPct)` in createBetaPatternOverlap.m can be set to `floor()` instead of `ceil()` depending on the number of neurons in the assembly. A goal is to have uqPat1, uqPat2, and uqPat3 have all the same assembly size for typical use of the simulation and this may require changing `ceil()` to `floor()` in some cases.

## Normalizing Data Structures

The .dat files that are produced by the pattern formation simulation should be run through normalization code in Python before being supplied as input to the pattern retrieval simulation. The normalization code will need to be run on a system with a large amount of RAM memory such as a supercomputer. For example, the code may need 60 GB of RAM to run successfully with the data from the full sized network simulation of the CA3 pattern formation.

The data normalization code can either be run from the modifyMat.ipynb Jupyter notebook file or the code in that notebook file can be run seperately in Python without the notebook. The path to the .dat simulation save file will need to be updated in the code. For example, `sim = Simulation('path_to_data/network40p.dat', debug=True)` will need to be updated with path_to_data representing the path to the .dat file. Also, the path to the output file will need to be updated. For example, `sim.save('path_to_data/network40p_ov_five_pct_assem75_divnorm.dat',debug=True)` will need to be updated with path_to_data being the path to the output file.

## Using Normalized Data

The data can be run in the pattern retrieval simulation after the data is normalized. The build folder for the pattern formation simulation should contain the .dat normalized data. The file path to the .dat normalized data should be entered into the main file of the pattern retrieval code. This is entered as what fId is equal to. For example, `fId = fopen("path_to_data/network65p.dat", "rb")`.

## Changing Neuron Degradation Amounts

The line `if (randPyrCell < k + 138)` can have its number changed (k_plus_number) to change what number of neurons have degradation. This number is originally about 50% of a 275 cell assembly. The number of cells in the k_plus_number are the number degraded. For instance, setting it to 28 would cause about 10% degradation in an assembly of size 275.

## Plotting and Analyzing Results

Users can use the software listed in [results recreation](https://hco-dev-docs.readthedocs.io/en/latest/pattern_comp_sep/results_recreation.html) to plot results. Data files generated from new runs of the simulations supply files that can be used in place of the ones listed in prior created files on Zenodo [link](doi.org/10.5281/zenodo.10870586). Figures and descriptions in (Kopsick et al., 2024) can be used to guide what the meaning of each plot is.