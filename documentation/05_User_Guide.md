# 5. User Guide

In the examples below we will use Mentor's Questa&reg; simulator calls. Other simulators will have very similar, almost identical calls and behaviour.

## 5.1 SIMU HDL Configuration

The cloned repo doesn't need any configuration to run the existing test-cases, except the avalability of the simulation tools and licenses.

If a vendor specific IPs or primitives are used, the HDL vendor libraries have to be compiled as well.

If new test-cases are created then some configurations have to chanchanged, see details further below.

### 5.1.1 HDL Flow Paths Configuration

### 5.1.2 HDL Flow Options Configuration



### 5.1.3 HDL Test-cases, test-benches, RTL Configuration

### 5.1.4 HDL Regression Configuration



## 5.2 SIMU HLS Configuration



### 5.2.1 HLS Flow Paths Configuration

### 5.2.2 HLS Flow Options Configuration



### 5.2.3 HLS Test-cases, test-benches, HLS Configuration

### 5.2.4 HLS Regression Configuration



## 5.2 Run Single Test-Case simulation

There are several ways to run simulation of a single test-case. They acheive the same result, running a test-case simulation, but are invoked in different ways using:

 * simulator vendor TCL terminal or GUI
 * Linux bash terminal
 * Linux tclsh terminal

### 5.2.1 Test-case simulation - in Linux TCL interpreter and SIMU shell with SIMU test-case simulation call

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> run_testcase ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

The function _run_testcase_ is used only by the automated regression call. 

Additionally this function is a unified way to call a vendor simulator if it is hard to remember the vendor simulator specific call with its attributes.

### 5.2.2 Test-case simulation - in Linux TCL interpreter and SIMU shell

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../testcases/tc_reset/tc_reset.tcl 
```

As shown above instead of using the special function to run a test-case it is possible directly to source the test-case TCL script.

```
$ cd symphony/dev/sim/run
$ tclsh 
> source ../testcases/tc_reset/tc_reset.tcl 
```

Alternatively it is also possible not to use the SIMU shell as shown below. In this case the user interface lacks history and it is not convenient for use and for that reason it is shown here for completeness, but it is not recommended to be used.

### 5.2.3 Test-case simulation - in simulator vendor TCL interpreter

#### 5.2.3.1 ModelSim/QuestaSim vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

The example above uses the call to QuestaSim/Modelsim in CLI mode invoked from Linux bash terminal.

The ModelSim/QuestaSIm simulator vendor TCL shell in can be accessed in three different ways:

	* simulator vendor TCL terminal in the GUI
	* Linux bash terminal
	* Linux tclsh terminal

#### 5.2.3.2 Xilinx XSim vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ TODO: fix -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

TODO: fix

The example above uses the call to Xilinx XSim in CLI mode invoked from Linux bash terminal.

The ModelSim/QuestaSIm simulator vendor TCL shell in can be accessed in three different ways:

 * simulator vendor TCL terminal in the GUI
 * Linux bash terminal
 * Linux tclsh terminal

#### 5.2.3.3 Xcelium vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ TODO: fix -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

TODO: fix

The example above uses the call to Xilinx XSim in CLI mode invoked from Linux bash terminal.

The ModelSim/QuestaSIm simulator vendor TCL shell in can be accessed in three different ways:

 * simulator vendor TCL terminal in the GUI
 * Linux bash terminal
 * Linux tclsh terminal

#### 5.2.3.4 ActiveHDL vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ TODO: fix -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

TODO: fix

The example above uses the call to Xilinx XSim in CLI mode invoked from Linux bash terminal.

The ModelSim/QuestaSIm simulator vendor TCL shell in can be accessed in three different ways:

 * simulator vendor TCL terminal in the GUI
 * Linux bash terminal
 * Linux tclsh terminal

#### 5.2.3.2 VitisHLS vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ TODO: fix -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

TODO: fix

The example above uses the call to Xilinx XSim in CLI mode invoked from Linux bash terminal.

The ModelSim/QuestaSIm simulator vendor TCL shell in can be accessed in three different ways:

 * simulator vendor TCL terminal in the GUI
 * Linux bash terminal
 * Linux tclsh terminal

#### 5.2.3.2 Other vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ TODO: fix -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

TODO: fix

The example above uses the call to Xilinx XSim in CLI mode invoked from Linux bash terminal.

The ModelSim/QuestaSIm simulator vendor TCL shell in can be accessed in three different ways:

 * simulator vendor TCL terminal in the GUI
 * Linux bash terminal
 * Linux tclsh terminal

## 5.3 Run Manual Regression Simulation

There are several ways to run regression simulation of all test-cases. They achieve the practically the same result, running regression simulation, but have some subtle differences.

It has to be noted that regression call can be executed in all different ways the individual test-cases can run, as it was already covered:

 * simulator vendor TCL terminal or GUI
 * Linux bash terminal
 * Linux tclsh terminal

The manual regression uses a list of test-cases that is manually maintained. An example of that script can be found in _/dev/sim/regression_lists/REGRESSION_TC_LIST_MANUAL.tcl_

### 5.3.1 Manual Regression using manually maintained test-case list - in Linux TCL interpreter and SIMU shell

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```



## 5.4 Automatic Regression using auto-generated test-case list

The auto-generated test-cases list located in __/dev/sim/regression_lists/_ can be checked for regression progress information, which is a nice feature not available when running manual regression.

Another nice feature of the automated-regression is the option to run several threads in parallel. To accomplish this just open a few terminals and execute in each one a separate regression call. All regression calls will run in their own terminals and will use the auto-generated regression script to pick the next available test-case to run. This parallel execution makes the regression run much faster at the expense of using more simulator licenses, one per thread.

### 5.4.1 Automatic Regression -  in Linux TCL interpreter and SIMU shell with SIMU regression call

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../scripts_configure/run_regression.tcl
```



### 5.4.2 Automatic Regression - in ModelSim/QuestaSim simulator vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../scripts_configure/run_regression.tcl -do exit
```

The simulator vendor TCL shell can be accessed in simulator vendor TCL/GUI terminal or in bash terminal. The example above useds the call to QuestaSim/Modelsim in CLI mode invoked from Linux bash sell.

