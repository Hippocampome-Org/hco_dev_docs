Synaptic Probabilities CSV Downloads
====================================

## Downloadable files
CSV downloadable files are provided for each matrix. This helps accomidate analysis and programming with the values from the matrices.

## Creating the downloadable files
====================================
Instructions for creating the downloadable files are in the /synap_prob/gen_csv_dls/ directory in the Readme.txt file. A copy of that file as of 05/30/23 is:

## In MySQL Workbench
1. open then run:
<br>netlist_parcels_create_table.sql
<br>2. run with "run sql script":
<br>netlist_parcels.sql
<br>Alternatively run in a command prompt:
<br>`mysql -h localhost -u <username> -p <database> < netlist_parcels.sql`

## For ADL and SD
3. open and run:
<br>adl_stats.sql
<br>4. open and run:
<br>adl_values.sql
<br>5. use export button in query results window to export
<br>adl_values.csv

## NPS, NOC, and SP, the \_values.sql for them can be directly run and results from that are exported to csv files.
These files create database views.
<br>The views can be exported into csv files with export_table.sh.
<br>Run in command prompt:
<br>`./export_table.sh <username> <password> <database> <view_name>`
<br>This will export the data to /var/tmp/SynproExports/
====================================

## Storing the files
The downloadable csv files are stored on the hippocampome server in the folder /synap_prob/data/. This folder does not need to be present in a local installation. The purpose of the files are to be in a place where they can be downloaded so only having them present on the server can be suitable. The github repo for hippocampome does not have the data directory because it is expected to only exist on the server instead of needing to be on a local install.