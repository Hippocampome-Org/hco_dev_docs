Overview of Synaptic Probabilities Software
===========================================

The following instructions describe the general process to rebuild or update synapic probabilities materials.

## Rebuilding or Updating

If a prior version of the Synaptic Probabilities database tables exists, backup that before adding new material to reference 
the original material for comparison. For example, export a hippocampome database with the synaptic probabilities tables. Optionally, 
an extra copy of that exported database can be made with a name such as hippocampome2. This can help with comparing new table content
created by rebuilding the tables with the material in the prior tables.

## Tables Related to Propagate Errors Data

Instructions for building these tables are in the [Propagate Errors Calculations](https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/docs/synaptic_probabilities/propagate_errors.md) page in this documentation.
These tables require data from the tables neurite_quantified, number_of_contacts, and table based on the SynproParcelVolumes.csv file in the prop_error directory. The csv files created are for the tables SynproCP, SynproCPTotal, SynproNOC, SynproNOCTotal, SynproNoPS, and SynproNoPSTotal.

## Other Tables

Instructions for building all other synaptic probabilities related tables are in the [Synaptic Probabilities](https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/docs/synaptic_probabilities/fragment.md) page in this documentation.
