CARLsim6 Installation
=====================

This documentation provides further information about installing a Hippocampome branch of CARLsim6.

## GMU Supercomputer Installation

These instructions will cover methods to install a Hippocampome branch of CARLsim6 on the GMU Hopper supercomputer.

A Hippocampome branch of CARLsim6 should be downloaded. For this documentation, the ca3net_nm4cstp version of the Hippocampome branch can be downloaded. The branch should be downloaded from [here](https://github.com/nmsutton/CARLsim6/tree/feat/ca3net_nm4cstp). On that branch page, one should click the "Code" button and select "Download ZIP". One should not enter the command `git clone https://github.com/nmsutton/CARLsim6.git` because that will download the wrong branch. If one needs to clone the branch with git then one needs to specify the "feat/ca3net_nm4cstp" branch name in that clone command to retrieve the right branch ([reference](https://stackoverflow.com/questions/1911109/how-do-i-clone-a-specific-git-branch)).

It is recommended to extract the zip file in a "git" directory in one's home directory on the supercomputer. One will need to make that directory. A program such as FileZilla ([link](https://filezilla-project.org/)) can be used to transfer files to Hopper with the sftp protocol. Specifically, currently, the hopper.orc.gmu.edu server address should be used and one's GMU username and password should be used as login credentials. One may need to use two-factor authentication to login also. Currently, sftp can require two-factor authentication for each file transfer's initiation. This can be somewhat impractical.

One will also need to use ssh to access Hopper. One can do that from a command prompt, such as Windows' Powershell, or Linux's Gnome Terminal. The command `ssh gmu_username@hopper.orc.gmu.edu` should be used and gmu_username should be replaced with one's GMU username. One's GMU password is then entered. Two-factor authentication may also be needed here.

## Gencode

One will need to change the gencode information in CMakeLists.txt of the CARLsim6's base directory to one that fits with Hopper. As of April '25, this is:
<br>vim CMakeLists.txt and change "-gencode arch=compute_86,code=sm_86" to
"-gencode arch=compute_80,code=sm_80". Also "vim" is a text editor command.
<br>Compatibility with the gencode depends on what the GPUs in use on Hopper are.

## CMake

cmake needs to be installed. Specifically, version 3.22.0 is recommended to be used. Download "cmake-3.22.0-linux-x86_64.sh" from [here](https://cmake.org/files/v3.22/). Copy the file to the git folder. Make it executable:
<br>`chmod +x ./cmake-3.22.0-linux-x86_64.sh`
<br>Install with the command:
<br>`./cmake-3.22.0-linux-x86_64.sh --prefix=/home/gmu_username/cmake-3.22`
<br>where `gmu_username` is one's GMU username.
<br>use full path when running cmake. E.g,:
<br>/home/gmu_username/cmake-3.22/bin/cmake

## Google Test Suite

Download Google test, and version 1.10.0 is recommended. Download the zip file from [here](https://github.com/google/googletest/archive/refs/tags/release-1.10.0.zip) [source](https://github.com/google/googletest/releases). Upload the zip file to the "git" directory and extract it there. Navigate to the base directory for Google test's source code. Make the directory "build" in the googletest-release-1.10.0 base directory. Enter the "build" directory. Switch to the GPU node ([reference](https://wiki.orc.gmu.edu/mkdocs/Running_GPU_Jobs/)) with:
<br>`salloc -p gpuq -q gpu --nodes=1 --ntasks-per-node=1 --gres=gpu:1g.10gb:1 -t 0-01:00:00`
<br>Note this GPU node session will have a 1 hour time limit.
<br>Install with commands:
<br>`/home/gmu_username/cmake-3.22/bin/cmake \
    -DCMAKE_INSTALL_PREFIX=/home/gmu_username/gtest-1.10 \
    /home/gmu_username/git/googletest-release-1.10.0`
<br>where gmu_username is one's GMU username.
<br>`make`
<br>`make install`

In one's home directory the following should be set in the file ".bashrc":
<br>`export GTEST_LIBRARY=/home/gmu_username/gtest_1.10/lib/libgtest.a`
<br>`export GTEST_MAIN_LIBRARY=/home/gmu_username/gtest_1.10/lib/libgtest_main.a`
<br>`export GTEST_ROOT=/home/gmu_username/gtest_1.10/`

## Compile CARLsim6

Go to the base directory of CARLsim6's source code. Run these commands:
<br>`mkdir .build`
<br>`cd .build`
<br>switch to gpu node (if not still on it):
<br>`salloc -p gpuq -q gpu --nodes=1 --ntasks-per-node=1 --gres=gpu:1g.10gb:1 -t 0-01:00:00`
<br>The code included in this CARLsim6 branch will need the C++ Boost library and therefore it should be loaded with:
<br>`module load boost/1.73.0`
<br>`export CPATH=/opt/ohpc/pub/libs/gnu9/mpich/boost/1.73.0/include:${CPATH:+:${CPATH}}`
<br>`/home/gmu_username/cmake-3.22/bin/cmake \`
<br>`-DCMAKE_INSTALL_PREFIX=/home/gmu_username/CARLsim6 \`
<br>`-DCARLSIM_NO_CUDA=OFF \`
<br>`/home/gmu_username/git/CARLsim6`
<br>`make all`
<br>`make install`

Test install by trying to run hello_world at /.build/projects/hello_world/hello_world . This may not run correctly if it is just a hello_world version not compatible with this CARLsim6 branch, and this may be fine. Oblomem projects may run correctly even if hello_world does not run.

## General Hopper Commands

Some general commands are:
<br>submit job to queue:
<br>`sbatch ./slurm_wrapper.sh`
<br>view jobs:
<br>`squeue -u gmu_username`
<br>`/home/gmu_username/view_jobs.sh` (when view_jobs.sh a script with the view jobs command)
<br>cancel job
<br>`scancel <jobid>`
<br>where `<jobid>` is the id found from `squeue -u gmu_username`
<br>load module:
<br>`module load boost/1.73.0`
<br>search for module:
<br>`module spider boost`
<br>example slurm_wrapper.sh
<br>`#!/bin/bash`
<br>`#SBATCH --partition=gpuq`
<br>`#SBATCH --qos=gpu`
<br>`#SBATCH --gres=gpu:A100.80gb:1`
<br>`#SBATCH --ntasks-per-node=1`
<br>`#SBATCH --job-name="ca3full1"`
<br>`#SBATCH --time=0-3:00:00`
<br>`#SBATCH --output /scratch/gmu_username/ca3full1.txt`
<br>`#SBATCH --mem=120G`
<br>`srun ./ca3_snn_GPU`

`srun ./ca3_snn_GPU` can be replaced with `./rebuild.sh` when a rebuild script is made.
<br>Example `rebuild.sh`:
<br>`make clean && make && compiled_project_name`
<br>It should be noted that the software will need to be rebuilt with `make clean` and `make` anytime the project source code is updated. Therefore, it can be helpful to have a `rebuild.sh` script that automates this process anytime the compiled program is to be run.
<br>It should be also noted that the first time a project is run may require one to manually run the `make` command. This `rebuild.sh` script will exit on `make clean` if it does not find an already compiled file.
<br>The `rebuild.sh` script will also need to be made executable with `chmod +x ./rebuild.sh`

## Example .bashrc File

An example .bashrc file has been provided [here](https://hco-dev-docs.readthedocs.io/en/latest/oblomem/example_bashrc.html). One may not have a .bashrc file present by default. In that case, one should copy the contents of the example .bashrc file and place it in their .bashrc file. One should replace gmu_username with their GMU username. If one already has a .bashrc file, then one should copy all lines below `${CLUSTER} == "hopper"` to `else` in their .bashrc file when `hopper` is detected as the system in use. This will add to the modules loaded and environment variables included when using Hopper. Some of these are nessisary, e.g., CUDA, for working with Oblomem.