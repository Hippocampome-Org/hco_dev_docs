Automated speed control
=======================

This documentation describes ways to create automated speed control functions for controlling continuous attractor network bump movement. This movement tracks animals' movements.

## Speed control methods

For controlling bump speed in the simulation, two signal levels adjusted are the amount of conjunctive cells' signals non-specific to a direction and conjunctive cells' signals specific to a direction. The non-specific signal is coded with the variable base_ext (aka base external input to grid cells) and the specific signal is speed_signaling (signal greater in at least one preferred direction). Those signals influence the direction and speed of bump movement and affect each other’s contributions to that. 

Animal speeds and directions were retrieved recorded data and formatted for use in the simulation. In work in the article, the animal recordings had a sampling rate of reporting the animal’s position every 20 milliseconds. Differences between locations were used to extract speeds and directions from animal recordings. A script was created that reported the percentage of time the animal spent at each speed. That script is moves_reporter.m. The reporting of what amount of time the animal spent at different movement speeds helps inform which speeds to focus the speed control function on, assuming an interest in recreating movements from the animal dataset.

## Concept of speed control

The concept is that an animal would have learned signal responses that direct grid cell firing in a way that tracks its movement. The article's work does not simulate the learning process but this is an area of future work that could be explored. Simulations in the article provide speed control functions that manage the signaling as though neurons had been previously trained to generate the signals. A user can create there own speed control functions to produce such management.

## Creating speed a control function

Online tools were used in the article and can be accessed by users to help create speed control functions. It can be useful to fit speed_signaling data using a polynomial regression curve, and a tool for that is (Lutus, 2023). base_ext can typically be fit to a sigmoidal curve and a tool for that is (MyAssays Ltd., 2023). A user can use the move_straight function to program the bumps to move at a 90 degree angle. The user can then adjust base_ext and speed_signaling values to find settings that cause the bumps to move at intended speeds. 

Speeds can be evaluated by, per se, every second how many pixels does the center of the bump move. Each pixel normally represents a grid cell on the plot, and therefore the speeds is how many neurons does the bump's center move in position each second. This can be thought of as "moves per second". Once data on moves per second is collected along with signal levels, that can be input into the curve fitting tools to find equations that generate the intended signals. These equations can then be input into move_path.cpp's control_speed function to cause the signals to be created given a speed input. In general_params.cpp, once auto_speed_control is enabled then the simulation will use the control_speed function to manage speed control. The speed control function can be tested for performance using move_straight with different speeds. 

Both the neural layer and area the real animal environment was recorded from was square. Because of this, the neural layer can be conceptually overlayed over the environment. The north compass direction (0 degrees) was assigned the same position in the neural layer and the real environment. Perhaps such direction assignment cognitively could be different between a neural layer and real animal’s environment but for simplicity this approach was used in this work.

## Place cell input

Place cell input to the grid cell layer helps prevent grid field firing from drifting to unintended positions. Further details are in the article's supplementary materials section V. The place cell firing was theta modulated with phase precession. The reasoning for this is that place cells have often been reported to have those properties in animal experimentation articles.

References:

Lutus, P., Accessed online July 4, 2023. Polynomial regression data fit tool. Copyright 2023. https://arachnoid.com/polysolve/

MyAssays Ltd., Accessed online July 4, 2023. Online mathematical curve-fitting tool. https://www.mycurvefit.com
