# 5. User Guide

In the examples below we will use Mentor's Questa&reg; simulator calls. Other simulators will have very similar, almost identical calls and behaviour.

## 5.1 SIMU HDL Configuration

The cloned repo doesn't need any configuration to run the existing test-cases, except the avalability of the simulation tools and licenses.

If a vendor specific IPs or primitives are used, the HDL vendor libraries have to be compiled as well.

If new test-cases are created then some configurations have to chanchanged, see details further below.

### HDL Flow Paths Configuration

There are several paths that have to be configured before the environment can be used:

1. Paths in the general sim/scripts
2. Paths in the sim/test-cases script

### HDL Flow Options Configuration

Configure the command line options script

#### TC execution flow option

1. 

#### Regression execution flow options

1. 

### HDL Test-cases, test-benches, RTL Configuration

The test-case (TC) name in the test-case TCL script

The RTL files in the compile_all script

The TB files the TC files in the compile_all script

### HDL Regression Configuration

The test-case/test-bench folder in the regression config script

## 5.2 SIMU HLS Configuration

### HLS Flow Paths Configuration

There are several paths that have to be configured before the environment can be used:

1. Paths in the general sim/scripts folder
2. Paths in the sim/test-cases folder
3. Paths in build/scripts folder

### HLS Flow Options Configuration

The HLS Vitis project is created from TCL sources in every run. The project that is run for simulation, synthesis or export is a Vitis project that can be open in Vitis GUI after the run. The Vitis project is deleted before every new run unless that is disabled in the configuration file. 

If the Vitis project deleting option is disabled before the new run, the designer has to pay attention not to have conflicting residual configurations in the Vitis design from the previous run because the project creation scripts will overwrite some settings.

### HLS Test-cases, test-benches, HLS Configuration



### HLS Regression Configuration



## 5.3 Run Single Test-Case (TC) simulation

It has to be noted that 

1. All TCs have to be executed executed from the **run** folder > _cd symphony/dev/sim/run_
2. All TC names have to start with the **tc_** prefix. This is required by the regression automation and results parsing.
3. Every HDL TC is paired with an identical in name TCL script. Similarly every HLS TC is paired with an almost identical in name TCL scripts where the suffix might differ for the TCL file to indicate different type of HLS TC execution.
4. That TC TCL script is the one actually executed by the SIMU framework. 
5. The TC TCL script contains some required by SIMU information, such as several pre-configured paths, the test-case name, also one parameter for HLS TC type of execution.

There are several ways to run simulation of a single test-case. They achieve the same result, running a test-case simulation, but are invoked in different ways using:

a) Linux TCL interpreter using tclsh terminal  

b) Simulator vendor TCL CLI in bash terminal 

c) Simulator vendor TCL terminal in GUI 

 * _The three use-cases above can be used with or without the SIMU shell_ 
 * _The three use-cases above can be used with the run_testcase function form the SIMU shell or by simply "sourcing" the test-case script_

### 5.3.1 Test-case simulation - using Linux TCL interpreter

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

a) There is no SVTCL in CLI or GUI variants in VitisHLS. As a result only the Linux _tclsh_ TCL interpreter should be used.

~TODO:investigate_using_VitisHLS_option_to_pass_a_TCL_file_for_execution.~

## 5.4 Run Regression Simulation

There are several ways to run a regression simulation of all test-cases. They achieve the practically the same result, running regression simulation, but have some subtle differences.

It has to be noted that a regression call can be executed in all different ways similar to the individual test-cases run execution types, as it was already covered:

a) Linux TCL interpreter using tclsh terminal

b) simulator vendor TCL CLI in bash terminal 

c) simulator vendor TCL terminal in GUI - can be used to run simulations on Windows OS

 * _The three use-cases above can be used with or without the SIMU shell_ 
 * _The three use-cases above can be used with the run_testcase function form the SIMU shell or by simply sourcing the test-case script_

### 5.4.1 Manual Regression Simulation 

The manual regression uses a list of test-cases that is manually maintained. An example of that script can be found in _/dev/sim/regression_lists/REGRESSION_TC_LIST_MANUAL.tcl_

A manual regression can't run in multiple parallel threads.

The regression run progress can't be easily monitored.

#### 5.4.1.1 Manual Regression - using Linux TCL interpreter

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

~TODO:investigate_using_VitisHLS_option_to_pass_a_TCL_file_for_execution.~

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

~TODO:investigate_using_VitisHLS_option_to_pass_a_TCL_file_for_execution.~

