

[TOC]

# Architecture



## Scripts folder structure

The TCL scripts are located in the scrpts_config folder folllowing the convention described below:

<img src="images/scritps_folder_structure.png" alt="image-20240608225620702" style="zoom:50%;" />

### tccommon_lib folder

This folder contains all scripts ececuted during the run of a test-case. All these scripts are called directly on only from the test-cases script.

### scripts_lib

There are three main scripts in this folder - the simu library script, the regression script and the utils library.

The tclreadline2.tcl script provides the user friendly experince remebering the history of the executed commands.

The compile_xilinx_libs.tcl script is used to compile Xilinx libraries, but the same functionality can be done manually from the Vivado tool GUI.

#### auto_gen

In this folder is sitting cmd_line_options.tcl which is automatically generated and should not be changed manually becuse it will be overwritten. The script is generated from the config_cmd_line_option_defailt.tcl and in some cases takes into account with higher precedence the CLI passesd arguments when the simu library is used to run test-case or regression simulations.
