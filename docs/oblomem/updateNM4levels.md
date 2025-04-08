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
</table>	
<br>netID	the ID of a network. If only 1 network is in the simulation then this should be set to 0.
<br>groupID	the ID of a group.
<br>baseDP	the baseline concentration of Dopamine
<br>tauDP	the decay time constant of Dopamine
<br>base5HT	the baseline concentration of Serotonin
<br>tau5HT	the decay time constant of Serotonin
<br>baseACh	the baseline concentration of Acetylcholine
<br>tauACh	the decay time constant of Acetylcholine
<br>baseNE	the baseline concentration of Noradrenaline
<br>tauNE	the decay time constant of Noradrenaline

Example:
sim.updateNM4Levels(0, 0, false, false, true, true, 0, 0, p.ach_old_obj_loc, p.ne_old_obj_loc);


sim.setWeightAndWeightChangeUpdate(INTERVAL_10MS, false, 0.5f);