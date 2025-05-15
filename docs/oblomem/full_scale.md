Full Scale Experiment
=====================

This documentation provides further information about working with the full scale versions of experiments.

## Overall

In general, the full scale project is similar to the small scale project. However, there are some particular differences.

## Differences

#### Lognormal Distributions

The full scale project includes 8 neuron types in contrast to the small scale experiment often only containing 2 neuron types. Each of the 8 neuron types has its own lognormal distribution. This is present in the main file.

#### Group Numbers

The full scale experiment contains additional code in the main file to track the group number of neuron type groups. This is in lines such as `p.CA3_QuadD_LM = CA3_QuadD_LM;`. This becomes useful in functions that want to apply operations to neuron type groups and needs to have the group ids provided for it.

#### Additional Objects Passed to Functions

Functions such as `non_pattern_pres_run` will have several more current vectors and lognormal distribution generator objects passed to it. This is due to the additional neuron types' resources needing to be accounted for in the function's processing. Some of these objects and vectors are stored in the struct named `p` which generally is named for "parameters".

#### Settings for 8 Neuron Types' Properties

The file `generateCONFIGStateSTP.h` there are many settings for neuron type properties for the additional neuron types as compared to the small scale version with 2 neuron types. Neuron properties for the 8 neuron type groups are based on those found in (Kopsick et al., 2024).

#### Scaling Amount

In the file `generateCONFIGStateSTP.h`, the variable `scaling_amount` helps control the scaling of the small- or full-scale sized properties. Setting the `scaling_amount` at `1.0` causes the neuron counts per neuron type group to match the full scale numbers used in (Kopsick et al., 2024). The small scale experiment typically uses 4% of the neuron counts found in the full scale version. This is set by assigning `scaling_amount = 0.04`.

#### Connection Selected for Normalization

Many more synaptic connections exist between neuron type groups given that there are 8 neuron types in this experiment. The normalization code uses a selected connection to perform normalization. That connection should be CA3_Pyramidal -> CA3_Pyramidal. The parameter `norm_weights_connId` in `general_params.cpp` sets which connection number should receive normalization. In the full scale experiment, this connection will typically be number 50. In the small scale experiment, this will typically be connection number 3. Setting this as the wrong connection number will normalize the wrong synapse weights.

#### Computer Resources Needed to Run Full Scale Simulation

Different settings in the slurm file, compared to the small scale version, are needed to run the simulation in full scale.

## References

Kopsick, J. D., Kilgore, J. A., Adam, G. C., & Ascoli, G. A. (2024). Formation and retrieval of cell assemblies in a biologically realistic spiking neural network model of area CA3 in the mouse hippocampus. Journal of Computational Neuroscience, 52(4), 303-321.