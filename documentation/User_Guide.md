# User Guide

In the examples below we will use Mentor's Questa&reg; simulator calls. Other simulators will have very similar, almost identical calls and behaviour.

## SIMU Configuration

The cloned repo doesn't need any configuration to run the existing test-cases, except the avalability of the simulation tools and licenses.

If a vendor specific IPs or primitives are used, the HDL vendor libraries have to be compiled as well.

If new test-cases are created then some configurations have to chanchanged, see details further below.

## Run Single Test-Case simulation

There are several ways to run simulation of a single test-case. They acheive the same result, running a test-case simulation, but have some subtle differences.

### Test-case simulation - in simulator vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
```

The simulator vendor TCL shell can be accessed in GUI or in CLI. The example above useds the call to Quetta/Modelsim in CLI mode invoked from Linux bash sell.

### Test-case simulation - in Linux TCL interpreter and SIMU shell with SIMU test-case simulation call 

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
$ run_testcase ../testcases_envFidus_sv_simMquestaXvivadoCxcelium/tc_fidus_common/tc_fidus_clock_reset.tcl
```

### Test-case simulation - in Linux TCL interpreter and SIMU shell

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../testcases/tc_reset/tc_reset.tcl 
```

## Run Regression Simulation

There are several ways to run a regression simulation of a all test-cases. They acheive the practically the same result, running regression simulation, but have some subtle differences.

### Regression using atomatic test-case list - in simulator vendor TCL interpreter

```
$ cd symphony/dev/sim/run
$ vsim -c -do ../scripts_configure/run_regression.tcl -do exit
```

The simulator vendor TCL shell can be accessed in GUI or in CLI. The example above useds the call to Quetta/Modelsim in CLI mode invoked from Linux bash sell.

### Regression using atomatic test-case list -  in Linux TCL interpreter and SIMU shell with SIMU regression call 

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../scripts_configure/run_regression.tcl
```

### Regression using manual test-case list - in Linux TCL interpreter and SIMU shell

```
$ cd symphony/dev/sim/run
$ tclsh runme_simu_shell.tcl
> source ../home/work/des.v/trunk/simu_fixes/simu/dev/sim/regression_lists_templates/REGRESSION_TC_LIST_MANUAL.tcl
```

