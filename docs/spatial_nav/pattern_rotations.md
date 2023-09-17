Grid pattern rotation methods
=============================

This documentation will prodive further information about concepts involved with grid pattern rotations and methods to create them in the simulation.

## Grid pattern rotations in real cells

It can be observed in real cell recordings that rotations are present. This can be observed in plots in this work's article and many other articles. A grid pattern rotation is the rotation of the six grid fields surrounding a center grid field being rotated around an axis positioned at the middle of that center grid field.

## Simulating a rotated pattern

Applying an angle of rotation to the animal movements input into the simulation creates a rotation of bump movement that translates into a rotation of grid fields in physical space plots. This is able to help recreate rotations of grid patterns that are observed in real cells. The parameter `grid_pattern_rot` in general_params.cpp is used to set the movement input rotation to an angle that the user wants.

It is helpful to consider where the borders of the movement input (trajectory) will be positioned relative to the neural layer's plot borders when selecting the scale and position of the trajectory used with the virtual animal. This can avoid an appearance in plots of grid fields wrapping around the borders of the plot as described in the "grid field plot border wrapping" documentation. For instance, rotating a square movement area can cause the upper right or bottom left of the trajectory to be closer to a square neural layer's borders than if it was not rotated. Selecting some amount of distance from the neural layer borders even when rotated can avoid the field wrapping.

A way to preview where the trajectory is positioned relative to the neural layer is to use the "move_animal_onlypos" movement function as described in the "movement paths" documentation. That can be used in combination with virtual animal starting position settings as described in the "converting animal recordings" documentation to create plots of where the trajectory occurs relative to the neural layer. 