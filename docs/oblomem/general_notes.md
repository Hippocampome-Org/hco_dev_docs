General Notes
=============

This documentation contains general notes for working with Oblomem.

## Normalization

Some experiments contain normalization weight scaling through the line `weight_scale = (1/weight_mean)*p->normalization_weight;` in general_functs.cpp. This was found to not correctly include the intended formula. The corrected formula is `weight_scale = 1/(weight_mean*p->normalization_weight);`. The corrected formula should be used for simulations. Older Github commits may not contain this corrected formula. The results of simulations with the corrected formula were found to be similar, perhaps even close to the same, as with the uncorrected formula, but the correct formula should be used.