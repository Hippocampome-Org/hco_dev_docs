Izhikevich model parameter exploration testing firing patterns
==============================================================

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

## Preparing the json file

The json file will have its parameters automatially changed by the parameter exploration script and because of that it needs a few types of reformatting to accomidate the software. The original json file can be backed up as file_name_original.json and can be used as a reference if wanted. The original file can provide guidance on what parameter ranges are relevant to biology that can be explored. The EA software was originally allowed to go somewhat beyond the ranges listed in the json files, based on the EA mutation operator, so there is flexibility in what parameters are suggested to use relative to those ranges. Hippocampome.org's Izhikevich model page can also provide guidance on what parameters were found to be acceptable given a neuron type.

The values for each Izhikevich parameter need to be stored on one line per parameter. For example:
<br>	"parameter_ranges" : {
<br>			"K" : \[0.62, 0.62\],
<br>			"A" : \[0.005, 0.005\],
<br>			"B" : \[11.69, 11.69\],
<br>			...
<br>		}
<br>
This allows the parameter exploration script to correctly detect the link with the parameter of interest to change the values. The values should all have values in the whole number and fraction places, e.g., 1.0 not 1 or .1. This is for pattern matching. It is recommended to create a file with the standard version of the parameters before they are altered by the exploration script. This will allow for easily copying over the standard version to the json file used for testing to "reset" the values back to the standard levels. This could be stored as file_name_custom.json. The json file used for testing will normally have the neuron_type-subtype.json name, and the primary_input file will be set to use this file to run firing pattern tests on it

## Conversions

An important note is that the parameters VMIN, VT, and VPEAK have values in the json file that are relative to the VR parameter. For example, if VR decreases by 1, those three other parameters should accordingly decrease by 1 to retain their original parameter values. On Hippocampome.org, for neuron id 6003, subtype 1 lists VT as -43.52 and VR as -58.53. Because of this, in the EA software to enter those same values, VR should be listed as -58.53 and VT as 15.01. This is because VT relative to VR is 15.01 greater. This will have the EA software test the parameters with VR=-58.53 and VT=-43.52.

## Setting parameter value ranges

A method that can help to set parameter ranges is using matlab's linspace() function. E.g., linspace(starting_value,ending_value,total_values). Probably similar functions exist in other languages. Values listed as the upper and lower range bounds for each parameter in the original json files can be starting points as starting and ending values to test but additional ranges can be tested beyond that.

The number of values intended to test for each parameter must be specified in auto_mod_fp_iz.sh lines:
<br>for i in {0..8} 
<br>for j in {0..8} 
<br>
<br>where 0..8 means to cycle through 9 values per parameter. i is the value index for parameter 1 and j is that for parameter 2. Setting a different number of values to be tested must include updating these lines to match the number of values to be tested.

## Run auto_mod_fp_iz.sh

Once the settings have been entered, a user should run auto_mod_fp_iz.sh to generate tests of how each parameter value scores with the reference firing pattern. The scores are in the form of "fitness scores". As described in the spatial navigation simulation article, the fitness score is the sum of the combined error scores representing difference in model firing from real firings in a firing pattern. The errors are also weighted in a process described in ([Venkadesh et al., 2019](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007462))'s original work. The results and parameter values appear in the file /scripts/param_explore/output/param_records_ea_iz.txt. If a user has a text editor, e.g., Sublime Text, that can show document updates in real time, then the user can open the file in the editor and watch the scores reported in realtime to ensure the correct parameters are being tested.

An additional note is that the file auto_mod_fp_iz_vr.sh is a specialized script which tests changing more than 2 parameters at a time.

## Plotting

See the plotting documentation for instructions on plotting results.

## Optional parameters

The parameters for a neuron type listed on Hippocampome.org are the top ranked parameters found to fit a firing pattern. There are also lower ranked parameter that can be explored if wanted as well. Those are listed in rank order in "ensemble" files in the downloads section of the Izhikevich Model section of neuron pages.