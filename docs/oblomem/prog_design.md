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

## Altering Functionality

One can deactivate or comment out code sections to prevent that part of the experiment from occuring. For example, commenting out the line `normalize_weights...` in the main file can typically disable normalization from occuring. This is in the case that other lines are not creating normalization as well. In a main file, there can be per se two lines with `normalize_weights` and one line with `sim.scaleWeights`. Typically only one of those lines will be uncommented. Each of those lines can create normalization if uncommented, and are designed to test normalization in different ways. Commented out lines are often only uncommented for special purposes, and can typically be ignored, unless they provide documentation about the content of the software. By documentation I meant that they describe the purpose of code below them.

Functionality is possibly implemented in ways that is not as simple as just searching for the letters "normaliz" in the main file to find any examples of normalization. For instance, any simulation that has `sim.scaleWeights` uncommented in the main file is also creating normalization. One can see that the normalize_weights function also contains the `sim->scaleWeights` operation. `scaleWeights` in the main file takes an operation such as in the normalize_weights function and includes it without the function. One should look for such code if one wants to make sure to deactivate such functionality in the simulation.

## Differences Between Project Versions

One can read about the differences in code between project versions using a document difference software tool. For example, comparing the difference between context cell and non-context cell projects, or differences between fullscale and non-fullscale versions. Nate recommends the software named Meld ([link](https://gnome.pages.gitlab.gnome.org/meld/)) for use in Windows or Linux operating systems (it is open source). This software can compare source code between files and highlight the differences for inspection. Other document difference software exists that can be used as another option for comparing files. One can choose to use Meld or any different software for this purpose. 

This software can also be useful for tracing where errors introduced in code may have come from. For example, one can save a working version of a simulation to Github. If an issue comes up, then one can compare the code with an issue to the one that is not working correctly to help trace where the issue may have originated from.