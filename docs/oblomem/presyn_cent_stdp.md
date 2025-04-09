Presynaptic Centered STDP Development Documentation
===================================================

This documentation provides development documentation about a feature providing presynaptic-centered STDP that was created for the Oblomem neuromodulation research.

## Concepts
"It is important to note that CARLsim uses nearest neighbor STDP (Morrison et al. 2008)..." (Beyeler, 2023).

Fig. 7 in (Morrison et al. 2008) shows the difference between symmetric-nearest-neighbor STDP (panel A) and presynaptic-centered-nearest-neighbor STDP (panel B).

The CARLsim user guide's description "...each presynaptic spike is paired with the last postsynaptic spike and each postsynaptic spike is paired with the last presynaptic spike to calculate the final weight change due to STDP." (Beyeler, 2023) indicates that symmetric-nearest-neighbor STDP is included in the official branch of CARLsim. This is due to that quoted description matching very closely to the description in Fig. 7A's caption of (Morrison et al. 2008) that describes symmetric-nearest-neighbor STDP.

Symmetric-nearest-neighbor STDP is not to be confused with symmetric and asymmetric STDP in general. Symmetric in this case specifically refers to the spikes relative to each other in the nearest-neighbor system. Symmetric and asymmetric STDP in general refers to pre before post and post before pre each having their own effects on plasticity.

## Usage
<br>TODO: I will work on creating more of this documentation soon.

## References

Beyeler, M. (2023) Chapter 5: Synaptic Plasticity. https://uci-carl.github.io/CARLsim6/ch5_synaptic_plasticity.html

Morrison, A., Diesmann, M., & Gerstner, W. (2008). Phenomenological models of synaptic plasticity based on spike timing. Biological cybernetics, 98, 459-478.