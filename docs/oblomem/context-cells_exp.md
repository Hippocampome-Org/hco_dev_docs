Experiment Components in Context-cells Simulations
==================================================

This documentation provides further information about experiment components originally designed for context-cells experiments. These experiments were designed to test a neuromodulation theory by Dr. Hasselmo.

## Lateral Inhibition

See [lateral inhibition documentation](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/lateral_inhibition.html) on methods used in implementing lateral inhibition.

## Neuromodulation Affecting STP

TODO: add description here.

## Neuromodulation Acting as Inhibitory Neurons

Neuromodulation acting as inhibitory neurons can be enabled by the parameter `enable_ach_inhib` set to `1`. Setting that to `0` disables the parameter. Current can then be supplied to the interneuron group representing neuromodulation by using the `setExternalCurrent()` function. For instance, `sim.setExternalCurrent(ACh_CA3_Basket, 1000.0f);`.



