# 5. SIMU User Guide

Unless noted psecifically, in the examples below we will use SystemVerilog test-cases and Mentor's Questa&reg; HDL simulator calls. Other HDL languages and simulators will have very similar, almost identical calls and behaviour.

## 5.1 HDL Flow and Configuration

The cloned repo doesn't need any configuration to run the existing test-cases examples, except the availability of the simulation tools and licenses.

If a vendor specific IPs or primitives are used, the HDL vendor libraries have to be compiled as well.

If new test-cases are created then some configurations have to changed accordingly, see details further below.

### 5.1.1 HDL Flow Paths Configuration

There are several paths that may be configured before the environment can be used. This is not required when using the provided examples. Otherwise when new folders are created or existing files or folders are moved or renamed then some changes will be required.

There are two places where the file/folder paths are configured:

```
dev/sim/scripts_config/...
dev/sim/testcases_env...
```

1. Paths in _dev/sim/scripts_config/..._ - SIMU central configuration place 
2. Paths in _dev/sim/testcases_env..._ - test-case specific configurations

The configuration options are variables inside the TCL files located at these locations.

### 5.1.2 HDL Command Line Options Default

The default command line options script located in:

```
dev/sim/scripts_config/config_cmd_line_options_default.tcl
```

This file contains the default options during CLI calls. During CLI call it is possible to override some configs defined in this file.

Here you can define/change many options, like:

1. simulator vendor
2. simulation optimization
3. coverage
4. waveform logging
5. host terminal/OS
6. compilation level
7. and other

NOTE: Not all configuration combinations are possible

### 5.1.3 HDL TB/TC Execution Flow and Configurations

It should be noted that there can be many test-benches and every test-bench can be called by many test-cases.

The test-case (TC) is executed from the run folder:

```
cd dev/sim/run
```

All test-bench(TB) & test-cases(TC) HDL and their TCL scripts are located in folders like:

```
dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium
dev/sim/testcases_envXilinx_hls_simXvitisXvivado
```

The folder name implies that the test-bench & test-cases are/using:

1. using Fidus IP's/DBL's (envFidus), AMD/Xilinx environment IP's (envXilinx), Altera/Intel IP's (envIntel), OSVVM environment IP's (envOsvvm)

2. written in SystemVerilog (\_sv\_), Verilog (\_v\_), VHDL (\_vhdl\_), HLS (\_hls\_)
3. simulators used are Mentor Questa (Mquesta), Xilinx Vivado XSim (Xvivado), Xilinx Vitis (Xvitis), Cadence Xcelium (Cxcelium)

In the test-cases(TC) folder it is possible to have another level of hierarchy to group similar test-cases by functionality, for example:

```
dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common
dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_axis_video
```

In the example above we have one set of several test-cases and test-benches in _tc_fidus_common_ folder and another set of test-cases and test-benches in _tc_fidus_axis_video_. 

This hierarchy structure allows to have many module level simulations and many top (also called chip level or device level) simulations with many test-cases each which is often needed to cover different functionality configurations.

Every TB file (for example _tb.sv_) has a dedicated _compile_all.tcl_ file compiling all TB, TC, BFMs and related RTL. If a different set of TB, TC, BFM, RTL HDL has to be compiled for a different module then a new folder is recommended to be created for that with a different _compile_all.tcl_ file and TB file. The folder looks like this:

```
/dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/compile_all.sv
/dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tb.sv
```

The _tb.sv_ contains the instance of the Device Under Test (DUT) also called Unit Under Test (UUT) which is the instance of the HDL/RTL module to be simulated. In addition to the DUT in the TB we instantiate the BFM Interfaces as well and connect them to the DUT ports, clocks and resets.

Located in the same folder, every TC file has a dedicated TCL script with identical name as the test-case file:

