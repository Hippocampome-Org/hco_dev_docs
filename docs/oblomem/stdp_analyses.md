STDP Analyses Methods
=====================

This documentation provides further information about methods that can be used to analyze STDP calculations.

## STDP Calculations

STDP in CARLsim is computed with the equations ([source](https://uci-carl.github.io/CARLsim6/ch5_synaptic_plasticity.html#ch5s2_spike_timing_dependent_plasticity)):
<br>delta_t is the spike time difference (post minus pre) between presynaptic and postsynaptic spikes in a pair of neurons.
<br>If delta_t > 0, then delta_w_pre_post = alpha_plus\*exp(-delta_t/tau_plus) else if delta_t <= 0, then delta_w_post_pre = alpha_minus\*exp(-delta_t/tau_minus). delta_w_pre_post is the change in weight due to pre before post spikes. delta_w_post_pre is the change in weight due to post before pre spikes.
<br>Total weight change in a connection between two neurons given a time period is delta_w_pre_post + delta_w_post_pre, where one neuron is the pre and the other is the post. This is a directional connection where the connection strength can be different if the pre and post switched roles. It is considered a different connection in the switched roles case.

CARLsim uses symmetric nearest neighbor STDP (Morrison et al. 2008). See Fig. 7 for a visual example of that.

## Reporting delta_t for Analyses

Online articles have described that saving data produced by CUDA code for debugging purposes can be done with print statements. The printed material can then be transferred ("redirected") to a log file (at least in Linux) using command output redirection. CUDA (as of April 2025) is limited in its abilities to directly write to output files within code that processes GPU commands and therefore the print statements are used instead of directly writing to an output file. Redirecting output is described in a section below and can be used instead of directly writing output to a log file within CUDA code.

Print statements in CUDA code can be used to report delta_t as CARLsim detects it in simulations. This can be used to analyze in detail the calculations that are used as a part of STDP. These statements being used in CUDA code assumes that the simulation will be run in GPU mode in order to run the CUDA code. The print statements can be put into the file snn_gpu_module.cu.

The print statements specifically are:
<br>Tracking delta_w_pre_post with the function updateLTP():
```
SynInfo synInfo = runtimeDataGPU.preSynapticIds[p];
uint32_t pre_id = GET_CONN_NEURON_ID(synInfo);
int post_id = nid;
if (simTime > 400 && simTime < 500 && pre_id >= 450 && pre_id < 600 && post_id < 150) {
	printf("t %d pre_before_post pre_id %d post_id %d stdp_tDiff %d\n",simTime,pre_id,post_id,stdp_tDiff);
}
```
<br>This tracking in the function updateLTP() uses a method (e.g., synInfo and GET_CONN_NEURON_ID) found in work on [nm4cstp](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/nm4cstp.html) to find pre_id. The simTime condition can be set to a time period. The pre_id and post_id conditions can also be customized. 
<br>These lines of code can go after the line `runtimeDataGPU.wtChange[p] += ...` for the `else` condition of `if (connectConfigsGPU[connId].WithESTDPtype == PKA_PLC_MOD)` for tracking STDP calculations with non-PKA-PLC neuromodulated STDP. The print statement can go inside the PKA_PLC_MOD condition for tracking that type of STDP.
<br>Tracking delta_w_post_pre with the function generatePostSynapticSpike():
```
int pre_id = preNId; int post_id = postNId;
if (simTime > 400 && simTime < 500 && pre_id >= 450 && pre_id < 600 && post_id >= 450 && post_id < 600) {
	printf("t %d post_before_pre pre_id %d post_id %d stdp_tDiff %d\n",simTime,pre_id,post_id,stdp_tDiff);
}
```
<br>These lines of code can go after the line `runtimeDataGPU.wtChange[pos] += ...` for the `else` condition of `if(connectConfigsGPU[connId].WithESTDPtype == PKA_PLC_MOD)` for tracking STDP calculations with non-PKA-PLC neuromodulated STDP. The print statement can go inside the PKA_PLC_MOD condition for tracking that type of STDP.

These forms of tracking will report when CARLsim detects a post spike in pre-before-post, or pre spikes in post-before-pre.

## Results Analyses

The script /general_analyses/stdp_analyses/spike_times_analyses.m in Oblomem source code ([link](https://github.com/Hippocampome-Org/oblomem)) can read in results from the print statements and report statistics from them.

This script reads input csv files with the format:
<center>
<table border=1>
	<tr><td>text</td><td>sim_time</td><td>pre_post_type</td><td>text</td><td>pre_id_number</td><td>text</td><td>post_id_number</td><td>spike_time_difference</td></tr>
</table>
</center>

<br>The "text" columns are not used for analysis in this script. Text in the pre_post_type column is also not used in computing statistics, but it be useful to store info. in that column about that type for future reference.

For example:
<center>
<table border=1>
	<tr><td>text</td><td>sim_time</td><td>pre_post_type</td><td>text</td><td>pre_id_number</td><td>text</td><td>post_id_number</td><td>text</td><td>spike_time_difference</td></tr>
	<tr><td>t</td><td>2809</td><td>pre_before_post</td><td>pre_id</td><td>320</td><td>post_id</td><td>896</td><td>stdp_tDiff</td><td>389</td></tr>
</table>
</center>

## Redirecting Command Line Output

In Linux, command line output (containing results of print statements) can be saved to a file with the operator ">" after running a program. For example `./rebuild.sh > command_line_output.txt`. Example instructions are [here](https://askubuntu.com/questions/420981/how-do-i-save-terminal-output-to-a-file). Multiple advantages exist in redirecting output. Command line windows can have line display limits that can be avoided by saving to a log file. In other words, one can save as many results as wanted instead of being limited by a window line limit. Redirection also automatically saves output to a file and avoid some work in copying and pasting from a command line window in order to do that. 

Redirection can be run in an unattended mode. Without redirection, the line limits can may one want to halt the program if the line limit is exceed (attending to this condition). This is no longer an issue with redirection, and therefore one can just run the simulation and wait for it to complete. The results will automatically be saved.

A note is that the author of this document has only tried this process on his local computer. He expects that this process can also be created on a supercomputer, e.g., with a slurm script, if the line in the script that runs the simulation has a `> command_line_output.txt` after it. The log file should then be stored in the build directory for the simulation once it is run.

One can optionally alter the normal sim_timesteps to only be after the required time for reporting the data of interest. The simulation will then end and one will know the data is done collecting, instead of running the simulation longer than needed for the data to be reported.

## Comment Out Other Print Statements

When creating this reporting it can be useful to comment out other print statements, e.g., in the CARLsim main file that report statistics, that can interfere with having only the print statements of interest for these analyses.

## References

Morrison, A., Markus D., and Gerstner, W. (2008). Phenomenological models of synaptic plasticity based on spike timing. Biological Cybernetics 98: 459-478.