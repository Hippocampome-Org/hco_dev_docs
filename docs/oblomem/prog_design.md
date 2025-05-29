General Programming Design
==========================

This documentation will describe the general design of the software. This can help with working with it or further developing it.

## Project Files

#### main_oblomem_xyz.cpp (xyz is project-specific name)
This is the main simulation file. This file manages the overall operation of the simulation.

#### general_functs.cpp
This is a file that contains functions used within the simulation

#### general_params.cpp
This file contains much of the parameter values used in the simulation. Setting values in this file can alter the way the simulation's results turn out.

#### generateCONFIGStateSTP.h
This file contains mamy of the neuron property values used in the simulation. It is content run specifically at the Config section of the simulation.

#### generateSETUPStateSTP.h
This file contains some monitor objects and is run in the Setup section of the simulation.

## Design Methods

The code is meant to be read in a way that comments describe code below them. Also, function names describe the purpose of the function. One can read the way the simulation operates by starting with the main file and reading what each section does by it's comments, function names, and variable names.

The design is intended to contain much of the control over the simulation in the general_params.cpp file. This can be thought of generally as a configuration file for the simulation. May parameters have comments with them in general_params.cpp that explain their purpose. Sometimes, one should look past multiple commented out values to read the comment about what the purpose of the parameter is. One can search for where each parameter in the file is used in the simulation to read about what it does as well.