```
/dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.sv
/dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The TC is creating a dynamic instance of the BFM and is connecting the BFM to the already instantiate in TB the proper BFM Interface. After the dynamic BFM connection and activation is located the main body of the TC implementing the TC specific functionality.

After executing simulation a _results_folder_ is created to collect the output products from simulation run used for results aggregation processing in regression runs. Some simulation output products might reside in the run folder as well:

```
/dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/results_rtl
/dev/sim/run
```

### 5.1.4 HDL Regression Execution Flow and Configurations

The regression option can be controlled in two places:

1. Command line options file

   ```
   dev/sim/scripts_config/config_cmd_line_options_default.tcl
   ```

2. Regression configurations dedicated settings file

   ```
   dev/sim/scripts_config/config_settings_regression.tcl
   ```

​	This file defines the folders to be scanned in the automatic regression mode and in results aggregation post processing.

​	This file defines the TC's to be ignored in the automatic regression mode and in results aggregation post processing. 

The regression concept means that all test-cases are executed sequentially and at the end an aggregated pass/fail status will be reported. In addition aggregated test-coverage report is produced as well. 

There are two types of regressions available in SIMU - automatic regression and manual regression. Both options are available and can be un independently. In long run the automatic recession is going to be deprecated in the favour of the manual regression. Nevertheless in foreseeable future the automatic regression is the only one providing a complete regression aggregation of pass/fail and coverage and for that reason this is the recommended flow. The Manual regression option is a relatively new feature and yet doesn't provide proper multi-folder aggregation of pass/fail and coverage.

The automatic mode executes all TC by parsing the folders and executing all TCL files starting with _tc\_..._ prefix. In this case all TCs are execute in a TCL sorting predetermined sequence since the sorting is performed using the TCL language sorting defaults. This creates some inconvenience in controlling the execution order. 

The manual regression option gives full control over the execution sequence. 

Every regression call performs the following:

1. Executing all TCs in automatic fashion or in manual mode
2. Automatic TC regression scan and run
3. TC execution sequence control
4. Using a different random seed forcing randomization during every new run
5. Re-run regression with a single forced seed
6. Pass/Fail aggregation result
7. Code Coverage aggregation result - scans all _result_rtl_ folders and merges all coverages and reports the aggregate result

### 5.1.5 HDL Simulator Vendor Specific Configurations

Simulator vendor specific configurations are captured in the dedicated files, one per vendor, located in _dev/sim/scripts_config/_:

```
config_settings_activehdl.tcl   // ActiveHDL simulator configurations
config_settings_vsim.tcl.       // Mentor Questa/ModelSim configurations
config_settings_xcelium.tcl     // Cadence Xcelium configurations
config_settings_xsim.tcl        // Xilinx Vivado XSim configurations
```

All vendor specific calls are pre-configured in the files above. The proper vendor file is selected based on the vendor configurations in command line options file here:

```
dev/sim/scripts_config/config_cmd_line_options_default.tcl
```

All vendor files follow a similar structure having identical compile/simulate calls. The HLS config file, not shown above is very different compared to the rest of the HDL compile/simulate flows.

## 5.2 HLS Flow and Configuration

### 5.2.1 HLS Flow Paths Configuration

There are several paths that may be configured before the environment can be used. This is not required when using the provided examples. Otherwise when new folders are created or existing files or folders are moved or renamed then some sttings  changes to the settings will be required.

There are three places where the file/folder paths are configured:

```
dev/sim/scripts_config/...
dev/sim/testcases_env...
dev/builds/axiburst_write/...
```

1. Paths in _dev/sim/scripts_config/..._ - SIMU central configuration place 
2. Paths in _dev/sim/testcases_env..._ - test-case specific configurations. Here the simulation flow is configured to HLS.
3. Paths in _dev/builds/axiburst_write/..._ - HLS project specific configurations

The configuration options are variables inside the TCL files located at these locations.

### 5.2.2 HLS Command Line Options Default

The default command line options script located in:

```
dev/sim/scripts_config/config_cmd_line_options_default.tcl
```

This file contains the default options during CLI calls. During CLI call it is possible to override some configs defined in this file.

Here you can define/change many options related to the HLS flow, like:

1. TODO add

NOTE: The Simulation flow can't be changed here to HLS. Not all configuration combinations are possible

### 5.2.3 HLS TB/TC Execution Flow and Configurations

It should be noted that there can be many test-benches and every test-bench can be called by many test-cases.

The test-case (TC) is executed from the run folder:

```
cd dev/sim/run
```

All test-bench(TB) & test-cases(TC) HDL and their TCL scripts are located in folders like:

```
dev/sim/testcases_envFidus_sv_simMquestaXvivadoCxcelium
dev/sim/testcases_envXilinx_hls_simXvitisXvivado
```

The folder name implies that the test-bench & test-cases are/using:

1. using Fidus IP's/DBL's (envFidus), AMD/Xilinx environment IP's (envXilinx), Altera/Intel IP's (envIntel), OSVVM environment IP's (envOsvvm)

2. written in SystemVerilog (\_sv\_), Verilog (\_v\_), VHDL (\_vhdl\_), HLS (\_hls\_)
3. simulators used are Mentor Questa (Mquesta), Xilinx Vivado XSim (Xvivado), Xilinx Vitis (Xvitis), Cadence Xcelium (Cxcelium)

In the test-cases(TC) folder it is possible to have another level of hierarchy to group similar test-cases by functionality, for example:

```
dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write
dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_read
```

In the example above we have one set of several test-cases and test-benches in _tc_xilinx_axiburst_write_ folder and another set of test-cases and test-benches in _tc_xilinx_axiburst_read_. 

This hierarchy structure allows to have many module level simulations and many top (also called chip level or device level) simulations with many test-cases each which is often needed to cover different functionality configurations.

#### 5.2.3.1 HLS Flow Details

The HLS Vitis project is created from TCL sources in every run. If the Vitis project deleting option is disabled before the new run, the designer has to pay attention not to have conflicting residual configurations in the Vitis design from the previous run because the project creation scripts will overwrite some settings, but not all. For that reason the default is to delete the previous Vitis project before the new run and recreate the project from scratch from the TCL script.

After the run is completed, the project can be open in the Vitis_HLS GUI and the work can continue in the GUI. After that all changes have to be exported to the TCL and the CFG scripts. These two files contain all information needed to re-create the HLS project in Vitis-HLS.

There are two types but 3 scripts in total, located in the project scripts folder located in the _/dev/builds/scripts/axiburst_write_ folder

```
proj_create.cfg   <- contains all Vitis-HLS configuration options
proj_create.tcl   <- contains all commands and files needed to create RTL_HLS project (with the default dummy/empty TB HLS file )
proj_paths.tcl    <- all paths needing to be configured
```

When the HLS flow is executed, initially only the HLS synthesisable (RTL-HLS) code and dummy TB HLS files are used to create a Vitis_HLS project. Later the real TB&TC files are added to the Vitis_HLS project, replacing the dummy TB files. All C++ RTL-HLS files are located in _dev/sim/sources_hls_.

The Vitis-HLS project is created in _dev/builds/vitis/axi4_sqrt_ folder and can be open in the Vitis GUI. When the work is done it is important to export the project .CFG file and propagate the changes manually to the _project_create.cfg_ file to preserve that work. The same is true for the project creation TCL file _proj_create.tcl_. 

The already created HLS project can be open like this, note using _-classsic_ is going to be deprecated:

```
cd dev/builds/vitis/
vitis_hls -classic -p ./axi4_sqrt
```



Every TB&TC file (for example the C++ file _tc_01_xilinx_axiburst_write.cpp_ - here the TB is the TC, hence only one file) has a dedicated _tbtc_add.tcl_ file that adds all TB, TC, BFMs files to the already pre-existing RTL-HLS only Vitis project. If a different set of TB, TC, BFM, RTL HDL has to be compiled for a different module then a new folder is recommended to be created for that with a different _tbtc_add.tcl_ file and TB&TC files. It is also recommended to put a copy of the header .h/.hpp files of the HLS top level and the simulation management library. The folder looks like this:

```
/dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write/tbtc_add.tcl                     <- TB&TC add script
/dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write/tc_01_xilinx_axiburst_write.cpp  <- TB&TC
/dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write/axi4_sqrt.hpp                    <- HLS top
/dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write/sim_management_pkg.h             <- sim lib
```

The TB&TC contains the instance of the Device Under Test (DUT) also called Unit Under Test (UUT) which is the instance of the HLS top module to be simulated. In addition to the DUT in the TB&TC we instantiate the BFMs as well and connect them to the DUT.

Located in the same folder _/dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write/_, every TB&TC file has 5 dedicated TCL scripts with identical name as the test-case file but with 5 different suffixes to indicated the HLS flow stage:

```
tc_01_xilinx_axiburst_write_1cppsim.tcl     <- C++ simulation stage
tc_01_xilinx_axiburst_write_2csynth.tcl     <- HLS synthesis stage
tc_01_xilinx_axiburst_write_3cosim.tcl       <- HLS Co-simulation stage first runs the C++ HLS simulation and then the synthesized RTL simulation using Xilinx or other HDL simulator
tc_01_xilinx_axiburst_write_4export.tcl     <- HLS exporting stage where the Vivado IP is exported containing the synthesized HDL
tc_01_xilinx_axiburst_write_5implement.tcl  <- HLS implementation stage where the IP is running OOC P&R in Vivado and timing closure is reported
```

After executing simulation a _results_rtl_ is created to collect the output products from simulation run used for results aggregation processing in regression runs. Some simulation output products might reside in the run folder as well:

```
/dev/sim/testcases_envXilinx_hls_simXvitisXvivado/tc_xilinx_axiburst_write/results_rtl
/dev/sim/run
```

### 5.2.4 HLS Regression Execution Flow and Configurations

The regression option can be controlled in two places:

1. Command line options file

   ```
   dev/sim/scripts_config/config_cmd_line_options_default.tcl
   ```

2. Regression configurations dedicated settings file

   ```
   dev/sim/scripts_config/config_settings_regression.tcl
   ```

​	This file defines the folders to be scanned in the automatic regression mode and in results aggregation post processing.

​	This file defines the TC's to be ignored in the automatic regression mode and in results aggregation post processing. 

The regression concept means that all test-cases are executed sequentially and at the end an aggregated pass/fail status will be reported. In addition aggregated test-coverage report is produced as well. 

Only one type regression is available in SIMU to run HLS simulations - manual regression. The Automatic regression is not suited/supported to run HLS.

The manual regression option gives full control over the execution sequence. 

Every regression call performs the following:

1. Executing all TCs in automatic fashion or in manual mode - not yet
2. Automatic TC regression scan and run - not yet
3. TC execution sequence control
4. Using a different random seed forcing randomization during every new run - not yet
5. Re-run regression with a single forced seed - not yet
6. Pass/Fail aggregation result - not yet
7. Code Coverage aggregation result - scans all _result_rtl_ folders and merges all coverages and reports the aggregate result - not yet

**NOTE: The regression option for HLS is not fully functional/completed yet.**

### 5.2.5 HLS Simulator Vendor Specific Configurations

Simulator vendor specific configurations are captured in the dedicated files, one per vendor, located in _dev/sim/scripts_config/_:

```
config_settings_xhls.tcl        // Xilinx Vitis configurations
```

All vendor specific calls are pre-configured in the files above. The proper vendor file is selected based on the vendor configurations in command line options file here:

```
dev/sim/scripts_config/config_cmd_line_options_default.tcl
```

All vendor files follow a similar structure having identical compile/simulate calls (only one for now but Mentor also has HLS compiles that can be added here). The HLS config file is quite different from the HDL config file since the HLS simulation flow is significantly different compared to the rest of the HDL compile/simulate flows.

## 5.3 Run Single Test-Case (TC) simulation

It has to be noted that 

1. All TCs have to be executed executed from the **run** folder > _cd symphony/dev/sim/run_
2. All TC names have to start with the **tc_** prefix. This is required by the regression automation and results parsing.
3. Every HDL TC is paired with an identical in name TCL script. Similarly every HLS TC is paired with an almost identical in name TCL scripts where the suffix might differ for the TCL file to indicate different type of HLS TC execution.
4. That TC TCL script is the one actually executed by the SIMU framework. 
5. The TC TCL script contains some required by SIMU information, such as several pre-configured paths, the test-case name, also one parameter for HLS TC type of execution.

There are several ways to run simulation of a single test-case. They achieve the same result, running a test-case simulation, but are invoked in different ways using:

a) Linux TCL interpreter using tclsh terminal  - suitable for all HDL vendors and for all HLS vendors

b) Simulator vendor TCL CLI in bash terminal - suitable only some HDL vendors

c) Simulator vendor TCL terminal in GUI - suitable only some HDL vendors

 * _The three use-cases above can be used with or without the SIMU shell_ 
 * _The three use-cases above can be used with the run_testcase function form the SIMU shell or by simply "sourcing" the test-case script_

### 5.3.1 Test-case simulation (for all HDL and all HLS vendors) - using Linux TCL interpreter

**(with/without SIMU shell, with/without SIMU _run_testcase_ simulation call, with/without SVTI in CLI mode, simulator vendor tool independent)**

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

There are several parameters that can be passed in CLI mode (passing CLI parameters is possible when using third party TCL like _tclsh_, but not all TCL interpreters support that, for example ModelSim/Quartus parameters passing is different and for this reason not supported by SIMU) TODO:

* PRAM = 1 or 0 - vendor TCL can be disabled or enabled if supported by the vendor
* add examples here

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> run_testcase ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

From he example above, the function _run_testcase_ is used only by the automated regression call. 

Additionally this function is a unified way to call a vendor simulator if it is hard to remember the vendor simulator specific call with its attributes.

It the example below it can be safely replaced by the TCL _source_ command.

```
$ cd symphony/dev/sim/run
$ tclsh 
> source ../testcases/tc_reset/tc_reset.tcl 
```

Alternatively, as seen above, it is also possible not to use the SIMU shell. In this case the user interface lacks history and it is not convenient for use and for that reason it is shown here for completeness, but it is not recommended to be used for not being user friendly.

TODO: add examples with parameters passing

### 5.3.2 Test-case simulation - using simulator vendor TCL interpreter (SVTCL)

**(from Linux bash or simulator GUI)**

The simulator vendor tools differ in a few ways:

a) all vendors support simulation tools calls (for example vmap, vcom, vsim, etc.) as bash or terminal commands 

b) some vendors have the simulation tools calls (for example vmap, vcom, vsim, etc.) integrated as TCL commands in their TCL interpreters 

c) all vendors have TCL GUI terminal

d) some vendors TCL interpreter supports CLI mode, others don't

e) some vendors have a separate license for waveform viewing which is way less expensive compared to the simulator license

#### ModelSim/QuestaSim TCL interpreter

**(with/without SIMU shell, with/without SIMU _run_testcase_ simulation call, from Linux bash or from simulator vendor GUI, can run on Windows OS)**

ModelSim/Questa TCL interpreter has the following specifics:

a) supports TCL CLI mode and GUI TCL terminal 

b) the simulation tool calls are available as bash terminal commands and are also available as commands in the TCL interpreter

c) there is a separate license for waveform viewing which is way less expensive compared to the simulator license

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

Below are captured several practically identical ways to execute a TC.

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

The example above is using the QuestaSim/Modelsim TCL interpreter in CLI mode invoked from Linux bash terminal or Windows terminal.

```
-- start the simulator vendor GUI
# vsim
-- the commands below are executed in the Modelsim/Questa GUI TCL terminal
> cd symphony/dev/sim/run
> source runme_simu_shell.tcl
> run_testcase ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The example above is using the SIMU shell _run_testcase_ function in the QuestaSim/Modelsim TCL interpreter with the SIMU shell in the simulator vendor GUI TCL terminal started from Linux or Windows terminal.

