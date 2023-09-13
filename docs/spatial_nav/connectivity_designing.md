Connectivity design methods
===========================

This documentation will cover methods and reasons to design connectivity in the simulation.

## Grid cell to interneuron connections

Selecting which connections should occur from grid cells to interneurons (INs) can help control the spacing, positioning, and rotation of grid feilds. Adding grid cell to interneuron connection can help reinforce firing in bump within a rotated grid pattern. Each IN is connected to grid cells in a center-surround (CS) connection distribution. The CS distributions support the firing of bumps in their centers and inhibit firing in their surround regions. Bump firing leads to grid field firing in firing vs. animal location rate map (physical space) plot. Controling bump firing locations through selecting connectivity therefore leads to controling the positions, spacing, and rotations of grid fields.

Selecting bump locations by designing CS connectivity is able to reinforce firing in fields within a rotated grid pattern. This firing helps influence the grid patterns to remain in a formation that fits those fields instead of drifting into another rotation. In addition, a specific desired rotation angle can be set and the one-to-few grid cell to INs connections help sustain that selected angle to persist over simulated time. In the animal data plots, specific rotations of grid patterns have been observed. The one-to-few neurons connected can be selected in a way that helps match the intended rotation observed in the animal data. Selecting which INs are connected can also help reinforce spacing and positioning of fields between each other to reproduce that seen in the animal data.

Some things to keep in mind are adding an amount of IN connections that is within biologically reported levels. The article describes some of the properties associated with these levels such as connectivity found in animals, synapse parameters that modulate the amount of inhibition signaled that is affected by connectivity, and how connectivity may be specific to the dorsoventral axis of the medial entorhinal cortex. A balance is typically intended between advantages gained by adding connections and keeping connectivity within biologically realistic levels.

## Programming grid cell to IN connections

To be added later...

## Programming CS connections

To be added later...