updateNM4Levels Development Documentation
=========================================

This documentation provides development documentation about the feature updateNM4Levels 
that was created for the Oblomem neuromodulation research. This feature allows neuromodulation 
levels to be altered at runtime during a CARLsim experiment. Originally, neuromodulation 
variable could only be altered once per experiment and were set in the configuration state of 
an experiment.

## Usage
### updateNM4Levels()
updateNM4Levels(int netID, int groupID, bool updateDA, bool update5HT, bool updateACh, bool updateNE, float levelDA, float level5HT, float levelACh, float levelNE);

Allowed CARLsim states:
<br>RUN_STATE

Parameters
<table border=1>
<tr><td>parameter</td><td>description</td></tr>
<tr><td>netID</td><td>the ID of a network. If only 1 network is in the simulation then this should be set to 0.</td></tr>
<tr><td>groupID</td><td>the ID of a group.</td></tr>
<tr><td>updateDA</td><td>set if Dopamine neuromodulation should be updated</td></tr>
<tr><td>update5HT</td><td>set if Serotonin neuromodulation should be updated</td></tr>
<tr><td>updateACh</td><td>set if Acetylcholine neuromodulation should be updated</td></tr>
<tr><td>updateNE</td><td>set if Noradrenaline neuromodulation should be updated</td></tr>
<tr><td>levelDA</td><td>set the new level of Dopamine neuromodulation</td></tr>
<tr><td>level5HT</td><td>set the new level of Serotonin neuromodulation</td></tr>
<tr><td>levelACh</td><td>set the new level of Acetylcholine neuromodulation</td></tr>
<tr><td>levelNE</td><td>set the new level of Noradrenaline neuromodulation</td></tr>
</table>	

This function updates the levels originally set by the setNeuromodulator() function. Specifically, this updates the "base" values, e.g., baseACh. This function requires that values be set in setNeuromodulator() for any neuron group that updateNM4Levels() will update the values of.

groupID appears to be based on the order in which neuron groups have their Start Id as reported in carlsim.log of a simulation. Notably, this is not the order specified by the standard neuron group ID that is stored in the neuron group name. For instance, this groupID may be different in number than that specified by a neuron group name such as MEC_LII_Stellate. Also, the order of IDs in groupID does not depend on which groups have neuromodulation. For example, there is only one neuron group (GroupA) with neuromodulation and that group's Start Id is higher than two other groups then GroupA's groupID is 2.

It is recommended that updateNM4Levels() be used in combination with the setWeightAndWeightChangeUpdate() function. The reason for this is that by default CARLsim only checks for neuromodulation level updates each second of simulated time. This means that an update created with updateNM4Levels() will not go into effect until the next second of time in the simulation passes. Using setWeightAndWeightChangeUpdate() to set the CARLsim update function to every 10 ms with the INTERVAL_10MS setting can allow for the updateNM4Levels()'s modulation to go into effect faster. CARLsim will then check every 10 ms for changes to neuromodulation levels. For example, the function can be set as `setWeightAndWeightChangeUpdate(INTERVAL_10MS, false, 0.5f)`.

Example of usage:

`setWeightAndWeightChangeUpdate(INTERVAL_10MS, false, 0.5f)` This sets the simulation to check for neuromodulation updates every 10 ms.

`sim.updateNM4Levels(0, 0, false, false, true, true, 0, 0, 0.5, 0.25);` This sets the neuron group with groupID 0 to have its Acetylcholine and Noradrenaline levels updated. The levels for them are set to 0.5 and 0.25, respectively.

## Programming design

A similar approach to the setweight() function was used to create this function. Code for updateNM4Levels() was added to:
<br>/carlsim/interface/src/carlsim.cpp
<br>/carlsim/interface/inc/carlsim.h
<br>/carlsim/kernel/src/snn_manager.cpp
<br>/carlsim/kernel/inc/snn.h
<br>This is similar to files that include setweight() code. 

In snn_manager.cpp, the cudaMemcpy() function was used to copy the neuromodulation levels to CUDA memory with the grpACh and other neuromodulator variables. In CPU memory, grpACh and other neuromodulation variables have their values copied to memory. This updates the values in memory with the runtimeData objects for the neuromodulation levels. The other files that include updateNM4Levels() code are forms of code that allow the function to be accessed and pass data within the simulator.