```
-- start the simulator vendor GUI
# vsim
-- the commands below are executed in the Modelsim/Questa GUI TCL terminal
> cd symphony/dev/sim/run
> source ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The example above is using the QuestaSim/Modelsim TCL interpreter in simulator vendor GUI TCL terminal started from Linux or Windows terminal.

```
$ cd symphony/dev/sim/run
$ tclsh 
> vsim -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit 
```

Alternatively, as seen above, it is also possible to use the SVTI inside the Linux tclsh interpreter. This is somewhat redundant using two TCL interpreters and not that user friendly.

#### Xilinx XSim TCL interpreter

Vivado TCL interpreter has the following specifics:

a) supports TCL CLI mode in Linux bash and GUI TCL terminal from Vivado 

b) the simulation tool calls are available as bash terminal commands and are also available as commands in the vendor TCL interpreter

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

TODO: add call examples

#### Xcelium TCL interpreter

Xcelium TCL interpreter has the following specifics:

a) It is not possible to call the VTCI in Linux bash terminal.  As a result, only the SimVision GUI TCL terminal is available.

b) The simulation tool calls are only available as bash terminal commands and are not available as commands in the TCL interpreter

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

```
-- start the simulator vendor GUI
# simvision
-- the commands below are executed in the SimVision GUI TCL terminal
> cd symphony/dev/sim/run
> source ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The example above is using the Xcelium TCL interpreter in simulator vendor GUI TCL terminal called Simvision.

