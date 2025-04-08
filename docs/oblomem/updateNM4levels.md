updateNM4Levels Development Documentation
=========================================

This documentation provides development documentation about the feature updateNM4Levels 
that was created for the Oblomem neuromodulation research. This feature allows neuromodulation 
levels to be altered at runtime during a CARLsim experiment. Originally, neuromodulation 
variable could only be altered once per experiment and were set in the configuration state of 
an experiment.

## Usage
updateNM4Levels(int netID, int groupID, bool updateDA, bool update5HT, bool updateACh, bool updateNE, float levelDA, float level5HT, float levelACh, float levelNE);

Parameters
<table border=1>
<tr><td>parameter</td><td>description</td></tr>
<tr><td>netID</td><td>the ID of a network. If only 1 network is in the simulation then this should be set to 0.</td></tr>
<tr><td>groupID</td><td>the ID of a group.</td></tr>
<tr><td>baseDP</td><td>the baseline concentration of Dopamine</td></tr>
<tr><td>tauDP</td><td>the decay time constant of Dopamine</td></tr>
<tr><td>base5HT</td><td>the baseline concentration of Serotonin</td></tr>
<tr><td>tau5HT</td><td>the decay time constant of Serotonin</td></tr>
<tr><td>baseACh</td><td>the baseline concentration of Acetylcholine</td></tr>
<tr><td>tauACh</td><td>the decay time constant of Acetylcholine</td></tr>
<tr><td>baseNE</td><td>the baseline concentration of Noradrenaline</td></tr>
<tr><td>tauNE</td><td>the decay time constant of Noradrenaline</td></tr>
</table>	

Example:
sim.updateNM4Levels(0, 0, false, false, true, true, 0, 0, p.ach_old_obj_loc, p.ne_old_obj_loc);


sim.setWeightAndWeightChangeUpdate(INTERVAL_10MS, false, 0.5f);