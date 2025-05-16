Experiment Components in Non-context-cells Simulations
======================================================

This documentation provides further information about experiment components originally designed for non-context-cells experiments. These experiments were designed to test theories on neuromodulation of STDP long-term plasticity.

## Overview

Some code was specifically included to test elements of the STDP long-term plasticity neuromodulation theory which may not be present in the context-cells version of the experiments. In the future, these elements may be merged together in the experiment versions, if that is something that becomes useful. The context-cells versions of experiments refers to simulations originally designed to experiments designed to test neuromodulation of presynaptic inhibition as described by a theory by Dr. Micheal Hasselmo.

## Patterns Presented in Testing

Setting `numPatterns = 6` in general_params.cpp of the retrieval (test) experiment can help avoid testing extra patterns for context cell ensembles. `npf_start` can also be used to help specify which ensemble patterns should be presented.

## Disable Context Cells

Some versions of experiments have `enable_context_cells` as an option in general_params.cpp. This option should be set to `false` in experiments without context cells. This will disable the use of context cell ensemble pattern presentation.