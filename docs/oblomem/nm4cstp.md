nm4cstp Development Documentation
=================================

This documentation provides development documentation about adding CARLsim6's short term plasticity (STP) neuromodulation to the Hippocampome (HCO) branch of CARLsim. This code was created for the Oblomem neuromodulation research. This feature allows STP neuromodulation to occur with HCO's connection-specific STP (CSTP), which is STP on a neuro group pairs basis (e.g., paired neuron types as groups). This feature addition is named nm4cstp after the 4 neuromodulators in CARLsim (NM4) combined with CSTP. However, this version of nm4cstp only offers group level, in contrast to paired group level, STP modulation. How this combines with CSTP will be described later.

## Usage

The standard methods of STP-based neuromodulation are implemented with some specific conditions. More details about the standard STP-based neuromodulation is available [here](https://uci-carl.github.io/CARLsim6/ch20_neuromodulation.html#ch20s5_nm4stp).

The specific conditions present, and how STP modulation combines with CSTP are:
<br>STP is set in a neuron type pair basis and STP modulation is currently set on a individual neuron type basis. The presynaptic neuron type (neuron group) is the one that supplies neuromodulation in STP between neuron type pairs (a presynaptic neuron group and post synaptic group). For instance, setting neuromodulation levels for MEC LII Stellate cells will effect all STP connections that MEC LII Stellate cells has a presynaptic neuron in. Connections where the Stellate cells are postsynaptic neurons will not be affected by the Stellate cells’ neuromodulation settings unless the Stellate cells are also the presynaptic neurons in the connections.

Another condition is that code has only been added to run nm4cstp in GPU mode. Further programming would need to be added to enable this during CPU mode.

Example usage:
```
sim.setNeuromodulator(MEC_LII_Stellate, 
    1.0f, 1.0f, 1.0f, false, // DA
    1.0f, 1.0f, 1.0f, false, // 5HT
    1.0f, 1000000.0f, 0.000001f, true, // ACh
    1.0f, 1000000.0f, 0.000001f, true); // NE

float u[] = { 0.0f, 0.0f, 10.0f, 0.0f, 0.30f / 0.15f, 1.0f };
float tau_u[] = { 0.0f, 0.0f, 0.99f, 0.0f, -700.0f / 750.0f, 1.0f };
float tau_x[] = { 0.0f, 0.0f, 0.0f, 0.0f, 700.0f / 50.0f, 1.0f };
sim.setNM4STP(MEC_LII_Stellate, u, tau_u, tau_x);
```

More details about the setNM4STP function is [here](https://uci-carl.github.io/CARLsim6/classCARLsim.html#aac61df1a82373e89549e608b78557b82)
<br>More details about the setNeuromodulator function is [here](https://uci-carl.github.io/CARLsim6/classCARLsim.html#a88198ca833c0d254d5e7d75d2c8ee4fb)

## Programming design

Code was added to snn_gpu_module.cu and /carlsim/CMakeLists.txt to enable this function. The code in CMakeLists.txt had the flag CARLSIM_CSTP_NM added. In snn_gpu_module.cu, code is included where if the CARLSIM_CSTP_NM flag is true then nm4cstp is enabled.

The code included for nm4cstp was based on that included in CARLsim6's STP neuromodulation feature. In CARLsim6's feature, the source code includes updating STP based on the U, tau_u, and tau_x constants in two different places. These values are refered to as constants here even though the value of them is changed dynamically. The STP equations have a variable u that is not to be confused with the constant in the equation. In the STP equations, U, tau_u, and tau_x, do not change and are treated as constants, and that is why they are refered to as constants here.

STP values are updated from the U constant in the kernel_conductanceUpdate function. The kernel_STPUpdateAndDecayConductances function updates STP with the tau_u and tau_x constant values. An approach was used to have the `config` object be based on the presynaptic neuron group in both of these functions. The effect this causes is to have the STP's neuromodulation effects be based on the presynaptic neuron group's neuromodulation constant values in each STP calculation between pairs of neuron groups. Specifically, setSTP() is based on pairs of neuron groups, and neuromodulation uses the presynaptic group within that pair to create neuromodulation effects on the STP produced by synapses in neurons between those groups.

The code:
```
SynInfo synInfo = runtimeDataGPU.preSynapticIds[lSId];
uint32_t  preNId = GET_CONN_NEURON_ID(synInfo);
short int preGrpId = runtimeDataGPU.grpIds[preNId];

auto config = groupConfigsGPU[preGrpId];
```
The following includes understandings from reverse engineering the source code, and is a best guess at how it works:
### Synapse IDs
Was added to kernel_STPUpdateAndDecayConductances to allow config to be based on the presynaptic neuron group. lSId abbreviates local synaptic id. This is coded by offset + j. offset is cumulativePre\[nid\] where nid is the neuron id currently being processed. The line `nid = (STATIC_LOAD_START(threadLoad) + threadIdx.x)` sets the nid to a neuron id based on GPU factors such as the current block, thread, and grid process numbers. The cumulativePre array returns an index of the cumulative number of synapse ids starting from the first synapse included for nid. Specifically, this index counts all presynaptic (pre) and postsynaptic (post) neuron pairs in synapses that occured for all numerically lower neuron ids than nid. If this is neuron id 2 then all synapses in neuron 0 and neuron 1 are added to this index before reaching the first index of neuron 2. `j` is the index of each neuron that synapses with nid. The object name cumulativePre indicates that nid is the postsynaptic neuron in the neuron pairs in the synapse. 

### Neuron IDs
The function GET_CONN_NEURON_ID provides the neuron id of the presynaptic neuron (preNId) given synInfo. This is used with the grpIds array to provide what group number (preGrpId) preNId exists in. Finally, the groupConfigsGPU array uses preGrpId to provide the config object that contains the neuromodulator constant values for the presynaptic neuron group.

### Constant Values
The lines `tau_u_inv = runtimeDataGPU.stp_tau_u_inv[lSId];` and `tau_x_inv = runtimeDataGPU.stp_tau_x_inv[lSId];` retreive tau_u and tau_x values based on the synapse id. This is currently unique to the HCO branch because these values are based on neuron group pairs. The main branch of CARLsim6 retrieves these values based on individual groups, not group pairs. These lines of code were programmed in this way to accomidate the HCO branch's CSTP.

In the CARLSIM_CSTP_NM section of kernel_STPUpdateAndDecayConductances, grpId is replaced with preGrpId with values such as grpDA. This is designed to use the presynaptic neuron's group id instead of that of the postsynaptic neuron.

### Testing and prior examples
Testing has been done to verify that the neuron groups and neuromodulator constant values are retrieved as wanted from this code. For example print statements such as 
`printf("t:%d pre:%d post:%d preGrpId:%d tauu_1:%f tauu_2:%f ACh:%f\n",t,preNId,nid,preGrpId,config.wstptauu[NM_NE + 1],config.wstptauu[NM_NE + 2],runtimeDataGPU.grpACh[preGrpId]);` helped check which neuron ids in synapses were retrieved. This print statement also checked what group ids were retrieved and what neuromodulation values where retrieved based on those group ids.

The preSynapticIds array has been observed to be used in a similar way in other files such as snn_manager.cpp. This reinforces that this is a correct way to implement the code added in this work. One can search for uses of preSynapticIds in other files to similarly reverse engineer how this code can be used, and used for the purposes of this work. There does not appear to be other developer documentation available to clarify how such code works, but the methods used in this work appear to work as intended based on testing and prior code examples.

## Further work

This code can be expanded on to include modulation based on neuron group pairs. For instance, given two neuron groups named MEC_LII_Stellate and MEC_LII_Basket. The neuromodulation levels set between MEC_LII_Stellate and MEC_LII_Basket compared to MEC_LII_Stellate with itself could be different given this expansion.