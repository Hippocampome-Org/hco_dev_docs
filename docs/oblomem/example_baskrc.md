Example .bashrc File
====================

This is an example .bashrc file for use on Hopper. gmu_username represents a GMU username. This .bashrc file was able to work with CARLsim6 on Hopper as of April 2025.

.bashrc file contents:
```
# load the proper set of modules based on the cluster
export CLUSTER=`sacctmgr -n  show cluster format=Cluster|xargs`
export CNODE=`hostname -s`

# Source global definitions
if [ -f /etc/bashrc ]; then
	        . /etc/bashrc
fi

if [ ${CLUSTER} == "argo" ]; then

	# User specific aliases and functions
	module load gcc/7.3.1
	module load cuda/10.1
	module load boost/1.67.0
	module load anaconda3/latest

	# CARLsim4 related
	export PATH=/cm/shared/apps/cuda/10.1/bin:$PATH
	export LD_LIBRARY_PATH=/cm/shared/apps/cuda/10.1/lib64:$LD_LIBRARY_PATH

	# CARLsim4 mandatory variables
	export CARLSIM4_INSTALL_DIR=/home/jkopsick/CARL_hc_08_25_21
	export CUDA_PATH=/cm/shared/apps/cuda/10.1

	export CARLSIM_CUDAVER=10
	export CUDA_MAJOR_NUM=7
	export CUDA_MINOR_NUM=0

	# CARLsim4 optional variables
	export CARLSIM_FASTMATH=0
	export CARLSIM_CUOPTLEVEL=3

	# load DGX-A100-01 (Hopper GPU Node) specific modules
elif [ ${CLUSTER} == "hopper" ] && [ ${CNODE} == "dgx-a100-01" ]; then

	module load hosts/dgx
	module load cuda/11.0.2-wf 
	module load gnu9/9.3.0
	module load openmpi/4.0.4-ev
	#module load boost/1.73.0-tf
	module load boost/1.73.0
	# module load cmake/3.19.5-be

	# CUDA
	#export PATH=/opt/sw/dgx-a100/apps/cuda/11.2.0/bin${PATH:+:${PATH}}
	#export LD_LIBRARY_PATH=/opt/sw/dgx-a100/apps/cuda/11.2.0/lib64:$LD_LIBRARY_PATH
	#export CPATH=/opt/sw/dgx-a100/apps/cuda/11.2.0/targets/x86_64-linux/include:/opt/ohpc/pub/libs/gnu9/mpich/boost/1.73.0/include:/usr/lib/x86_64-linux-gnu${CPATH:+:${CPATH}}

	#export LD_LIBRARY_PATH=/usr/local/lib:/home/gmu_username/git/CARLsim6/.build/:$LD_LIBRARY_PATH

	# CARLsim4
	export CARLSIM4_INSTALL_DIR=/home/gmu_username/CARL
	export CARLSIM_CUDAVER=11
	export CUDA_MAJOR_NUM=8
	export CUDA_MINOR_NUM=0
	export CUDA_PATH=/opt/sw/dgx-a100/apps/cuda/11.2.0

	# CARLsim4 optional variables
	#export CARLSIM_FASTMATH=0
	#export CARLSIM_CUOPTLEVEL=3

	# CARLsim6
	export PATH=/home/gmu_username/cmake-3.22/bin${PATH:+:${PATH}}
	export LD_LIBRARY_PATH=/home/gmu_username/gtest-1.10/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
	export LD_LIBRARY_PATH=/home/gmu_username/CARLsim6/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

	export PATH=/opt/sw/dgx-a100/apps/cuda/11.2.0/bin${PATH:+:${PATH}}
	export LD_LIBRARY_PATH=/opt/sw/dgx-a100/apps/cuda/11.2.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

	# Boost
	export CPATH=/opt/ohpc/pub/libs/gnu9/mpich/boost/1.73.0/include:${CPATH:+:${CPATH}}

	# GTest
	export GTEST_LIBRARY=/home/gmu_username/gtest-1.10/lib/libgtest.a
	export GTEST_MAIN_LIBRARY=/home/gmu_username/gtest-1.10/lib/libgtest_main.a
	export GTEST_ROOT=/home/gmu_username/gtest-1.10/

else
	module load hosts/hopper
	module load boost/1.73.0	
	module load gnu9/9.3.0
	#module load boost/1.73.0-tf

	export GTEST_LIBRARY=/home/gmu_username/gtest-1.10/lib/libgtest.a
	export GTEST_MAIN_LIBRARY=/home/gmu_username/gtest-1.10/lib/libgtest_main.a
	export GTEST_ROOT=/home/gmu_username/gtest-1.10/

	# CARLsim4
	export CUDA_PATH=/opt/sw/dgx-a100/apps/cuda/11.2.0

	export CPATH=/opt/ohpc/pub/libs/gnu9/mpich/boost/1.73.0/include:/usr/lib/x86_64-linux-gnu
fi
```