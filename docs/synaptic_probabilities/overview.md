Overview of Synaptic Probabilities Software
===========================================

The following instructions describe the general process to rebuild or update synapic probabilities materials.

## Rebuilding or Updating

If a prior version of the Synaptic Probabilities database tables exists, backup that before adding new material to reference the original material for comparison. For example, export a hippocampome database with the synaptic probabilities tables. Optionally, an extra copy of that exported database can be made with a name such as hippocampome2. This can help with comparing new table content created by rebuilding the tables with the material in the prior tables.

## Data in the Project

In general, the data exists as values in the database, matrix values in json, evidence records, csv downloadable files, and a synaptic probabilities tool on the site.

## Synaptic Probabilities Statistical Values

Instructions for building statistical values in the database are in [Synaptic Probabilities Calculations](https://hco-dev-docs.readthedocs.io/en/latest/synaptic_probabilities/propagate_errors.html) page in this documentation.
These tables require data from the tables neurite_quantified, number_of_contacts, and table based on the SynproParcelVolumes.csv file in the prop_error directory. The csv files created are for the tables SynproCP, SynproCPTotal, SynproNOC, SynproNOCTotal, SynproNoPS, and SynproNoPSTotal.

## Evidence Tables

Instructions for building evidence record related tables are in the [Synaptic Probabilities Evidence](https://hco-dev-docs.readthedocs.io/en/latest/synaptic_probabilities/fragment.html) page in this documentation.

## Other Data

The additional pages [Json Files](https://hco-dev-docs.readthedocs.io/en/latest/synaptic_probabilities/json_files.html), [CSV Downloads](https://hco-dev-docs.readthedocs.io/en/latest/synaptic_probabilities/csv_dls.html), [Probabilities Tool](https://hco-dev-docs.readthedocs.io/en/latest/synaptic_probabilities/probabilities_tool.html), and [Matrix Header Height](https://hco-dev-docs.readthedocs.io/en/latest/synaptic_probabilities/header_height.html) describe additional resources.