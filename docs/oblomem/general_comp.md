General Experiment Components
=============================

This documentation contains descriptions about experiment components in Oblomem.

## Acetylcholine (ACH) Levels as Current

The variable `ach_calcium2current` in general_params.cpp enables using ACh levels as current input to CA3 Pyramidal cells, such as cells that are included in concept cell ensembles. This current can replace some of the log-normally distributed background activity current in the simulation. The purpose of this simulation component is to create a simple way to include ACh neuromodulation of neural activities.

The vector `ach_level` supplies ACh level measurements that manage the amount of current supplied by this method. `ach_level` represents levels of activity captured in animal recordings. The parameters `calcium2current` and `calcium_min` help control the amount of current supplied.