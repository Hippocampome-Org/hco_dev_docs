General Experiment Components
=============================

This documentation contains descriptions about experiment components in Oblomem.

## Acetylcholine (ACH) Levels as Current

The variable `ach_calcium2current` in general_params.cpp enables using ACh levels as current input to CA3 Pyramidal cells, such as cells that are included in concept cell ensembles. This current can replace some of the log-normally distributed background activity current in the simulation. The purpose of this simulation component is to create a simple way to include ACh neuromodulation of neural activities.

The vector `ach_level` supplies ACh level measurements that manage the amount of current supplied by this method. `ach_level` represents levels of activity captured in animal recordings. The parameters `calcium2current` and `calcium_min` help control the amount of current supplied.

## Controlling Frequency of Ensemble Presentation

Some methods are included to create a periodic presentation of ensembles instead of ensembles constantly being presented any time a virtual animal encounters a concept matching the ensemble. For example, an animal in an environment location does not need the concept of the location is in presented to it constantly when it is in the location to learn a neural representation of the environment. Instead, the concept ensemble of a location, represented as per se 150 selected neurons being activated, can be presented 3 times a second for 20 ms (gamma rhythm length) when the animal is in that location. If the animal moves to a different location midway through that second then a different location pattern can start to be presented at that frequency.

The concept is that the brain periodically recognizes stimuli representing what location it is in. In this way, the animal's attention is only recognizing its location periodically, instead of thinking of the concept "location" non-stop when it is in a location. This can concerve brain resources to no need to be expending energy to remind oneself of being in a location endlessly. Also, it can avoid fatiguing one's thought processes of the idea of location being replayed constantly. Concaptually, location can be represented in indirect types of perception stimuli. For instance, place cells respond to smells, sights, sounds, etc., associated with a location. All of these things are generalized here as sensory input recognizing a location. It is an abstraction, which could be modeled in more detail in future work, but currently it is kept simplified at the level of work the project is at.

This "pace setter" of ensemble presentation, being the control of how frequent ensembles are presented, is included in specific areas of the source code. For place cell ensembles (assemblies) this is partly controlled by the lines such as 
<br>`...time_in_interval >= p.present_first_assembly...`
<br>and
<br>`...{pc_gamma_t_rem--;} // decrement time in gamma...`.
Context cells use similar lines of code to control this.

Object concept ensemble presentation is allowed to continuously occur when `obj_exp` is equal to any value except `0`. This uses the idea that the animal's attention is actively on an object, e.g., non-stationary object, during that condition. It is actively "exploring" that object. The `obj_exp` being equal to non-zero values is set based on animal recordings data read into the simulation. This is retrieved from the `object_exp.cpp` file and the creation of that file is documented elsewhere.

Letting the object ensemble present continually during exploration, and restricting the frequency of location ensemble presentations, helps adjust for the imbalance of the amount of time an animal spends in each condition. The animal is always in a location but only sometimes is exploring an object. This can cause an imbalance in training, and synaptic strengthening of neurons associating with these concepts potentially. Training can be more balanced when the patterns presented for location are brought closer to the number of patterns presented for object exploration by restricting the amount of pattern presentations of locations. If wanted, one can alter the frequency of such ensemble presentations, but this may cause challenges for training ensembles to respond to stimuli. There is a specific balance of learning and then unlearning or suppressed memories as a goal of this work, and balancing the training pattern presentation levels in this way can potentially help this effort.

The line
<br>`if (last_obj_exp_assemb == -1 || t - last_obj_exp_assemb >= p.theta_cycle) {`
<br>seems to have been from testing only allowing object exploration pattern presentation once per theta rhythm frequency. This is not currently enabled as a constraint because `last_obj_exp_assemb` is never set to anything other than `-1`. Part of a method to add theta to the timing could be to have `last_obj_exp_assemb` set to `t` when `obj_pres_active` first becomes `true` to cause this constraint to become active it seems. `obj_pres_active` has a bug as documented in the work_report.docx document in the section starting with "Added 05/29/25...". That should be fixed in order to rely on `obj_pres_active` for functionality.

## non_pattern_pres_run

This function can be used to set current back to baseline levels. This is in addition to being able to set the current for non pattern presentation periods. Boolean parameter help control its operations. It uses distributions of current levels to represent baseline levels. Baseline refers to a non-pattern presentation level. For instance, any patterns can be presented, and the experiment time is moved forward by 1 timestep (1ms), and the current is set back to non-presentation levels. The current will be at a non-presentation level unless pattern presentation code sets the current back to higher levels (pattern presentation levels) before the next timestep of the simulation occurs.

The parameters pc_pres_active and obj_pres_active are used to track when to set the current back to baseline levels specifically for pyramidal cells. This is due to pattern presentation cells being pyramidal cells. The distribution of current as background levels is only updated when a pattern presentation is not occuring. I used Jeffrey's original design for this from (Kopsick et al., 2024). I am unsure why the interneuron current would not update at that time, and one can change it so it does if wanted. This was also noted in the work_report.docx document in the paragraph with the line "...control of current during pattern presentation...".
