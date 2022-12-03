CARLsim Developer Documentation - Synaptic Signal Decay
=============================

This documentation will explain methods used in programming synaptic signal decay in the synapse connection specific short term plasticity (CSTP) model in CARLsim.

Methods used in implementing Synaptic Signal Decay
----------------
<br>
<b>Printing variables for debugging</b><br>
Flags can be set with the variables listed with "PRINT_SYNAPSE_SPIKE" in snn_gpu_module.cu to print variable values for debugging if wanted<br>
<br>
<b>Storing synaptic connection conductance after a synaptic spike</b><br>
GPU version:<br>
In snn_gpu_module.cu's kernel_conductanceUpdate(), the synaptic conductance that occurs from a synaptic spike is stored with functions such as setAMPASynGValue() for each receptor. This function stores the value in arrays such as AMPA_syn_g. The function computes the memory address to assign the value in the array using pitched memory. Nvidia recommends using pitched memory for 2D arrays in CUDA [source](http://horacio9573.no-ip.org/cuda/group__CUDART__MEMORY_g80d689bc903792f906e49be4a0b6d8db.html). The arrays in this case are 2D with one dimesion being the pre-synaptic neuron index and the other dimension being the post-synaptic neuron index. Pitched memory has the advantage of better coordinating parallel processing of memory access to achive high performance. A note about pitched memory is that syn_gPitch's value is automatically assigned by the CUDA pitch memory code.<br>
<br>
The inputs to the functions are post-synaptic neuron id and a pre-synaptic neuron index. This index represents which numbered pre connection the pre-synaptic neuron id has among a post-synaptic neuron id's synapses. This is numbered sequentially from low to high in neuron group numbers that the pre-synaptic neurons are in and low to high in neuron id number within each group. It should be noted the index is not the id, but instead another way to count pres given a post. The kernel_conductanceUpdate() function indentifies which synapses have had spikes using the I_set array. That array uses both pitched memory and bitwise shifts. Bitwise shifts cause shifts in variable value bit positions. Such shifts can be used to encode different information in a single variable value relative to a bit position. To find which pre-neuron index is in a spike, decoding the I_set array's indices is needed.<br>
<br>
The wtId variable finds the pre-synaptic neuron index with the use of a quick access synapse id table (sh_quickSynIdTable). wtId was originally created to track which synaptic weight value each synapse has, but can also be used as an index for the synapse-specific conductance arrays. In functions such as setAMPASynGValue(), atomicAdd() is used to avoid race conditions where multiple threads are attempting to access a variable's memory storage included in the function at the same time and a conflict occurs. Removing atomicAdd() seems to cause unwanted results.<br>
<br>
<details>
<summary>Optional: test that wtId = pre_synaptic_neuron_index</summary>
Confirming wtID is equal to the pre-synaptic neuron index can be done if wanted by creating a loop through all pre indices given a post and using GET_CONN_NEURON_ID() to ensure wtId == pre_neuron_index. For example:<br>
for (int j2 = 0; j2 < lmt; j2++) {<br>
	synInfo2 = runtimeDataGPU.preSynapticIds[cum_pos + j2];<br>
	preNId2 = GET_CONN_NEURON_ID(synInfo2);<br>
	if (preNId == preNId2) {<br>
		preIndex = j2;<br>
		if (preIndex != wtId) {<br>
			printf("mismatch found: post:%d preindex:%d wtId:%d\n",postNId,preIndex,wtId);<br>
		}<br>
	}<br>
}<br>
</details><br>
The signal value that the synaptic spike's conductance is added to the total conductance for the relevant receptor at the timestep of the spike. This calculation of the synapse's conductance added to the total conductance will change after this timestep because each timestep after that of the spike will have signal decay applied to it. The adding of the signal at the spike's timestep is in lines such as:<br>
runtimeDataGPU.gAMPA[postNId] += AMPA_sum;<br>
<br>
<b>Decay of the synaptic signal</b><br>
The function kernel_STPUpdateAndDecayConductances() is where the synaptic signal is decayed. Functions such as getNMDARSynGPtr() retrieve the conductance values for each synapse. Those are then decayed by stp_d variables in lines such as<br>
*ampa_ptr *= runtimeDataGPU.stp_dAMPA[lSId];<br>
Once the decay has been processed the conductance is then added to the total conductance for the receptor in lines such as:<br>
runtimeDataGPU.gAMPA[nid] += *ampa_ptr;<br>