#### ActiveHDL TCL interpreter

ActiveHDL TCL interpreter has the following specifics:

a) Supports TCL CLI mode in a vendor developed terminal application and GUI TCL terminal from ActiveHDL. It is not possible to call the SVTCL in Linux bash terminal. As a result the two vendor TCL interpreters differ in some aspects which require specific configurations in SIMU. 

b) The simulation tool calls are available as bash terminal commands and are also available as commands in the vendor TCL interpreter TODO: check this statement

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

TODO: add call examples, they should be similar to the Modelsim/Quartus calls.

#### VitisHLS TCL interpreter

VitisHLS TCL interpreter has the following specifics:

a) Older Vivado HLS did not have SVTCL in CLI or GUI variants in VitisHLS. As a result only the Linux _tclsh_ TCL interpreter should be used.

b) Vitis 2024 has SVTCL which can be invoked:

```
#vitis_hls -i
vitis_hls> exit
#
```







## 5.4 Run Regression Simulation

There are several ways to run a regression simulation of all test-cases. They achieve the practically the same result, running regression simulation, but have some subtle differences.

It has to be noted that a regression call can be executed in all different ways similar to the individual test-cases run execution types, as it was already covered:

a) Linux TCL interpreter using tclsh terminal

b) simulator vendor TCL CLI in bash terminal 

