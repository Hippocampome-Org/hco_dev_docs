Parameter exploration with firing pattern approximations and Izhikevich model values
====================================================================================

This documentation is for running parameter exploration with Izhikevich model parameters and testing similarity to firing patterns as reported in published articles. The step of the Izhikevich model parameter exploration focused on here is fitting parameter values to results that are similar to published firing patterns.

## Evolutionary algorithm software

The work of ([Venkadesh et al., 2019](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007462)) uses an evolutionary algorithm (EA) to find Izhikevich model parameters that fit firing patterns found in published articles for specific neuron types. This spatial navigation work uses the software from that work to assess fits to firing patterns of different parameter values. The evolutionary process in the algorithm is not used in this work, only the scoring of firing patterns with parameters function.

A fork of the EA software has been created to assist with this work and it is availible at [this Github repo](https://github.com/nmsutton/Time). Users should download the code from that repo to use it for parameter exploration. That code runs with Java so Java must be installed for the user. Some more details about requirement for the software are listed in the Github readme file for the fork.

## Parameter exploration script

The main script for running parameter exploration of the firing patterns for this spatial navigation work is auto_mod_fp_iz.sh. The parameters for that script are in params_config_fp_iz.sh. Users should edit the parameters of interest in params_config_fp_iz.sh. p1_v and p2_v are the parameter values to test for parameter 1 and 2. p1_p and p2_p are the names of the parameters as listed in the json file under "parameter_ranges". The json file to use is one that matches the neuron number of interest.

### Finding the correct json file

The neuron type Izhikevich parameters were explored with in the article was medial entorhinal cortex layer II stellate cell. This has neuron id 6003 on Hippocampome.org ([link](https://hippocampome.org/php/neuron_page.php?id=6003)). In ([Venkadesh et al., 2019](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007462))'s research some Izhikevich parameters were reused between neurons. There is no json file for neuron id 6003 but there is one for neuron id 6019. It can be observed that the json file for neuron id 6019 is the one to use to test neuron id 6003 because on Hippocampome.org's Izhikevich model page ([link](https://hippocampome.org/php/Izhikevich_model.php)) the Izhikevich parameters for 6003 and 6019 are the same. This indicates the same parameters were reused for 6003 that were used in 6019, and that if a json file exists (which is the case) for 6019, that should be used for testing 6003.

### Selecting a subtype

Json files are labeled in the way of neuron_type-subtype.json in their names. Users should be aware that selecting a json file involves choosing what subtype is wanted. Hippocampome's neuron pages, e.g., [neuron 6019](https://hippocampome.org/php/neuron_page.php?id=6003), list the different subtypes under the "Izhikevich Model" section.

### General settings

Users should set the file "primary_input" in the base EA software folder to the json file intended to be tested. This is set in the first line with folder_1,folder_2,json_filename,1,0. E.g., 11,3a,6-019-1,1,0. This assumes testing of a single compartment version of the neuron type, and if a multicompartment neuron type is meant to be tested then different settings should be used.

izhikevich_base.params can set the number of generations (number of runs) to use for finding a results score. The article's work used generations = 3 because parameters were not meant to be changed by the EA but instead were changed by a seperate script. Because of that, multiple generations were not expected to create different results but 3 were used just for comparison and any initialization of the software purposes.

