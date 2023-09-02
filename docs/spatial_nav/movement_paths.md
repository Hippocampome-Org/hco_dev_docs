Movement paths
==============

These parameters set which trajectory of movement the virtual animal should follow.

move_straight Movement Test
===========================

A basic virtual animal movement test is the move_straight test. This test can be used for purposes such as measuring bump movement speed while bumps travel in a stright line direction. To enable this test, set "move_straight = 1" and all other bool variables in the movement trajectory section to 0.

move_animal Movement Path
=========================

This movement trajectory follows the path an animal moved in recorded real animal data. This path requires that files for the animal movement path be provided. Additional software is included that reformats data recorded in animals into files that can be used in this software. This software is available in the "moves_converter" directory.

move_animal_onlypos Movement Path
=================================

This function will move a virtual animal all through a trajectory specified by a real animal's recorded movement file but not simulate any spiking from neurons. A purpose of this function is to preview the position of a trajectory when centering it for plotting animal movement data. It is designed to generate results very fast so that many starting positions of an animal in an enviorment can be tried to refine the trajectory's position for plot. The starting point of an animal in an enviornment can be set with pos\[2\] and bpos\[2\] in general_params.cpp. Changing the starting point shifts where the rest of the trajectory of movement occurs, as can be seen in plots. Centering the trajectory for plots is useful when real animal movement data is converted into movement data for the simulation and the movement path is wanted to be centered for plots. This function's preview of a trajectory can help with that.

A note is that because no spikes are generated, plotting should have "plot_spikes" turned of to avoid an error. This can be done in plotting scripts such as gridscore_sim_run_local.m. 

move_animal_aug Movement Path
=============================

This is designed to run virtual animal movement based on real animal data to a specified time then augment the movement trajectory with synthetically generated movement which efficiently covers areas of the enviorment the animal has yet to visit. The intention is that it can take a long time, e.g., hours, before some enviornment areas are visited to be able to see cell response in those areas. This attempts to accelerate the visiting of most general areas of the environment while still including real animal movements. This movement function may not be working correctly because its software development never completed. Users can check the results and code to see if this works. Research methods used different strategies and never ended up needing this code so it is left as partially completed perhaps.