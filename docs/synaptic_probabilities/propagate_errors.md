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

