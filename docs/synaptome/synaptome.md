Synaptome
=========

The synaptic physiology page on the Hippocampome site includes values that have been created for parameters to use in synapse modeling.

## Building the Database

Due to the quantity of data in the Synaptome project an additional database was created for it. Instructions for building the database are in the Github repo for the project [link](https://github.com/Hippocampome-Org/synaptome_db).

## CSV Downloads

CSV files were provided from the author of the synapse model and are availible for download. On the Hippocampome server these are stored at \<server_address\>/general/synapse_modeling/Final_Synaptic_Data/.

## Json Files

Code for generating Json files is in /synap_model/gen_json/generate_json.php. The json files store the database values for fast access with avoiding having users need to spend the computational time quering the database. The Json results files are stored in /synap_model/gen_json/json_files. These are the files that are displayed on the hippocampome site pages.

To generate the json files, navigate to the generate_json.php page and click the button "Synaptome Model Values All Conditions".

## Summary of Modeling Values

Clicking on an entry in the main matrix will bring one to the summary of modeling values page. The code for the page is at synaptic_mod_sum.php. This data is retreived from database queries executed in the php code.