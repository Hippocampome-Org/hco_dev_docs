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
In snn_gpu_module.cu's kernel_conductanceUpdate(), the synaptic conductance that occurs from a synaptic spike is stored with functions such as setAMPASynGValue() for each receptor. This function stores the value in arrays such as AMPA_syn_g. The function computes the memory address to assign the value in the array using pitched memory. Nvidia recommends using pitched memory for 2D arrays in CUDA [source](https://docs.nvidia.com/cuda/cuda-runtime-api/group__CUDART__MEMORY.html) . The arrays in this case are 2D with one dimesion being the pre-synaptic neuron index and the other dimension being the post-synaptic neuron index. Pitched memory has the advantage of better coordinating parallel processing of memory access to achive high performance. A note about pitched memory is that syn_gPitch's value is automatically assigned by the CUDA pitch memory code.<br>
<br>
The inputs to the functions are post-synaptic neuron id and a pre-synaptic neuron index. This index represents which numbered pre connection the pre-synaptic neuron id has among a post-synaptic neuron id's synapses. This is numbered sequentially from low to high in neuron group numbers that the pre-synaptic neurons are in and low to high in neuron id number within each group. It should be noted the index is not the id, but instead another way to count pres given a post. The kernel_conductanceUpdate() function indentifies which synapses have had spikes using the I_set array. That array uses both pitched memory and bitwise shifts. Bitwise shifts cause shifts in variable value bit positions. Such shifts can be used to encode different information in a single variable value relative to a bit position. To find which pre-neuron index is in a spike, decoding the I_set array's indices is needed.<br>
<br>
The wtId variable finds the pre-synaptic neuron index with the use of a quick access synapse id table (sh_quickSynIdTable). wtId was originally created to track which synaptic weight value each synapse has, but can also be used as an index for the synapse-specific conductance arrays. In functions such as setAMPASynGValue(), atomicAdd() is used to avoid race conditions where multiple threads are attempting to access a variable's memory storage included in the function at the same time and a conflict occurs. Removing atomicAdd() seems to cause unwanted results.<br>
<br>
<details>
<summary>Optional: quick synaptic table programming</summary>
A sequence of bits is used to denote firing of a synapse. The position of the fired neuron can be found with the use of the quickSynIdTable array. Values in I_set can be processed through a sh_quickSynIdTable array based on quickSynIdTableGPU array to detect the neuron's position. Bit shift operations are used in the array values. Examples of values, as seen in snn_gpu_module.cu's code comments are:<br>
index |   cnt<br>
0000000 | 0<br>
0000001 | 0<br>
0000010 | 1<br>
0100000 | 5<br>
0110000 | 4<br>
<br>
In the initQuickSynIdTable() function, a bitwise operator is used to create quickSynIdTable indices. This is the "x & 1" operation where x is "i >> cnt" and i is an index of a loop starting at 1 up to the size of the quickSynIdTable array. "i >> cnt" bitwise shifts i by cnt bits to the right. The "x & 1" operation produces a value that is 1 or 0, depending on the least significant bit of x [source](https://stackoverflow.com/questions/38922606/what-is-x-1-and-x-1) . "least significant" seems to mean the bit furthest to the right in the variable value. If the last bit (least significant bit) is 1, the result is 1, otherwise it is 0.<br>
<br>
Therefore, as the value examples above show,<br>
(0000001 >> 1) & 1 = 0<br>
(0000010 >> 1) & 1 = 1<br>
(0100000 >> 5) & 1 = 1<br>
(0110000 >> 4) & 1 = 1<br>
note how cnt in the calculated examples here match cnt in the example table above. The maximum size of cnt is 7, which matches the number of bits in each value in the example table. See [reference](https://en.wikipedia.org/wiki/Bit_numbering) for some further info. Somehow a loop involving cnt is used to decode which neuron position a spike has occured in and find wtId. Note: the author of this page is unsure how all the details work with the use of the bitwise shifting to find neuron position but finds the neuron position returned with wtId can be used to track what pre-synaptic neuron index had a synaptic spike. More details could be added to this documentation in the future if it is further understood.<br>
</details><br>
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