# 5. User Guide

In the examples below we will use Mentor's Questa&reg; simulator calls. Other simulators will have very similar, almost identical calls and behaviour.

## 5.1 SIMU HDL Configuration

The cloned repo doesn't need any configuration to run the existing test-cases, except the avalability of the simulation tools and licenses.

If a vendor specific IPs or primitives are used, the HDL vendor libraries have to be compiled as well.

If new test-cases are created then some configurations have to chanchanged, see details further below.

### HDL Flow Paths Configuration

There are several paths that have to be configured before the environment can be used:

1. Paths in the general script
2. Paths in the test-case script

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

There are several paths that have to be configured before the environment can be used:

1. Paths in the general script
2. Paths in the test-case script

### HLS Flow Paths Configuration

### HLS Flow Options Configuration



### HLS Test-cases, test-benches, HLS Configuration

### HLS Regression Configuration



## 5.3 Run Single Test-Case simulation

It has to be noted that 

1. All test-cases have to be executed executed from the **run** folder > _cd symphony/dev/sim/run_
2. All test-case names have to start with the **tc_** prefix. This is required by the regression automation and results parsing.
3. Every HDL test-case is paired with an identical in name TCL script. 
4. That test-case TCL script is the one actually executed by the SIMU framework. 
5. The test-case TCL script contains some required by SIMU information, such as several pre-configured paths and the test-case name.

There are several ways to run simulation of a single test-case. They acheive the same result, running a test-case simulation, but are invoked in different ways using:

 * Linux TCL interpreter using tclsh terminal  
 * Simulator vendor TCL CLI in bash terminal 
 * Simulator vendor TCL terminal in GUI 
 * _The three use-cases above can be used with or without the SIMU shell_ 
 * _The three use-cases above can be used with the run_testcase function form the SIMU shell or by simply "sourcing" the test-case script_

### 5.3.1 Test-case simulation - using Linux TCL interpreter

**(with/without SIMU shell, with/without SIMU _run_testcase_ simulation call, simulator vendor tool independent)**

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

```
$ cd symphony/dev/sim/run
$ tclsh 
> vsim -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit 
```

Alternatively, as seen above, it is also possible to use the simulator vendor TCL interpreter inside the Linux tclsh interpreter. This is somewhat redundant using two TCL interpreters and not user friendly.

#### tclsh with ModelSim/QuestaSim

#### tclsh with Xilinx XSim

#### tclsh with Xcelium

Specific settings have to be enabled in this mode TODO:

#### tclsh with ActiveHDL

Specific settings have to be enabled in this mode TODO:

#### tclsh with VitisHLS

### 5.3.2 Test-case simulation - using simulator vendor TCL interpreter

**(from Linux bash or simulator GUI)**

#### ModelSim/QuestaSim vendor TCL interpreter

**(with/without SIMU shell, with/without SIMU _run_testcase_ simulation call, from Linux bash or from simulator vendor GUI, can run on Windows OS)**

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

The example above is using the QuestaSim/Modelsim TCL interpreter in CLI mode invoked from Linux bash terminal.

```
-- start the simulator vendor GUI
# vsim
-- the commands below are executed in the Modelsim/Questa GUI TCL terminal
> cd symphony/dev/sim/run
> source runme_simu_shell.tcl
> run_testcase ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The example above is using the SIMU shell _run_testcase_ function in the QuestaSim/Modelsim TCL interpreter with the SIMU shell in the simulator vendor GUI TCL terminal.

```
-- start the simulator vendor GUI
# vsim
-- the commands below are executed in the Modelsim/Questa GUI TCL terminal
> cd symphony/dev/sim/run
> source ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The example above is using the QuestaSim/Modelsim TCL interpreter in simulator vendor GUI TCL terminal.

#### Xilinx XSim vendor TCL interpreter

#### Xcelium vendor TCL interpreter

Xcelium TCL interpreter lacks CLI mode and for this reason only the SimVision GUI TCL terminal is available.

```
-- start the simulator vendor GUI
# simvision
-- the commands below are executed in the Modelsim/Questa GUI TCL terminal
> cd symphony/dev/sim/run
> source ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The example above is using the Xcelium TCL interpreter in simulator vendor GUI TCL terminal.

Specific settings have to be enabled in this mode TODO:



#### ActiveHDL vendor TCL interpreter

Specific settings have to be enabled in this mode TODO:

#### VitisHLS vendor TCL interpreter

#### Other vendor TCL interpreter

## 5.4 Run Regression Simulation

There are several ways to run a regression simulation of all test-cases. They achieve the practically the same result, running regression simulation, but have some subtle differences.

It has to be noted that a regression call can be executed in all different ways similar to the individual test-cases run execution types, as it was already covered:

 * Linux TCL interpreter using tclsh terminal
 * simulator vendor TCL CLI in bash terminal 
 * simulator vendor TCL terminal in GUI - can be used to run simulations on Windows OS
 * _The three use-cases above can be used with or without the SIMU shell_ 
 * _The three use-cases above can be used with the run_testcase function form the SIMU shell or by simply sourcing the test-case script_

### 5.4.1 Manual Regression Simulation 

The manual regression uses a list of test-cases that is manually maintained. An example of that script can be found in _/dev/sim/regression_lists/REGRESSION_TC_LIST_MANUAL.tcl_

A manual regression can't run in multiple parallel threads.

The regression run progress can't be easily monitored.

#### 5.4.1.1 Manual Regression - using Linux TCL interpreter

**(with/without SIMU shell, with TCL _source_ call, simulator vendor tool independent)**

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

#### 5.4.1.2 Manual Regression - using simulator vendor TCL interpreter

**(from Linux bash or simulator GUI)**

##### ModelSim/QuestaSim vendor TCL interpreter

**(can run on Windows OS)**

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

The example above uses the call to QuestaSim/Modelsim TCL terminal in CLI mode invoked from Linux bash sell.

```
# cd symphony/dev/sim/run
# source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

The example above is using the QuestaSim/Modelsim TCL interpreter GUI terminal.

##### Xilinx XSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

### 5.4.2 Automatic Regression 

The Automatic Regression gets its name from the auto-generated test-cases list located in __/dev/sim/regression_lists/_. 

The auto-generated test-case list can be checked for regression progress information, which is a nice feature not available when running manual regression.

Another nice feature of the automated-regression is the option to run several threads in parallel. To accomplish this just open a few terminals and execute in each one a separate regression call. All regression calls will run in their own terminals and will use one auto-generated regression script to pick the next available test-case to run and to exchange information about the next available next test-cases to execute. This parallel execution makes the regression run much faster, but at the expense of using more simulator licenses, one per thread.

#### 5.4.2.1 Automatic Regression - using Linux TCL interpreter

**(with SIMU shell, with SIMU regression call, simulator vendor tool independent)**

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../scripts_configure/run_regression.tcl
```

#### 5.4.2.2 Automatic Regression - using simulator vendor TCL interpreter

**(from Linux bash or simulator GUI)**

##### ModelSim/QuestaSim vendor TCL interpreter

**(can run on Windows OS)**

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../scripts_configure/run_regression.tcl -do exit
```

The example above useds the call to QuestaSim/Modelsim in CLI mode invoked from Linux bash sell.

```
# cd symphony/dev/sim/run
# source ../scripts_configure/run_regression.tcl
```

The example above is using the QuestaSim/Modelsim TCL interpreter GUI terminal. 

##### Xilinx XSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

##### ModelSim/QuestaSim vendor TCL interpreter

