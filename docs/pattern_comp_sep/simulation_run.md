numPyramidalFire sets the target assembly size. It needs to be updated in each line it exists in the main C++ file, e.g., main_ca3_lognormalI_09_05_23_assem_275.cpp, if one wants to change the assembly size.

createBetaPatternOverlap.m or createBetaPatternNoOverlap.m from the [analysis code](https://github.com/jkopsick/cell_assembly_formation_retrieval/tree/main/matlab_analysis_visualization_code) can create a csv file that is used for training the simulation.

The number after the word "network" in the .dat filename specifies the number of times a pattern is presented to cell assemblies. For example, network95p.dat describes the file containing the simulation state after 95 pattern presentations.

Normalizing data structures:
