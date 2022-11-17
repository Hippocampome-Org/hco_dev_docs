CARLsim Developer Documentation - CSTP
=============================

This documentation will explain methods used in developing CSTP code for CARLsim.

Methods used in programming CSTP
----------------

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
Fig. 1's equation 1 is coded in the line: ```cpp
runtimeDataGPU.stpu[ind_plus] += runtimeDataGPU.stp_U[pos] * (1.0f - runtimeDataGPU.stpu[ind_minus]);
```
</details>
<br>
References:<br>
[1] http://uci-carl.github.io/CARLsim4/classCARLsim.html#a54170c96807d6cdf699fa3664f81505e