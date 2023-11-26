Synaptic Probabilities Tool
===========================

## Keeing tool up-to-date
If any changes are made to the number_of_contacts table, build_error_prop.sh should be run to regenerate SynproNOCR.csv. The database should then be rebuild with the new SynproNOCR.csv. The SynproNOCR.csv file is a reformatted number_of_contacts file that provides one row for each subregion and layer of a neuron pair. This is helpful for retrieving which subregions and layers are relevant to each neuron pair for use in this tool.

## Purpose
The tool allows for setting custom values for synaptic probabilities calculations.

## Code
The code for the tool is in the connprob.php file.

## Results generation
The results are live generated through use of the neurite_quantified and NOCR tables.