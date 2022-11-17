CARLsim Developer Documentation - CSTP
=============================

This documentation will explain methods used in developing CSTP code for CARLsim.

Methods used in programming CSTP
----------------

<details>
<summary>Programming Methods</summary>
First occurence of each presynaptic spike:<br>
File: snn_gpu_module.cu<br>
Method: kernel_conductanceUpdate()<br>
<br>
Each neuron and the presynaptic neurons that connect to it are looped through
and saved in postNId and preNId variables.<br>
</details>