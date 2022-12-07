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
In snn_gpu_module.cu's kernel_conductanceUpdate(), the synaptic conductance that occurs from a synaptic spike is stored with functions such as setAMPASynGValue() for each receptor. This function stores the value in arrays such as AMPA_syn_g. The function computes the memory address to assign the value in the array using pitched memory. Nvidia recommends using pitched memory for 2D arrays in CUDA [source](https://docs.nvidia.com/cuda/cuda-runtime-api/group__CUDART__MEMORY.html) . The arrays in this case are 2D with one dimesion being the post-synaptic neuron id and the other dimension being an index of the pre-synaptic neuron relative to the total pre neurons given the post neuron. Pitched memory has the advantage of better coordinating parallel processing of memory access to achive high performance. A note about pitched memory is that syn_gPitch's value is automatically assigned by the CUDA pitch memory code.<br>
<br>
The inputs to the functions are post-synaptic neuron id and a pre-synaptic neuron index. This index represents which numbered pre connection the pre-synaptic neuron id has among a post-synaptic neuron id's synapses. This is numbered sequentially from low to high in neuron group numbers that the pre-synaptic neurons are in and low to high in neuron id number within each group. It should be noted the index is not the id, but instead another way to count pres given a post. <br>
</details><br>
<details>
<summary>Extra: additional pitched memory details</summary>
The 2D array referred to in pitched memory is not actually a 2D array but instead a 1D array with storage in the array that accomidates a 2D indexing calculation. This calculation takes a column and row index and finds the appropriate index in the array. In this case of synapse conductances, the row indices are post-synaptic neuron ids, and the column indices are pre-synaptic neuron index (not pre id). The "pitch" is set based on the max number of columns, which in this case is the max number of pre neurons given a post. The pitch may be automatically adjusted to a greater value than that max column number, to accomidate parallel processing memory positioning.<br>
<br>
Therefore, at a minimum, the memory allocated for each receptor's synapse current is an array at least the size of total_neurons * max_pres_per_post along with the size of a float datatype for each array index. This may be of note for resource management in large networks and the arrays being the size of total_neurons * pitch can be bigger than that minimum.<br>
<br>
The trade off for pitched memory is that it has faster memory access but takes more memory allocation. An array that is simply the number of synapses in the simulation would be more compact storage. It is possible these arrays allocate the most memory out of any array in CARLsim. If memory resources became and issue, using smaller arrays that cause slower computation could be an option.<br>
</details><br>
The kernel_conductanceUpdate() function indentifies which synapses have had spikes using the I_set array. That array uses both pitched memory and bitwise shifts. Bitwise shifts cause shifts in variable value bit positions. Such shifts can be used to encode different information in a single variable value relative to a bit position. To find which pre-neuron index is in a spike, decoding the I_set array's indices is needed.<br>
<br>
The wtId variable finds the pre-synaptic neuron index with the use of a quick access synapse id table (sh_quickSynIdTable). wtId was originally created to track which synaptic weight value each synapse has, but can also be used as an index for the synapse-specific conductance arrays. In functions such as setAMPASynGValue(), atomicAdd() is used to avoid race conditions where multiple threads are attempting to access a variable's memory storage included in the function at the same time and a conflict occurs. Removing atomicAdd() seems to cause unwanted results.<br>
<br>
<details>
<summary>Extra: quick synaptic table programming</summary>
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
note how cnt in the calculated examples here match cnt in the example table above. The maximum size of cnt is 7, which matches the number of bits in each value in the example table. See [reference](https://en.wikipedia.org/wiki/Bit_numbering) for some further info. Somehow a loop involving cnt is used to decode which neuron position a spike has occured in and find wtId. <br>
<br>
The NUM_THREADS in snn_gpu_module.cu was defined as 128. Max size of a 7 bit integer, 1111111, is 127, but if 0 is considered a value, there are 128 values that can be represented in a 7 bit int. This is also found by 2^7=128. sh_quickSynIdTable is built through a loop (up to 256 indices) using "i + threadIdx.x" as its array index. In C++, a standard int has 16 bits. The author of this page guesses the number of synapse firing bits encoded in each int value in I_set is related to these bit values.<br>
<br>
For every post neuron, there are maxNumPreSynN/32.0f array indices stored in the I_set array for pre neurons. This indicates that each array index for a pre neuron contains a value that can represent the synapse firing bits for at least 32 synapses. This is understood given maxNumPreSynN representing the most pres that a post can have, and for enough [pre,post] indeces to exist given the memory allocation, each indexed value must represent at least 32 synapses with a total of maxNumPreSynN/32.0f per index.<br>
<br>
With the standard int size having 16 bits, and each bit per se representing one synapse firing bit with the bit shift operation described above, it is unclear how each int could encode up to 32 synapse firing bits. However, somehow perhaps each int represents more synapse firing bits than its total number of bits.<br>
<br>
Note: the author of this page is unsure how all the details work with the use of the bitwise shifting to find neuron position but finds the neuron position returned with wtId can be used to track what pre-synaptic neuron index had a synaptic spike. More details could be added to this documentation in the future if it is further understood.<br>
</details><br>
<details>
<summary>Extra: test that wtId = pre_synaptic_neuron_index</summary>
Confirming wtID is equal to the pre-synaptic neuron index can be done if wanted by creating a loop through all pre indices given a post and using GET_CONN_NEURON_ID() to ensure wtId == pre_neuron_index. For example:<br>
.. code-block:: cpp

	for (int j2 = 0; j2 < lmt; j2++) {
		synInfo2 = runtimeDataGPU.preSynapticIds[cum_pos + j2];
		preNId2 = GET_CONN_NEURON_ID(synInfo2);
		if (preNId == preNId2) {
			preIndex = j2;
			if (preIndex != wtId) {
				printf("mismatch found: post:%d preindex:%d wtId:%d\n",postNId,preIndex,wtId);
			}
		}
	}
</details><br>
The new spike's synaptic conductance is added to the total conductance of the relevant receptor at the timestep of the spike. This calculation of the synapse's conductance adding to the total conductance will change after this timestep because each timestep after that will have signal decay applied to it. The adding of the signal at the spike's timestep is in lines such as:<br>
runtimeDataGPU.gAMPA[postNId] += AMPA_sum;<br>
<br>
<b>Decay of the synaptic signal</b><br>
The function kernel_STPUpdateAndDecayConductances() is where the synaptic signal is decayed. Functions such as getNMDARSynGPtr() retrieve the conductance values for each synapse. Those are then decayed by stp_d variables in lines such as<br>
*ampa_ptr *= runtimeDataGPU.stp_dAMPA[lSId];<br>
Once the decay has been processed the conductance is then added to the total conductance for the receptor in lines such as:<br>
runtimeDataGPU.gAMPA[nid] += *ampa_ptr;<br>
<br>
Adding each synapse's conductance after it has decayed to the total receptor conductance is seperated into two steps. The first step is at j == 0. This is the first pre index, and at this step each gReceptor (e.g., gAMPA) is set to equal the conductance of the synapse at j == 0. This clears prior values of conductance so that the gReceptor conductance can be rebuilt by looping through all relevant pres. The second step is that for each successive pre (j index) gReceptor is set to += each synapse's conductance. This loop is processed untill the gReceptor conductance is fully rebuilt.<br><br>
<details>
<summary>Extra: reasoning behind clearing and rebuilding</summary>
The reason the clearing and rebuilding is done is that a simple += new_conductance will not suffice. The updated gReceptor conductance can either be increased by additional synapse conductance or decreased by synapse conductance decay. A += operation without clearing gReceptor would add on top of a prior gReceptor total conductance when in some cases it should be reduced. Another way to compute the change could be finding the change in each synapse's conductance since the last timestep and += that to gReceptor but that would require tracking synaptic conductance at more than one timestep at a time. This method of clearing and rebuilding avoids the need to track those values at two timesteps.<br>
</details><br><br>
<b>Open questions</b><br>
(1) gReceptor values are only affected by STP-based synapse conductances? It seems likely this is the case, but in the bux fix code the clearing and rebuilding method could delete any values from sources other than the synapse conductances. For instance, neuromodulation has not yet been integrated into the CSTP but if it is than perhaps this could affect conductances. If any other sources of values contribute to gReceptor values then possibly the way the bug fix code processes them in STPUpdateAndDecayConductances() should be adjusted for that.