c) simulator vendor TCL terminal in GUI - can be used to run simulations on Windows OS

 * _The three use-cases above can be used with or without the SIMU shell_ 
 * _The three use-cases above can be used with the run_testcase function form the SIMU shell or by simply sourcing the test-case script_

### 5.4.1 Manual Regression Simulation (for all HDL vendors and all HLS vendors)

The manual regression uses a list of test-cases that is manually maintained. An example of that script can be found in _/dev/sim/regression_lists/REGRESSION_TC_LIST_MANUAL.tcl_

A manual regression can't run in multiple parallel threads.

The regression run progress can't be easily monitored.

#### 5.4.1.1 Manual Regression - using Linux TCL interpreter (for all HDL vendors and all HLS vendors)

**(with/without SIMU shell, with TCL _source_ call, simulator vendor tool independent)**

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

Passing CLI parameters is not supported when using manual regression.

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

#### 5.4.1.2 Manual Regression - using SVTCL

**(from Linux bash or simulator GUI)**

##### ModelSim/QuestaSim TCL interpreter

**(can run on Windows OS)**

ModelSim/Questa TCL interpreter has the following specifics:

a) Supports TCL CLI mode and GUI TCL terminal 

b) The simulation tool calls are available as bash terminal commands and are also available as commands in the TCL interpreter

