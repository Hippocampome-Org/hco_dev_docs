Methods to change the size of neural layers
===========================================

These are settings that need to be updated if a user wants to change neural layer sizes in the simulation.

## Neuron counts

### Setting grid cell layer size

In general_params.cpp, set x_size and y_size to new layer size. This represents the x and y axes of the layer. This change will set the grid cell layer size as long as that is set with the constant layer_size.

The total layer size is recommended to be evenly divisible by 2. This is because it may help aligning prefered directions in synaptic weight distribution design with the sim. For example, for the whole grid cell layer, having an even number of preferred direction neurons in the 4 main compass directions.

### Changing other layer sizes

Changing x_size and y_size will change other neural layer sizes as long as they use the layer_size cosntant for their sizes. Additional neuron layer sizes are specified with "\_Count" constant names. These can be changed individually if wanted.

### Changing interneuron layer sizes

Interneuron layer sizes, e.g., EC_LII_Axo_Axonic (AA), MEC_LII_Basket (BK), and EC_LII_Basket_Multipolar (BM), can be set individually. The interneurons work in cooperation with each other to create what could be thought of as a combined layer together. Because of that, the square root of their combined total should be a number that does not have a fraction. For instance, if the interneuron layers are intended to create a combined 50 x 50 neuron layer their total should be 2500 neurons. This is split across their number of individual layers, e.g., 834, 833, and 833, for the amounts in each interneuron layer. Currently, the greater number neuron layer size is recommended to be included in the order of AA, BK, BM. Specifically, if two layers are greater than the third, those greater size layers are recommended to be AA and BK. The reasons for this is that some programming functions may expect to make connections with uneven sized interneuron layers in this order.

## External current functions

All vectors that are used to set initial current to grid cells with setExternalCurrent must have ext_dir arrays that match the grid cell layer size. Such vectors are ext_dir, ext_dir_initial, and perhaps init_firings.cpp. The values for the vectors are in ext_dir.cpp, ext_dir_initial.cpp, and init_firings.cpp. Scripts have been included that can help with the resizing of the vectors. Those scripts are resize_ext_dir.m and resize_ext_dir_init.m. The vector from init_firings.cpp might not need to be resized if the grid cell layer size is at least 30x30.

## Synaptic connectivity

Synaptic connectivity between grid cells and interneurons will need to be adjusted to accomidate the different layer sizes. The script generate_weights.m is designed to help generate synaptic weight values for connections from interneurons to grid cells. Settings in that script should be set to the new layer size. For example, grid_size_ref and grid_size_target. Some other variables that should be altered in the script include start_x_shift and start_y_shift. Also, the center and surround sizes in the center-surround ring should be set proportionally to the amount of inhibition wanted with the new layer size. See connectivity design methods documentation for more instructions.

## Conductance (g) constant

In general_params.cpp the g_fast and g_slow parameter values may need to be adjusted to levels that fit with the new layer sizes.

## Animal data starting position

The starting position of the virtual animal, set by pos\[2\], and starting bump position, set with bpos\[2\], need to be adjusted to fit with the new layer size.

## Place cell input

The parameters for place cell input should be adjusted to fit the diameter of bumps given the new layer size.

## Plotting software

The plotting software needs to be updated for the new layer size. For example, in activity_movie_nrn_spc.m update x_size and y_size. In activity_image_phys_spc_smooth.m, update plot_subsect, grid_size, and plot_size.

## move_fullspace

If the move_fullspace trajectory function in move_path.cpp is to be used, update pos\[0\], pos\[1\], and bpos to have a correct starting offset position.

## Starting position of center-surround centroid

This should be automatically set with x_srt and y_srt but his can be checked if wanted.