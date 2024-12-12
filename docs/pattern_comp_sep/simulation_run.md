Simulation Running
==================

This documentation covers methods to use when running the pattern formation and retrieval simulation. The source code for the projects available [here](https://github.com/jkopsick/simulate_formation_and_retrieval). Anyone who has feedback on it should contact Dr. Jeffrey Kopsick [link](https://www.linkedin.com/in/jeffrey-kopsick-phd-315795b3/), Dr. Nate Sutton [link](https://www.linkedin.com/in/nate-sutton-0a814a45/), or coauthors on the (Kopsick et al., 2024) article. Github's Issues page can be used in the case that any possible issues arise.

## Projects

The simulation code is separated into two projects. One is the pattern formation code and the other is the pattern retrieval code. The names of the projects could be, for instance, `ca3_lognormalI_09_05_23_assem_275` for the formation code and `ca3_test_patcomp_lognormalI_09_05_23_assem_275` for the retrieval code.

## Generating Pattern Formation Data

numPyramidalFire sets the target assembly size, e.g., 275, in the main C++ file, e.g., main_ca3_lognormalI_09_05_23_assem_275.cpp, of the pattern formation simulation. It needs to be updated in each line it exists in the main file if one wants to change the assembly size. The value it is set equal to is the intended assembly size. The lines with `k += ` should also use the assembly size value.

The number after the word "network" in the .dat filename specifies the number of times a pattern is presented to cell assemblies. For example, network95p.dat describes the file containing the simulation state after 95 pattern presentations.

### Pattern Overlaps

createBetaPatternOverlap.m or createBetaPatternNoOverlap.m from the [analysis code](https://github.com/jkopsick/cell_assembly_formation_retrieval/tree/main/matlab_analysis_visualization_code) can create a csv file that is used for training the simulation. These generate the file `trainPat.csv` and the overlap version creates more csv files for use in the retrieval project.

createBetaPatternNoOverlap.m is run with an argument of the target assembly size. For example, `createBetaPatternNoOverlap(275)` creates a trainPat.csv file for a 275 cell assembly size.

createBetaPatternOverlap.m's run arguments are patSize and ovPct. patSize sets the assembly size. ovPct sets the overlap percentage. ovPct should be set with a value between 0 and 1, e.g., 3% overlap is set with 0.03. The lines with `ceil(patSize*ovPct)` in createBetaPatternOverlap.m can be set to `floor()` instead of `ceil()` depending on the number of neurons in the assembly. A goal is to have uqPat1, uqPat2, and uqPat3 have all the same assembly size for typical use of the simulation and this may require changing `ceil()` to `floor()` in some cases.

## Normalizing Data Structures

The .dat files that are produced by the pattern formation simulation should be run through normalization code in Python before being supplied as input to the pattern retrieval simulation. The normalization code will need to be run on a system with a large amount of RAM memory such as a supercomputer. For example, the code may need 60 GB of RAM to run successfully with the data from the full sized network simulation of the CA3 pattern formation.

The data normalization code can either be run from the modifyMat.ipynb Jupyter notebook file or the code in that notebook file can be run seperately in Python without the notebook. If wanted, a python virtual environment can be created that includes numpy which is required for running the python code. The path to the .dat simulation save file will need to be updated in the code. For example, `sim = Simulation('path_to_data/network40p.dat', debug=True)` will need to be updated with path_to_data representing the path to the .dat file. Also, the path to the output file will need to be updated. For example, `sim.save('path_to_data/network40p_ov_five_pct_assem75_divnorm.dat',debug=True)` will need to be updated with path_to_data being the path to the output file.

## Using Normalized Data

The data can be run in the pattern retrieval simulation after the data is normalized. The build folder for the pattern formation simulation should contain the .dat normalized data. The file path to the .dat normalized data should be entered into the main file of the pattern retrieval code. This is entered as what fId is equal to. For example, `fId = fopen("path_to_data/network65p.dat", "rb")`.

## Changing Neuron Degradation Amounts

The line `if (randPyrCell < k + 138)` can have its number changed (k_plus_number) to change what number of neurons have degradation. This number is originally about 50% of a 275 cell assembly. The number of cells in the k_plus_number are the number degraded. For instance, setting it to 28 would cause about 10% degradation in an assembly of size 275.

## Plotting and Analyzing Results

Users can use the software listed in [results recreation](https://hco-dev-docs.readthedocs.io/en/latest/pattern_comp_sep/results_recreation.html) to plot results. Data files generated from new runs of the simulations supply files that can be used in place of the ones listed in prior created files on Zenodo [link](https://www.doi.org/10.5281/zenodo.10870586). Figures and descriptions in (Kopsick et al., 2024) can be used to guide what the meaning of each plot is.

## Changing Network Size or Other Properties

One can change network properties like network size in the file generateCONFIGStateSTP.h. An important note is that any formation project network property changes must be copied to the retrieval project for the retrieval project to run correctly and without errors. The reason for this is that the simulation state is saved during the formation project and imported in the retrieval project. The save state file can be network65p_ov_five_pct_assem75_divnorm.dat or a different file. The norm part of this example filename indicates that the network's saved state has been normalized with the python normalization code before being imported in the retrieval project. An example way to copy settings between the projects is to copy any changed files, e.g., generateCONFIGStateSTP.h, from one project to the other before the retrieval project is run. If the retrieval project tries to import a saved simulation state but does not have the correct network configuration from a file such as generateCONFIGStateSTP.h, then errors can occur with the importing of the data.

## Changing the Number of Assemblies

The trainPat.csv file will need to be updated to include the number of assemblies that are wanted. This can be done, for example, by updating createBetaPatternNoOverlap.m for this. There needs to be the number of assemblies + 1 with matrices created with the lines `randperm(size)`. In the formation project, `numPatterns` should be set to the number of intended assemblies. In the retrieval project, the code section that present the testing pattern neural activations, e.g., with lines such as `sim.setExternalCurrent(CA3_Pyramidal, pc_current);`, needs to run long enough to present all intended pattens. For instance, adding the code:
```
int numPatterns = 6;
int pat_start_i = 200;
int pat_max_i = pat_start_i + ((numPatterns*2)-1);
if (i >= pat_start_i && i < pat_max_i)
{ ... } // present patterns
if (i >= pat_max_i && i < (pat_max_i+1))
{ ... } // non-pattern-presentation activity
```
can ensure the patterns are presented long enough. In plotting, the file `createFigure3PanelD.m` needs to be updated to plot a suffient number of neurons and time to show the intended assemblies. This can be done, for instance, with adding the code:
```
if number_of_patterns == 6
    assemB4 = assemB1+3*nonAssemB1;
    assemB5 = assemB1+4*nonAssemB1;
    assemB6 = assemB1+5*nonAssemB1;
    nonAssemB4 = nonAssemB1*4;
    nonAssemB5 = nonAssemB1*5;
    nonAssemB6 = nonAssemB1*6;
    stimAssemIdx = find((SRPyrRaster(2,:) >= 0 & SRPyrRaster(2,:) < assemB1) | ...
    (SRPyrRaster(2,:) >= nonAssemB1 & SRPyrRaster(2,:) < assemB2) | ...
    (SRPyrRaster(2,:) >= nonAssemB2 & SRPyrRaster(2,:) < assemB3) | ...
    (SRPyrRaster(2,:) >= nonAssemB3 & SRPyrRaster(2,:) < assemB4) | ...
    (SRPyrRaster(2,:) >= nonAssemB4 & SRPyrRaster(2,:) < assemB5) | ...
    (SRPyrRaster(2,:) >= nonAssemB5 & SRPyrRaster(2,:) < assemB6));
    nonStimAssemIdx = find((SRPyrRaster(2,:) >= assemB1 & SRPyrRaster(2,:) < nonAssemB1) | ...
    (SRPyrRaster(2,:) >= assemB2 & SRPyrRaster(2,:) < nonAssemB2) | ...
    (SRPyrRaster(2,:) >= assemB3 & SRPyrRaster(2,:) < nonAssemB3) | ...
    (SRPyrRaster(2,:) >= assemB4 & SRPyrRaster(2,:) < nonAssemB4) | ...
    (SRPyrRaster(2,:) >= assemB5 & SRPyrRaster(2,:) < nonAssemB5) | ...
    (SRPyrRaster(2,:) >= assemB6 & SRPyrRaster(2,:) < nonAssemB6));
    fh = figure; clf; plot(SRPyrRaster(1,stimAssemIdx), SRPyrRaster(2,stimAssemIdx), '.r','MarkerSize', mSize);
    hold on;
    plot(SRPyrRaster(1,nonStimAssemIdx), SRPyrRaster(2,nonStimAssemIdx), '.b','MarkerSize', mSize);
    plot(SRBCRandRaster(1,:), SRBCRandRaster(2,:), '.', 'Color', [46 49 146]./255,'MarkerSize', mSize);

    fh.WindowState = 'maximized';
    xlim([0 1500])
end
```