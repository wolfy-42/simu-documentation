

[TOC]

# User Guide 

In the examples below we will use Mentor's Questa&reg; simulator calls. Other simulators will have very similar, almost identical calls and behaviour.

## SIMU Configuration



## Run Single Test-Case simulation



```
cd symphony/dev/sim/run
vsim -c -do ../testcases/tc_reset/tc_reset.tcl -do quit
```

## Run Regression Simulation



```
cd symphony/dev/sim/run
vsim -c -do ../scripts_configure/run_regression.tcl -do quit
```
