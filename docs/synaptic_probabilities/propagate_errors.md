Propigate Errors Calculations
=============================

Strategy: the calculations create several temporary SQL tables (views) that are
used to store intermediary values. The only perminant table added is
SynproParcelVolumes. The temporary tables will be removed upon starting a new
MySQL session, and can be automatically added again with the build_error_prop.sh
script as described below. This help avoid taking up database space with intermediary
tables when that is not needed.

## Step 1

Define the parcel-independent constants to be used throughout the NxN calculations.
<br>a. Inter-bouton distance: length_bouton = 6.2.
<br>b. Dendritic spine distance: length_spine = 1.09.
<br>c. Radius of interaction: radius_int = 2.
<br>d. Volume of interaction: volume_int = (4/3)*pi*radius_int^3.
<br>e. Constant: c = volume_int / (length_bouton * length_spine).

Implementation:
<br>Run constants.sql

## Step 2
Determine the total number of parcels (n_parcels) involved in the interaction between the "From" and "To" neuron types in the relevant subregion.

Implementation:
<br>Run number_of_parcels.sql

3) Select the first parcel for performing the NxN calculations.
<br>a. Query the database for the needed values.
<br>&nbsp;&nbsp;i. You need to read from the database all of the axonal length values for the "From" neuron type. This information is already being read from the database for the Number of Potential Synapses Evidence Page and the Number of Contacts Evidence Page. Find the mean value for all of these axonal lengths (axonal_length_mean_[parcel]) and calculate the standard deviation (axonal_length_stdev_[parcel]).
<br>&nbsp;&nbsp;ii. You need to read from the database all of the dendritic length values for the "To" neuron type. This information is already being read from the database for the Number of Potential Synapses Evidence Page and the Number of Contacts Evidence Page. Find the mean value for all of these axonal lengths (dendritic_length_mean_[parcel]) and calculate the standard deviation (dendritic_length_stdev_[parcel]).
<br>&nbsp;&nbsp;iii. You need to read from the database all of the axonal convex hull volume values for the "From" neuron type. This information is already being read from the database for the Number of Contacts Evidence Page. Find the mean value for all of these axonal convex hull volumes (axonal_volume_mean_[parcel]) and calculate the standard deviation (axonal_volume_stdev_[parcel]).
<br>&nbsp;&nbsp;iv. You need to read from the database all of the axonal convex hull volume values for the "To" neuron type. This information is already being read from the database for the Number of Contacts Evidence Page. Find the mean value for all of these dendritic convex hull volumes (dendritic_volume_mean_[parcel]) and calculate the standard deviation (dendritic_volume_stdev_[parcel]).
<br>b. Read in needed values from stored data/ files
    i. You need to read the parcel volume (volume_[parcel]) from the first line of either data/[parcel]-Table-1.csv or data/[parcel]-Table-2.csv.
c. Calculate the mean potential volume of overlap and its standard deviation.
    i. Calculate the overlap volume mean: overlap_volume_mean_[parcel] = (axonal_volume_mean_[parcel] + dendritic_volume_mean_[parcel]) / 4.
    ii. Calculate the overlap volume standard deviation: overlap_volume_stdev_[parcel] = sqrt(axonal_volume_mean_[parcel]^2 + dendritic_volume_mean_[parcel]^2) / 4.
d. Calculate the mean number of potential synapses (NPS) and its standard deviation.
    i. Calculate the NPS mean: NPS_mean_[parcel] = c * axonal_length_mean_[parcel] * dendritic_length_mean_[parcel] / volume_[parcel].
    ii. Calculate the NPS standard deviation: NPS_stdev_[parcel] = NPS_mean_[parcel] * sqrt((axonal_length_stdev_[parcel] / axonal_length_mean_[parcel])^2 + (dendritic_length_stdev_[parcel] / dendritic_length_mean_[parcel])^2) / volume_[parcel].
e. Calculate the mean number of contacts (NC) and its standard deviation.
    i. Calculate the NC mean: NC_mean_[parcel] = (1/n_parcels) + (c * axonal_length_mean_[parcel] * dendritic_length_mean_[parcel]) / overlap_volume_mean_[parcel].
    ii. Calculate the NC standard deviation: NC_stdev_[parcel] = NC_mean_[parcel] * sqrt((axonal_length_stdev_[parcel] / axonal_length_mean_[parcel])^2 + (dendritic_length_stdev_[parcel] / dendritic_length_mean_[parcel])^2 + (overlap_volume_stdev_[parcel] / overlap_volume_mean_[parcel])^2).
f. Calculate the mean connection probability (CP) and its standard deviation.
    i. Calculate the CP mean: CP_mean_[parcel] = NPS_mean_[parcel] / NC_mean_[parcel].
    ii. Calculate the CP standard deviation: CP_stdev_[parcel] = CP_mean_[parcel] * sqrt((NPS_stdev_[parcel] / NPS_mean_[parcel])^2 + (NC_stdev_[parcel] / NC_mean_[parcel])^2).