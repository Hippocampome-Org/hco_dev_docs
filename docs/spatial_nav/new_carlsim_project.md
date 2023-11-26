New CARLsim project creation
============================

These instructions are for creating a new CARLsim project.

## Install script

To be able to add a new project to CARLsim6 the init_cs6_local.sh script can be run.
This is found in the /scripts/new_proj directory. CARLsim6, as of the time of this writing, does not have a script to automate creating a new project that still works. There was an older script for that but it does not work in the latest version. This script can provide the action of creating a new project but it does not have programming to a point where it checks for preexisting projects of the same name, etc. Therefore it has not yet been added to the main CARLsim code. If one runs it with an issue, e.g., tries to create a project name that already exists, the user must correct the issues that result from that manually.

Running the script is performed by:
<br>$ init_cs6_local.sh \<project_name\> \<cs6_project_folder\> \<cs6_build_folder\>.
<br>One should enter the new project name as project_name and include the project and build folders for those variables, e.g., "/home/user/software/CARLsim6/projects". The script will add the project to CARLsim's cmake files and test run the project. It copies the hello_world project code into the new project for initial code. If a user wants to install a new project on a supercomputer the script init_cs6_supcomp.sh is availible.