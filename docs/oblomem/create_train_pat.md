Training Pattern Files Creation
===============================

This documentation provides further information how to create files used for training patterns.

## Purpose of files

Files such as trainPat.csv are created to specify a sequence of neurons that should receive current at certain times during pattern presentation. That certain time period is 20 milliseconds (ms) and is meant to represent one gamma cycle. For instance, one can specify that 150 neurons (ensemble size) should receive current that make 8 spikes per neuron during the 20 ms. This script randomizes the sequence that each neuron has spikes occur during the 20 ms. An advantage to having a separate file that stores this information is that this sequence of spikes can be replayed in a the same random order across multiple runs of experiments. The random order does not change, and therefore analyzing changing in simulation while controling for that randomization is accomidated.

The number of spikes per neuron per pattern will effect training speed using the patterns. Animal recordings can cause some patterns to be presented more often then other patterns. For instance, place cell ensembles compared to other types of cell ensembles (object or context cells). Setting these number of spikes can help adapt training to balance conditions where some patterns are made to occur more than others. Currently, most "formation" version of simulation projects are configured to accept different spikes per neuron for place cells vs. other cell types, and that will be explained more later.

## File creation

The script used to create trainPat.csv has the file path `/general_tools/create_train_pat/create_train_pat.m`.
This `create_train_pat.m` script should be run from Matlab by specifying an ensemble size, spikes_per_neuron, and output_file_name. For example:
<br>`create_train_pat(150,600,'trainPat.csv')`
<br>will create the file trainPat.csv, which specified an order of spike times to provide in a pattern.

The Parameters are:
<table>
	<tr><td>Parameter Name</td><td>Description</td></tr>
	<tr><td>ensemble_size</td><td>Default value: 150. Size of ensemble the pattern will be presented to</td></tr>
	<tr><td>spikes_per_neuron</td><td>Default value: 4. Number of spikes per neuron per pattern presentation</td></tr>
	<tr><td>output_file_name</td><td>Default value: trainPat.csv. File name of output file</td></tr>
</table>

## Customization

One can change the ensemble size parameter if different sized ensembles are used in the simulation. Changing this from the default value of 150 is not currently needed in the research project. Both the spikes_per_neuron and output_file_name can be different for patterns presented to place cell ensembles compared to other ensembles.

### Different Patterns for Place Cells

Results currently have found that place cells with 8 spikes per neuron and other cells with 4 spikes per neuron in patterns help balance the training in experiments. Typically the "formation" version of experiments will look for `trainPat.csv` and `trainPatPlaceCells.csv` files to inform what number of spikes per neuron should occur in patterns. When selection the `output_file_name`, keep in mind that these are the filenames used by default. `trainPat.csv` is set by default to have 4 `spikes_per_neuron`. `trainPatPlaceCells.csv` is set by default to have 8 `spikes_per_neuron`.
