CARLsim Developer Documentation - CSTP
=============================

This documentation will explain methods used in implementing synapse connection specific short term plasticity (CSTP) in CARLsim.

Methods used in implementing CSTP
----------------

<b>Spike events</b><br>
Spike events are tracked. When a spike occurs the formulas in fig 1. are used to process what signal should be added to a post synaptic neuron.

![STP Equations](http://uci-carl.github.io/CARLsim4/form_0.png)
Fig. 1. STP equations [1]. 
<details>
<summary>Programming: spike detection</summary>
First occurence of each presynaptic spike:<br>
File: snn_gpu_module.cu<br>
Method: kernel_conductanceUpdate()<br>
<br>
Each neuron and the presynaptic neurons that connect to it are looped through
and saved in postNId and preNId variables.<br><br>
A data bit is set when a spike occurs. A function getFiringBitGroupPtr() uses the [postId,preId] to check if the "firing bit" is set.<br><br>
</details>
<details>
<summary>Programming: STP formulas</summary>
<b>Fig. 1's equation 1</b><br>
du/dt = -u/tau_F + U * (1-u^-) * \delta(t-t_{spk})<br>
1. "-u/tau_F" is coded in the line: <br>
runtimeDataGPU.stpu[ind_plus] = runtimeDataGPU.stpu[ind_minus] * (1.0f - runtimeDataGPU.stp_tau_u_inv[lSId]);<br>
2. "U * (1-u^-)" is coded in the line: <br>
runtimeDataGPU.stpu[ind_plus] += runtimeDataGPU.stp_U[pos] * (1.0f - runtimeDataGPU.stpu[ind_minus]);<br>
3. "\delta(t-t_{spk})" is coded in the line: <br>
while (tmp_I_set) <br>
This looks for a firing bit set that represents a spike.<br>
<br>
<b>Fig. 1's equation 2</b><br>
dx/dt = (1-x)/tau_D - u^+ * x^- * \delta(t-t_{spk})<br>
1. "(1-x)/tau_D" is coded in the line: <br>
runtimeDataGPU.stpx[ind_plus] = runtimeDataGPU.stpx[ind_minus] + (1.0f - runtimeDataGPU.stpx[ind_minus]) * runtimeDataGPU.stp_tau_x_inv[lSId];<br>
2. " - u^+ * x^-" is coded in the line: <br>
runtimeDataGPU.stpx[ind_plus] -= runtimeDataGPU.stpu[ind_plus] * runtimeDataGPU.stpx[ind_minus];<br>
3. "\delta(t-t_{spk})" is coded in the line: <br>
while (tmp_I_set) <br>
This looks for a firing bit set that represents a spike.<br>
<br>
<b>Fig. 1's equation 3</b><br>
dI/dt = -I/tau_S + A * u^+ * x^- * \delta(t-t_{spk})<br>
1. "-I/tau_S" is coded in the line: <br>
runtimeDataGPU.AMPA_syn_i[synId] *= runtimeDataGPU.stp_dAMPA[pre_id];<br>
Where AMPA is replaced by each receptor processed.
Note: this is a part of the bug fix update.<br>
2. "A * u^+ * x^-" is coded in the lines: <br>
float STP_A = (runtimeDataGPU.stp_U[pos] > 0.0f) ? 1.0 / runtimeDataGPU.stp_U[pos] : 1.0f;<br>
change *= STP_A * runtimeDataGPU.stpx[ind_minus] * runtimeDataGPU.stpu[ind_plus];<br>
3. "\delta(t-t_{spk})" is coded in the line: <br>
while (tmp_I_set) <br>
This looks for a firing bit set that represents a spike.<br>
<br>
Unknown: <br>
Eq. 1: Why "-u/tau_F" appears processed as "u*(1-(1/tau_F))"<br>
Eq. 2: Why "(1-x)/tau_D" appears processed as "x+((1-x)/tau_D)"<br>
Eq. 3: How dirac delta is processed because it is a subtraction and not multiplication. In the code comments it is listed as "* \delta" instead of "- \delta" which is on the CARLsim site's equations. The code uses a different formula then listed on the software's user guide? The source scholarpedia article has "* \delta" so I suspect this is just a variation or error in the user guide.<br>
Eq. 3: The CARLsim user guide describes "A" as "A is the synaptic weight". However, the code processes A as "if stp_U > 0 then stp_U = 1/stp_U else stp_U = 1". stp_U represents synaptic weight? Why is stp_U inverted or set to 1? It is described as "scaling factor weighted" in comments and what does that mean? This is specific to "LN_I_CALC_TYPES" code. The source scholarpedia article appears to describe A as "the response amplitude that would be produced by total release of all the neurotransmitter".<br>
All eq.: Given that all equations in fig. 1 are derivatives, it is unclear where the source code solves for each variable's value for use in successive calculations in non-derivative form. For instance, u, x, and I, are all used in later calculations but where in the code are their values converted out of derivative form?<br>
Note: NS has seen similar equation coding of Izhikevich model derivatives in pg. 3 code of [3]. Therefore, while it is unclear how it works, there seems to be evidence that derivatives processing can be computed in such a way. Izhikevich added processing of the derivative calculation twice per 1 ms timestep for "numerical stability" but perhaps that is optional. A point of confusion is that in Izhikevich's code, there is "value = value + derivative" for application of a value's derivative to the value. In the CARLsim code it appears it is just "value = derivative". It is unclear how the derivative is added to the value rather than the value just set to equal the derivative in the CARLsim code.<br><br>
Potentially solved unknowns:<br>
Eq. 3: Why tau_d is encoded as 1-(1/tau_d) based on what tau_d a user inputs for a synapse. Where is that formula from? It is possible that this is from the conductance formula from eq. 13 in [2]. However, that formula is exp(1-(1/tau_d)) and it is unclear if exp() is included in the programming code.<br>
Answer: in fig. 1 equation 3, I * (1-(1/tau_d)) = I-I/tau_d. The reason tau_d is encoded as 1-(1/tau_d) is that appears to allow I*tau_d to perform the calculation on the left part of eq. 3's right side.
</details>
<br>
<b>Conductance variable</b><br>
CARLsim processes g (conductance) as a "multiplicative gain factor for fast and slow synaptic channels" [4]. G values in hippocampome's synaptic physiology page can be set using this parameter and it multiplies specific types of receptor current.<br>
<br>
<b>Current decay</b><br>

![Equation with tau_d](http://uci-carl.github.io/CARLsim4/form_53.png)
Fig. 2. Equation with tau_d [2].<br>
Synaptic currents are decayed with the use of the tau_d variable. Fig 2. shows how tau_d (tau in the equation) contributes to the conductance variable, g. The current decay is processed every timestep.<br><br>
<b>Variable updates</b><br>
Every timestep the STP variables are updated. As described in fig. 1's equations, certain parts of the equations are included only during the timestep a spike occurs. The other parts of the equations are updated over every timestep.<br>
<details>
<summary>Programming: Variable updates</summary>
The code for updating variables is seperated based on if it updates each timestep or only when a spike occurs. The right part of the left side of the equations that is spike dependent has its code located in the kernel_conductanceUpdate() method. The left part of the right side of the equations updates every timestep has its code located in kernel_STPUpdateAndDecayConductances().<br>
</details>
<br>
<b>Variable indices</b><br>
An important part of calculations in a network as opposed to single synapses is assinging to which neurons each variable belongs. This is variable indexing where the indices are neuons. Each variable can belong to either a presynaptic neuron (preId), post synaptic neuron (postId), preId+postId pair at an individual level (synId), or preId+postId pair at a group level (connId). This information was not included in the STP formulas due it seems to be from the formulas only representing individual and not groups of synapses.<br>
<br>
In STP, when spikes occur changes happen to presynaptic neuron chemical balances [5]. For instance, in short-term depression (STD) neurotransmitters are depleted at the axon. In short-term facilitation (STF), an influx of calcium occurs in the axon. In the synapse model, STD and STP are managed by u and x variables. The u and x variables are assigned to the presynaptic neuron index. The A variable is the synaptic response amplitude or weight (see programming: STP formulas: unknown section for more info). The A variable is assigned to a pair of preId and postId at the individual synapse level. This represents that each [preId,postId] can have its own synaptic response amplitude. I (current) is assigned to a pair of preId and postId at the individual synapse level. This means each synapse has its own unique current level. I current will eventually be added to a post-synaptic neuron at which point that value is assigned to a single post-synaptic neuron and not a synapse. Tau_d (aka tau_s in fig. 1) is assigned to [preId,postId] at the group level. Neuron types can be represented in CARLsim using neuron groups. Tau_d being at the group level means that it has a specific value for a neuron type pair.<br>
<br>
<b>CSTP and non-CSTP results differences</b><br>
In simulations with STP and without CSTP it has been observed that there was a bug in non-CSTP processing of STP. A bug fix has been sent to the CARLsim team. References: https://github.com/UCI-CARL/CARLsim6/issues/16, https://github.com/UCI-CARL/CARLsim6/pull/17.

<br>References:<br>
[1] http://uci-carl.github.io/CARLsim4/classCARLsim.html#a54170c96807d6cdf699fa3664f81505e<br>
[2] http://uci-carl.github.io/CARLsim4/ch3_neurons_synapses_groups.html<br>
[3] https://ieeexplore.ieee.org/abstract/document/1257420/<br>
[4] http://uci-carl.github.io/CARLsim4/ch4_connections.html#ch4s1s4_receptor_gain<br>
[5] http://www.scholarpedia.org/article/Short-term_synaptic_plasticity<br>