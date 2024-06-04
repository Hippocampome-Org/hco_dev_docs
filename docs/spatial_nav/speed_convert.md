Speed conversions
=================

This documentation describes how to use the speed conversion function in the simulation software. The is a somewhat advanced feature that is optional to use. It is not required to be used to work with the simulation software.

## speed_conversion function

In general_params.cpp there is a speed_conversion variable. This variable can be used through a function associated with it to cause attractor bumps to move faster or slower relative to animal movement speeds. The effect is a constant multiplier on animal speeds input into the simulation. For example, all animal speeds could be processed at 75% or 120% of their original values. This does not change the speed the animal was recorded as traveling, but instead what speed the attractor bumps should move using the animal speed input multipler to control attractor bump speed. The effect of creating slower bump speeds can be larger grid field firing sizes and spacings. Faster bump speeds can have the opposite effect.

## resolution_converter and plot_shift variables

The speed_conversion function was not used to produce any results reported in the article but is used in the sml_scl_hgh_fr_4 project described in the plotting and analyzing results documentation. If the speed_conversion function is used to slow the speed of bump movements then a physical space plot will include a smaller space of bump movements in the plot unless the resolution_converter and optionally plot_shift variables are used while plotting. For example, the speed_conversion variable set to 1.0 can cause a total plotting of horizontal bump firing of 32 pixels. The speed_conversion variable set to 0.72 would then cause that firing to span ~23 pixels. The resolution_converter variable can be set to 1.39 (which is 1/0.72) to plot the firing at a 32x32 pixel resolution. Plotting at the higher resolution can help with comparing simulated plots to real cell plots which may have a resolution such as 32x32 pixels. The plot_shift variable can help with centering the neural layer subsection selection that is chosen to be included in the physical space plot. For instance, using the speed_conversion function can cause the subsection to shift to an off-center position that a user can want to re-center in the physical space plot with the plot_shift function.

## Use of the move_animal_onlypos function

Another way to center the neural subsection chosen to be used with the physical space plot is to use the move_animal_onlypos function along with testing trajectory plot coordinates in plots. A user should remember to set plot_spikes = 0 when plotting results from move_animal_onlypos to avoid a plotting error. The pos\[2\] and bpos\[2\] variables in general_params.cpp can be used to position the trajectory plot coordinates which in turn affect the position of firing plotted in the physical space plot. Some experimentation with pos\[2\] and bpos\[2\] values can be used to find the centering. A note is that the pos\[2\] and bpos\[2\] variables should always have the same values.

## Overall notes

Overall, the speed_conversion and associated functions can be helpful tools to design simulated grid cells that fit the properties of real cells. Achiving specific grid field sizes and spacings can be aided with these methods.