Below are captured several practically identical ways to execute regression.

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

The example above uses the call to QuestaSim/Modelsim TCL terminal in CLI mode invoked from Linux bash shell or Windows terminal

```
-- start the simulator vendor GUI
# vsim
-- the commands below are executed in the Modelsim/Questa GUI TCL terminal
> cd symphony/dev/sim/run
> source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

The example above is using the QuestaSim/Modelsim TCL interpreter GUI terminal on Linux or Windows.

```
# cd symphony/dev/sim/run
# tclsh 
> vsim -c -do ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

Alternatively, as seen above, it is also possible to use the SVTI inside the Linux tclsh interpreter. This is somewhat redundant using two TCL interpreters and not user friendly.

##### Xilinx XSim TCL interpreter

Vivado TCL interpreter has the following specifics:

a) supports TCL CLI mode in Linux bash and GUI TCL terminal from Vivado 

b) the simulation tool calls are available as bash terminal commands and are also available as commands in the vendor TCL interpreter

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

TODO: add call examples

##### Xcelium TCL interpreter

Xcelium TCL interpreter has the following specifics:

a) It is not possible to call the VTCI in Linux bash terminal.  As a result, only the SimVision GUI TCL terminal is available.

b) The simulation tool calls are only available as bash terminal commands and are not available as commands in the TCL interpreter

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

