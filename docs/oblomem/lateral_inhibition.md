Lateral Inhibition
==================

This documentation provides development documentation about an optional component of the Oblomem simulation that is lateral inhibition.

## Design

A script was created for computing connectivity in lateral inhibition, named oblomem_connections.m. Currently, this script connects each place cell ensemble's neurons to interneurons. Those interneurons then connect to all other place cell neurons. This is lateral inhibition through use of interneurons as an intermedary resource providing inhibition.

The connectivity in the script could be altered if wanted. Given 4 place cell ensembles, and 150 neurons per ensemble, each interneuron connecting to place cells has 1 to 450 connections. Research could be done into what Hippocampome.org reports for interneuron connectivity maximums based on neuron types to ensure this is within reported maximum probability of connection values.

The variables max_lat_inh_pre_id and max_lat_inh_post_id in general_params.cpp specify the neuron id range maximum for the custom connection function in CARLsim to use to build connectivity. This is based on the maximum neuron id within the ensembles worked with in Oblomem currently being 1200 for both pre- and post-synaptic neurons. One should change these variable values if more lateral inhibition is wanted. Defining these maximums saves computational time in each simulation run by not needing to check N by N possible connections for defining active lateral inhibition connections when N can be per se 90,000 in the full scale version of the Oblomem simulation.