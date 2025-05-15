Lateral Inhibition
==================

This documentation provides development documentation about an optional component of the Oblomem simulation that is lateral inhibition.

## Design

A script was created for computing connectivity in lateral inhibition, named oblomem_connections.m. Currently, this script connects each place cell ensemble's neurons to interneurons. Those interneurons then connect to all other place cell neurons. This is lateral inhibition through use of interneurons as an intermedary resource providing inhibition.

The connectivity in the script could be altered if wanted. Given 4 place cell ensembles, and 150 neurons per ensemble, each interneuron connecting to place cells has 1 to 450 connections. Research could be done into what Hippocampome.org reports for interneuron connectivity maximums based on neuron types to ensure this is within reported maximum probability of connection values.

The variables max_lat_inh_pre_id and max_lat_inh_post_id in general_params.cpp specify the neuron id range maximum for the custom connection function in CARLsim to use to build connectivity. This is based on the maximum neuron id within the ensembles worked with in Oblomem currently being 1200 for both pre- and post-synaptic neurons. One should change these variable values if more lateral inhibition is wanted. Defining these maximums saves computational time in each simulation run by not needing to check N by N possible connections for defining active lateral inhibition connections when N can be per se 90,000 in the full scale version of the Oblomem simulation.

## Custom Connections

Dr. Hasselmo's theory indicates the possible presence of lateral inhibition in his theory. This is in descriptions such as "...activity of the word holster inhibits activity of the word boot..." (Hasselmo, 2013; Figure 6.6). Also, arrows pointing from "holster" to "boot" in panel A of Figure 6.6. See descriptions of "lateral inhibition" in the work_report.docx document for more information.

The parameter `enable_lateral_inhib` set to `1` in general_params.cpp enables lateral inhibition. Note, there is no `=` in this parameter value assignment. Setting the parameter to `0` prevents this type of lateral inhibition.

The `LatInhibConnections()` function in general_functs.cpp is used to configure lateral inhibition connections. This function inherits properties from the ConnectionGenerator class to create a custom connection function within CARLsim. The `ParseCSV()` function in the main file is used to import the `lat_inh_conns.csv` file produced by `oblomem_connections.m`. That csv file populates the `lat_inh_conns` 2D array. `LatInhibConnections()` references the lat_inh_conns array to specify which neurons should receive lateral inhibition connections. The synapse weight is set in the line `weight = ...`.

## References

Hasselmo, M. E. (2013). How we remember: Brain mechanisms of episodic memory. MIT press.