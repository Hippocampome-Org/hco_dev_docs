updateNM4Levels Development Documentation
=========================================

This documentation provides development documentation about the feature updateNM4Levels 
that was created for the Oblomem neuromodulation research. This feature allows neuromodulation 
levels to be altered at runtime during a CARLsim experiment. Originally, neuromodulation 
variable could only be altered once per experiment and were set in the configuration state of 
an experiment.

## Usage
# updateNM4Levels()
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

Example:
sim.updateNM4Levels(0, 0, false, false, true, true, 0, 0, p.ach_old_obj_loc, p.ne_old_obj_loc);


sim.setWeightAndWeightChangeUpdate(INTERVAL_10MS, false, 0.5f);