```
-- start the simulator vendor GUI
# simvision
-- the commands below are executed in the SimVision GUI TCL terminal
> cd symphony/dev/sim/run
> source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

The example above is using the Xcelium TCL interpreter in simulator vendor GUI TCL terminal Simvision.

IMPORTANT: TODO:regression not tested yet in Linux _tclsh_ or Simvision GUI TCL , but it is expected to be functional.

##### ActiveHDL TCL interpreter

ActiveHDL TCL interpreter has the following specifics:

a) Supports TCL CLI mode in a vendor developed terminal application and GUI TCL terminal from ActiveHDL. It is not possible to call the SVTCL in Linux bash terminal. As a result the two vendor TCL interpreters differ in some aspects which require specific configurations in SIMU. 

b) The simulation tool calls are available as bash terminal commands and are also available as commands in the vendor TCL interpreter TODO: check this statement

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

TODO: add call examples, they should be similar to the Modelsim/Quartus calls.

##### VitisHLS TCL interpreter

VitisHLS TCL interpreter has the following specifics:

a) There is no SVTCL in CLI or GUI variants. As a result only the Linux _tclsh_ TCL interpreter should be used.

_TODO:investigate_using_VitisHLS_option_to_pass_a_TCL_file_for_execution._



### 5.4.2 Automatic Regression 

The Automatic Regression gets its name from the auto-generated test-cases list located in __/dev/sim/regression_lists/_. 

The auto-generated test-case list can be checked for regression progress information, which is a nice feature not available when running manual regression.

Another nice feature of the automated-regression is the option to run several threads in parallel. To accomplish this just open a few terminals and execute in each one a separate regression call. All regression calls will run in their own terminals and will use one auto-generated regression script to pick the next available test-case to run and to exchange information about the next available next test-cases to execute. This parallel execution makes the regression run much faster, but at the expense of using more simulator licenses, one per thread.

IMPORTANT: If the regression hangs, it is possible that the regression list has no free test-cases to be taken. A solution to this is to just delete the automatically generated regression list or edit it in a text editor to replace stale _"taken"_ status to _"available"_ status.

#### 5.4.2.1 Automatic Regression - using Linux TCL interpreter

**(with SIMU shell, with SIMU regression call, simulator vendor tool independent)**

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

There are several parameters that can be passed in CLI mode (passing CLI parameters is possible when using third party TCL like _tclsh_, but not all TCL interpreters support that, for example ModelSim/Quartus parameters passing is different and for this reason not supported by SIMU) TODO:

* PRAM = 1 or 0 - vendor TCL can be disabled or enabled if supported by the vendor
* add examples here

Both ways to call regression, as captured below,  are identical.

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../scripts_configure/run_regression.tcl
```

