CARLsim Developer Documentation - CSTP
=============================

This documentation will explain methods used in implementing synapse connection specific short term plasticity (CSTP) in CARLsim.

Methods used in implementing CSTP
----------------

<b>Spike events</b><br>
Spike events are tracked. When a spike occurs the formulas in fig 1. are used to process what signal should be added to a post synaptic neuron.

![STP Equations](http://uci-carl.github.io/CARLsim4/form_0.png)
Fig. 1. STP equations (CARLsim STP). 
<details>
<summary>Programming: spike detection</summary>
First occurence of each presynaptic spike:<br>
File: snn_gpu_module.cu<br>
Method: kernel_conductanceUpdate()<br>
<br>
Each neuron and the presynaptic neurons that connect to it are looped through
and saved in postNId and preNId variables.<br><br>
A data bit is set when a spike occurs. A function getFiringBitGroupPtr() uses the [postId,preId] to check if the "firing bit" is set.<br><br>
atomicAdd() is used to add each new conductance value from synaptic spikes to a common gReceptor variable. The atomic operation allows the gReceptor variable to be added to as shared memory while multiple threads are computing new conductance variables and may want to add to the gReceptor variable at the same time.<br><br>
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
runtimeDataGPU.AMPA_syn_g[synId] *= runtimeDataGPU.stp_dAMPA[pre_id];<br>
Where AMPA is replaced by each receptor processed.
Note: this is a part of the bug fix update.<br>
2. "A * u^+ * x^-" is coded in the lines: <br>
float STP_A = (runtimeDataGPU.stp_U[pos] > 0.0f) ? 1.0 / runtimeDataGPU.stp_U[pos] : 1.0f;<br>
change *= STP_A * runtimeDataGPU.stpx[ind_minus] * runtimeDataGPU.stpu[ind_plus];<br>
3. "\delta(t-t_{spk})" is coded in the line: <br>
while (tmp_I_set) <br>
This looks for a firing bit set that represents a spike.<br>
<br>
Potentially resolved questions:<br>
Eq. 3: Why tau_d is encoded as 1-(1/tau_d) based on what tau_d a user inputs for a synapse. Where is that formula from? It is possible that this is from the conductance formula from eq. 13 in (CARLsim COBA). However, that formula is exp(1-(1/tau_d)) and it is unclear if exp() is included in the programming code.<br>
Answer: in fig. 1 equation 3, I * (1-(1/tau_d)) = I-I/tau_d. The reason tau_d is encoded as 1-(1/tau_d) is that appears to allow I*tau_d to perform the calculation on the left part of eq. 3's right side.<br>
Eq. 1: Why "-u/tau_F" appears processed as "u*(1-(1/tau_F))"<br>
Answer: u = u -u/tau_F is equivalent to u = u * (1-(1/tau_F))<br>
Eq. 2: Why "(1-x)/tau_D" appears processed as "x+((1-x)*(1/tau_D))"<br>
Answer: x = x + ((1-x)/tau_d) is equivalent to x = x + ((1-x)*(1/tau_D))<br>
Eq. 3: How dirac delta is processed because it is a subtraction and not multiplication. In the code comments it is listed as "* \delta" instead of "- \delta" which is on the CARLsim site's equations. The code uses a different formula then listed on the software's user guide? The source scholarpedia article has "* \delta" so I suspect this is just a variation or error in the user guide.<br>
Answer: the user guide has an error here. Looks like a formatting error where x^- was intended where x- was used.<br>
Eq. 3: The CARLsim user guide describes "A" as "A is the synaptic weight". However, the code in snn_manager.cpp, snn_gpu_module.cpp, and snn_cpu_module.cpp, processes A as "if stp_U > 0 then stp_A = 1/stp_U else stp_A = 1". stp_A represents synaptic weight? Why is stp_A set to inverted stp_U or 1? It is described as "scaling factor weighted" in comments and what does that mean? This is specific to "LN_I_CALC_TYPES" code. The source scholarpedia article appears to describe A as "the response amplitude that would be produced by total release of all the neurotransmitter". The reference article (Tsodyks, Pawelzik, Markram, 1998) describes A_se as the "absolute synaptic strength". Given there is another "wt" variable CARLsim uses for synaptic weight, it perhaps makes sense to consolidate the "A" and "wt" variables into one variable so that there are not two variables encoding synaptic strength.<br>
Something perhaps important about A = 1/U is that this appears to serve the same function as g0 = g0 / U in (Moradi, 2022). In CARLsim code, g is processed as a conductance variable when the connect() function is called. It never has a timepoint 0, g0 = g0 / U, conversion that is seen in (Moradi, 2022)'s calculations. "A" being set to 1/U causes the same g/U calculation to be done during a synaptic spike update. Still, perhaps A could be combined with wt or renamed to something other than "synaptic strength", but this is perhaps the role it currently plays in calculations.<br>
Answer: It appears the stp_A value is used for the same purpose of g0 = g0/U in (Moradi, 2022).<br>
All eq.: Given that all equations in fig. 1 are derivatives, it is unclear where the source code solves for each variable's value for use in successive calculations in non-derivative form. For instance, u, x, and I, are all used in later calculations but where in the code are their values converted out of derivative form?<br>
Note: NS has seen similar equation coding of Izhikevich model derivatives in pg. 3 code of (Izhikevich, 2003). Therefore, while it is unclear how it works, there seems to be evidence that derivatives processing can be computed in such a way. Izhikevich added processing of the derivative calculation twice per 1 ms timestep for "numerical stability" but perhaps that is optional. A point of confusion is that in Izhikevich's code, there is "value = value + derivative" for application of a value's derivative to the value. In the CARLsim code it appears it is just "value = derivative". It is unclear how the derivative is added to the value rather than the value just set to equal the derivative in the CARLsim code.<br>
Answer: from NS' understanding, the integration form of eular's forward numerical integration was used. In this method, derivatives are integrated by a value = value * dt * value_derivative. However, NS is not very familar with the specifics of the integration method so this info. should be investigated by anyone that wants to use it. Runge Kutta RK4 method is an alternative to Euler's method and it seems the Izhikevich equations have the option of using either integration method in CARLsim but the TM synapse model may only have Euler's method currently.<br><br>
</details>
<br>
<b>Conductance variable</b><br>
CARLsim processes g (conductance) as a "multiplicative gain factor for fast and slow synaptic channels" (CARLsim Gain Factors). G values in hippocampome's synaptic physiology page can be set using this parameter and it multiplies specific types of receptor current.<br>
<details>
<summary>Programming: Conductance variable</summary>
For each receptor, e.g., AMPA, the conductance constant is applied in lines such as "AMPA_sum = change * d_mulSynFast[connId]" in snn_gpu_module.cu or snn_cpu_module.cpp. The conductance constant is set when the connect() function is called for two neuron groups.<br>
A change from non-connection-specific STP to connection-specific STP is that AMPA_sum, and other receptors, are set to equal "change * d_mulSynFast[connId]" instead of "+=". This is due to each synapse having its signal stored in syn_g variables (e.g., AMPA_syn_g) instead of pooled in the gReceptor (e.g., gAMPA) variable. Lines such as "runtimeDataGPU.gAMPA[postNId] += AMPA_sum" are included to add the current at the timestep the spike occurs. Every timestep after that the current will be decayed in a connection-specific way (specific pre- and post-synaptic neuron current). The later code mentions of lines such as "runtimeDataGPU.gAMPA[postNId] += AMPA_sum" in the kernel_conductanceUpdate function were removed because the current needs to be added at the time in the loop when the current specific to a pre- and post-synaptic neuron is present. Each looping through pre neurons disposes of the AMPA_sum, etc., signal because of the "=" instead of "+=" change described above.<br>
</details>
<br><br>
<b>Synaptic weights</b><br>
Neurons can have synaptic strengths developer over learning with long-term placticity. A simulation that does not include learning methods such as spike-timing dependent placticity can still include synaptic strengths and assume these have been learned prior to the simulation time. A method for learning new weights is needed if the simulation is to include synaptic strength changes.<br>
<br>
Synaptic weights are specified when the connect() function is called for two neuron groups. The weights are a multiplier of the synaptic current that is produced through synapses.<br>
<details>
<summary>Programming: Synaptic weights</summary>
The weight is retrieved and stored in a "change" variable. E.g., in the line "change = runtimeDataGPU.wt[cum_pos + wtId];" in snn_gpu_module.cu or snn_cpu_module.cpp. Each time a pre-synaptic spike occurs which causes synaptic current to be sent to the post-synaptic neuron, the current of each receptor is multiplied by the synaptic weight variable. This occurs in a line such as "AMPA_sum = change * d_mulSynFast[connId];".<br>
</details>
<br><br>
<b>Conductance to current computation</b>

![Equation with tau_d](http://uci-carl.github.io/CARLsim4/form_53.png)
Fig. 2. Equation with tau_d (CARLsim COBA).<br><br>
Synaptic currents are have their conductance computed with the use of the tau_d variable. Fig 2. shows how tau_d (tau in the equation) contributes to the conductance variable, g. The potential for conductance is processed every timestep.<br>
<br>
Issues: while this equation is present in the CARLsim user guide, it does not appear implemented in the code and it appears to have mathematical issues as described in the programming: unknown section below.
<details>
<summary>Programming: Conductance computation</summary> 
Potentially this equation is not implemented in the source code. In snn_gpu_module.cu, in the function updateNeuronState() the line:<br>
I_sum = -(runtimeDataGPU.gAMPA[nid] * (v - 0.0f)...<br>
appears to be where all synapse current is summed from each receptor type. Equation 13 from fig. 2. does not appear present in the code. As described in the unknown section below there seems to be mathematical issues with the equation too.
<br>
Unknown:<br>
In the equation, exp() is affected by time since last spike, yet the heaviside function causes the exp() to only be a factor if time since last spike is 0. This causes g to be +1 for each spike and 0 with no spikes. Is it correct that with no spikes the conductance fully stop? This does not seem to make sense because the synaptic current is meant to decay at a rate related to tau_d not just be instantly gone in a timestep with no spikes because the conductance becomes 0.<br>
</details><br>
CARLsim translates STP variables into synaptic current through the use of formulas in fig. 3.

![Equation with tau_d](http://uci-carl.github.io/CARLsim4/form_52.png)
Fig. 3. Conductance to current (CARLsim COBA).<br><br>
STP variables are used to translate a synapse occurence into current in a post-synaptic neuron through the equations:<br>
g = wt * stp_g * A * u_ * x+<br>
I = g * e_syn<br>
where wt is the synaptic weight for the pre- and post-neuron pair. e_syn = (v - v_rev_receptor) or a variation on that, see (CARLsim COBA) for more info.<br>
On each timestep after the synaptic spike, g is decayed with the equation:<br>
d(g)/d(t) = -g/tau_d<br>
Computed as: g = g * (1 - (1 / tau_d))<br>
It can be noted that the processing of g is similar to what is decribed for I in fig 1. eq. 3. While the source scholarpedia article describes eq. 3 as processing I, perhaps an extension of that equation is what is used in CARLsim where g is updated instead of directly computing I. For g to become I, the equation I = g * e_syn must be used.<br>
The reason the derivative of g is computed as g * (1 - (1 / tau_d)) is that is equvalent to g + (-g / tau_d).<br><br>
<b>Variable updates</b><br>
Every timestep the STP variables are updated. As described in fig. 1's equations, certain parts of the equations are included only during the timestep a spike occurs. The other parts of the equations are updated over every timestep.<br>
<details>
<summary>Programming: Variable updates</summary>
The code for updating variables is seperated based on if it updates each timestep or only when a spike occurs. The right part of the left side of the equations that is spike dependent has its code located in the kernel_conductanceUpdate() method. The left part of the right side of the equations updates every timestep has its code located in kernel_STPUpdateAndDecayConductances().<br>
</details>
<br>
<b>Variable indices</b><br>
An important part of calculations in a network as opposed to single synapses is assinging to which neurons each variable belongs. This is variable indexing where the indices are neuons. Each variable can belong to either a presynaptic neuron (preId), post synaptic neuron (postId), preId+postId pair at an individual synapse level (synId), or preId+postId pair at a group level (connId). This information was not included in the STP formulas due it seems to be from the formulas only representing individual [pre,post] pair connections and not groups of different [pre,post] connections.<br>
<br>
In STP, when spikes occur changes happen to presynaptic neuron axon terminal chemical balances (Scholarpedia STP). For instance, in short-term depression (STD) neurotransmitters are depleted at the axon. In short-term facilitation (STF), an influx of calcium occurs in the axon. In the synapse model, STD and STP are managed by u and x variables. The u and x variables are assigned to a [pre,post] individual synapse level index. Each axon terminal for a synapse has its own supply of neurotransmitter and calcium resources that are docked to the terminal. Therefore, the u and x variables belong to each [pre,post] pair represented by the axon terminal, instead of the u and x variables only belonging to and being indexed by the presynaptic neuron.<br>
<br>
The A variable is the synaptic response amplitude or weight (see programming: STP formulas: unknown section for more info). The A variable is assigned to a pair of preId and postId at the individual synapse level. This represents that each [preId,postId] can have its own synaptic response amplitude. I (current) and g (conductance) is assigned to a pair of preId and postId at the individual synapse level. This means each synapse has its own unique conductance and current level. I (current) will eventually be added to a post-synaptic neuron at which point that value is assigned to a single post-synaptic neuron and not a synapse. Tau_d (aka tau_s in fig. 1) is assigned to [preId,postId] at the group level. Neuron types can be represented in CARLsim using neuron groups. Tau_d being at the group level means that it has a specific value for a neuron type pair. Each of the other TM model constants, i.e., tau_u, tau_x, U, and g, are indexed at the preId+postId group level for the same reason.<br>
<br>
<b>Synapse signal decay</b><br>
More details on computing signal decay is here: [link](https://hco-dev-docs.readthedocs.io/en/latest/csdevdocs/synaptic_signal_decay.html) .<br><br>
<b>CSTP and non-CSTP results differences</b><br>
In simulations with STP and without CSTP it has been observed that there was a bug in non-CSTP processing of STP. A bug fix has been sent to the CARLsim team. References: https://github.com/UCI-CARL/CARLsim6/issues/16, https://github.com/UCI-CARL/CARLsim6/pull/17.<br>

<br>References:<br>
CARLsim COBA, http://uci-carl.github.io/CARLsim4/ch3_neurons_synapses_groups.html<br>
CARLsim Gain Factors, http://uci-carl.github.io/CARLsim4/ch4_connections.html#ch4s1s4_receptor_gain<br>
CARLsim STP, http://uci-carl.github.io/CARLsim4/classCARLsim.html#a54170c96807d6cdf699fa3664f81505e<br>
Izhikevich, 2003, https://ieeexplore.ieee.org/abstract/document/1257420/<br>
Moradi, 2022, https://www.nature.com/articles/s42003-022-03329-5
Scholarpedia STP, http://www.scholarpedia.org/article/Short-term_synaptic_plasticity<br>
Tsodyks, Pawelzik, Markram, 1998, https://direct.mit.edu/neco/article-abstract/10/4/821/6161/Neural-Networks-with-Dynamic-Synapses?redirectedFrom=fulltext<br>