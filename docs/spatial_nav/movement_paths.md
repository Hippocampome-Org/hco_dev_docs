Movement paths
==============

These parameters set which trajectory of movement the virtual animal should follow. For running a movement function, its boolean value should be set 1 in general_params.cpp and other boolean values in the movement trajectory section should be set to 0.

move_straight
=============

A basic virtual animal movement test is the move_straight test. This test can be used for purposes such as measuring bump movement speed while bumps travel in a stright line direction. 

move_fullspace
==============

This function runs a test pattern of movement that efficiently visits each general area of an enviorment in a systematic way. The name "full space" refers to how it is designed to cover movement in the general full space of an enviornment efficiently. The time in which this full space coverage can occur depends on the speed at which the movement is set to occur. This movement function is useful for purposes such as parameter exploration with testing a large number of parameters on their grid score affects, and it is useful for previewing potential grid patterns that can be formed if longer experiments are run with animal data.

move_animal
===========

This movement trajectory follows the path an animal moved in recorded real animal data. This path requires that files for the animal movement path be provided. Additional software is included that reformats data recorded in animals into files that can be used in this software. This software is available in the "moves_converter" directory.

move_animal_onlypos
===================

This function will move a virtual animal all through a trajectory specified by a real animal's recorded movement file but not simulate any spiking from neurons. A purpose of this function is to preview the position of a trajectory when centering it for plotting animal movement data. It is designed to generate results very fast so that many starting positions of an animal in an enviorment can be tried to refine the trajectory's position for plot. The starting point of an animal in an enviornment can be set with pos\[2\] and bpos\[2\] in general_params.cpp. Changing the starting point shifts where the rest of the trajectory of movement occurs, as can be seen in plots. Centering the trajectory for plots is useful when real animal movement data is converted into movement data for the simulation and the movement path is wanted to be centered for plots. This function's preview of a trajectory can help with that.

A note is that because no spikes are generated, plotting should have "plot_spikes" turned of to avoid an error. This can be done in plotting scripts such as gridscore_sim_run_local.m. 

move_speed_change
=================

This function tests a series of speed changes in movements. Something to observe with this test is if changes in speeds cause residual effects on bump movement. For instance, if some signals take longer to dissipate that were present at different speeds when a speed change occurs, then initial bump speed activity effects can be observed before the signals are rebalanced to those intended for a specific speed.

move_circles
============

This function tests virtual animal movement in circles. It can be helpful to assess how well the bumps can consistently perform movement at different angles.

move_random
===========

This function tests random direction movements. This can be useful to test movement in a variety of directions.

move_ramp
===========

This function tests movement with speeds and/or angles ramping up then down repeatedly. This can be helpful to see how signals affect bumps during build up or reduction of speed or angle of movement.

move_animal_aug
===============

This is designed to run virtual animal movement based on real animal data to a specified time then augment the movement trajectory with synthetically generated movement which efficiently covers areas of the enviorment the animal has yet to visit. The intention is that it can take a long time, e.g., hours, before some enviornment areas are visited to be able to see cell response in those areas. This attempts to accelerate the visiting of most general areas of the environment while still including real animal movements. This movement function may not be working correctly because its software development never completed. Users can check the results and code to see if this works. Research methods used different strategies and never ended up needing this code so it is left as partially completed perhaps.