The call above is executing directly the regression script.

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> run_regression
```

Alternatively, using the SIMU library it is possible to call regression using the _run_regression_ function, as shown above.

TODO: add examples with parameters passing



#### 5.4.2.2 Automatic Regression - using SVTCL

**(from Linux bash or simulator GUI)**

##### ModelSim/QuestaSim TCL interpreter

**(can run on Windows OS)**

ModelSim/Questa TCL interpreter has the following specifics:

a) Supports TCL CLI mode and GUI TCL terminal 

b) The simulation tool calls are available as bash terminal commands and are also available as commands in the TCL interpreter

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../scripts_configure/run_regression.tcl -do exit
```

The example above uses the call to QuestaSim/Modelsim in CLI mode invoked from Linux bash shell.

```
# cd symphony/dev/sim/run
# source ../scripts_configure/run_regression.tcl
```

The example above is using the QuestaSim/Modelsim TCL interpreter GUI terminal. 

##### Xilinx XSim TCL interpreter

Vivado TCL interpreter has the following specifics:

a) Supports TCL CLI mode in Linux bash and GUI TCL terminal from Vivado 

b) The simulation tool calls are available as bash terminal commands and are also available as commands in the vendor TCL interpreter

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

TODO: add call examples

##### Xcelium TCL interpreter

Xcelium TCL interpreter has the following specifics:

a) It is not possible to call the SVTCL in Linux bash terminal.  As a result, only the SimVision GUI TCL terminal is available.

b) The simulation tool calls are only available as bash terminal commands and are not available as commands in the TCL interpreter

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

```
-- start the simulator vendor GUI
# simvision
-- the commands below are executed in the SimVision GUI TCL terminal
> cd symphony/dev/sim/run
> source ../scripts_configure/run_regression.tcl
```

The example above is using the Xcelium TCL interpreter in simulator vendor GUI TCL terminal Simvision.

IMPORTANT: TODO:regression not tested yet in Linux _tclsh_ or Simvision GUI TCL , but it is expected to be functional.

##### ActiveHDL TCL interpreter

ActiveHDL TCL interpreter has the following specifics:

a) Supports TCL CLI mode in a vendor developed terminal application and GUI TCL terminal from ActiveHDL. It is not possible to call the SVTCL in Linux bash terminal. As a result the two vendor TCL interpreters differ in some aspects which require specific configurations in SIMU. 

b) The simulation tool calls are available as bash terminal commands and are also available as commands in the vendor TCL interpreter TODO: check this statement

Specific settings have to be enabled in this mode TODO:

* param = 0 - disable vendor TCL interpreter 

TODO: add call examples, they should be similar to the Modelsim/Quartus calls.

##### VitisHLS TCL interpreter

VitisHLS TCL interpreter has the following specifics:

a) There is no SVTCL in CLI or GUI variants. As a result only the Linux _tclsh_ TCL interpreter should be used.

_TODO:investigate_using_VitisHLS_option_to_pass_a_TCL_file_for_execution._



