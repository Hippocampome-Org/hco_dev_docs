Project settings examples
=========================

These instructions provide directions to create example projects of different grid scales and average firing rates. These were settings used to create results in the article.

## How to use the settings

The example settings are found in /scripts/settings_archive/. The files in the example project directory should be copied into the base folder of the project.

## Selecting an example project

The example files are in folder with names including the abbreviations:
<br>sml, med, lrg scale which stand for small, medium, and large grid scale.
<br>low, med, high fr which stand for low, medium, and large average grid cell firing rates.
<br>A user can select which type of settings are wanted by using the files in the relevant folder.

## Building required files

The files synapse_weights.csv and grc_to_in_conns.csv will need to be rebuilt. Instructions to building them are in the "building required files" documentation. If animal data files are needed, those should be created using the "converting animal recordings" documentation.

## Renaming main file

The main cpp file in the src directory needs to be renamed to main_proj_name.cpp where proj_name is the name a user has selected for their CARLsim project name.

## Animal data

Any real animal data a user wants to include should be added with instructions in the "converting animal recordings" documentation.