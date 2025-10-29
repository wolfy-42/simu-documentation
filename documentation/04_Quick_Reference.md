# 4. Quick Reference



## 4.1 Simulation Commands

### Terminal

```
cd dev/sim/run
vsim -c -do ../testcases_envFidus_sv_simMquestaXvivado/tc_fidus_common/tc_fidus_clock_reset.tcl -do exit
vsim -c -do ../scripts_config/run_regression.tcl -do exit
```

### ModelSim TCLshell

```
cd dev/sim/run
tclsh (call Linux TCL shell)
source ../testcases_envFidus_sv_simMquestaXvivado/tc_fidus_common/tc_fidus_clock_reset.tcl
```


### SIMU-CLI inside the TCLshell (Linux or Tool TCLshell)

```
tclsh runme_simu_shell.tcl (invoke SIMU-CLI on Linux TCLshell)
vsim -c -do runme_simu_shell.tcl (invoke SIMU-CLI on ModelSim/Vivado TCLshell)
cd dev/sim/run
run_testcase ../testcases/tc_case/tc_case.tcl
run_testcase ../testcases/tc_case/tc_case.tcl –coverage
run_testcase ../testcases_envFidus_sv_simMquestaXvivado/tc_fidus_common/tc_fidus_clock_reset.tcl -optimize -coverage
run_regression
run_regression -modname tc_fidus_clock_reset
run_regression –coverage
select_simulator [xsim|vsim]
```

## 4.2 Simulation CLI Options

Edit file ```config_cmd_line_options_default.tcl``` to configure simulation CLI options.

**Common options**

```set ::CMD_ARG_LOGGING 1 # -logging```     : if set to 0, omits creation of WLF/WDB (vsim/xsim) or ASDB Active-HDL files.

```set ::CMD_ARG_OPTIMIZE 0 # -optimize```     : if set to 1, creates optimized simulation models. Not available in xsim.

``set ::CMD_ARG_COVERAGE 0 # -coverage``     : if set to 0, omits code coverage information. Not available in xsim.

`set ::CMD_ARG_SEED "” # -seed seedVal`   : if set, sets and locks the seed to the set value for all test-cases across all modules. If blank, seed is randomly generated.

`set ::CMD_ARG_SIMTOOL "vsim” # -simtool simTool` : Simulator vendor to vsim - for Questa/Modelsim, xsim - for Vivado, ahdl_gui/ahdl_sh - for avhdl.exe Active-HDL GUI TCL or vsimsa.bat TCL shells. Should not be blank.

`set ::CMD_ARG_AUTO_SIMTOOL 1 #` Attempt to auto-detect active simulator first

**Individual** **test-case** **Specific**

`set ::CMD_ARG_VIEW 0 # -view`       : if set to 1, opens simulator in viewer mode (questa or vivado gui).

`set ::CMD_ARG_COMPILE 1 # -compile`    : if set to 0, disables compilation of libraries and test-bench. Ie. does not re-compile. The testcase .sv file is still compiled.

**Regression Specific**

`set ::CMD_ARG_MODNAME "" # -modname modName` : if set, runs regression on a single module directory with modName. If blank, all module are included.

`set ::CMD_ARG_REPORT 0 # -report`      : if set, do not run regression, just recreate reports (regression results, and coverage).

## 4.3 Project(Main) TCL Script Configurations

**Configuring SIMU** 

Tell SIMU where you will be putting your testcases. This is done in ```scripts_config/config_settings_general.tcl``` and ```script_config/config_settings_testcases.tcl```. In ```config_settings_general.tcl```, add a line such as: 

```set TESTCASESDIR $::SIMDIR/testcases```

You can also remove the other ```TESTCASEDIR_*``` definitions

In ```config_settings_testcases.tcl``` you must edit the procedures ```getRegressionTCIgnoreList``` and ```getExternalTestcaseDirs```. In ```getRegressionTCIgnoreList```, remove any existing lappend lines. In ```getExternalTestcaseDirs```, remove the existing entries lappend lines and add one for your testcase directory, e.g. 

```lappend testcaseDirList $::TESTCASEDIR```

For Questa, if you are using complied FPGA vendor libraries, copy the associated ```modelsim.ini``` file into ```sim/run```.

## 4.4 TC/TB TCL Files Requiring Modifications

**Creating Testbenches and Testcases**

Testbench are created under sim/testcases. Each testbench has it’s own folder, which must start with tc_. Each testbench has a compile script (compile_all.tcl) and a testbench definition ([tb.sv/v/vhd](http://tb.sv/v/vhd)). For VHDL there is also a interconnect package, global_signal_pkg.vhd. The testbench has one or more testcases, each composed of a source file (tc_*.sv/v/vhd) and a definition script. The testcase name must start with tc_. 

The easiest way to create a testbench and testcase is to start from an existing one. This avoids the need to recreate the boilerplate necessary for each test case from scratch. 

**compile_all.tcl** 

This file contains the commands to compile all the source files required for your testbench and testcase. All of the compile steps, except for the testcase compilation are dependent on $::CMD_ARG_COMPILE. This allows to avoid recompiling the static testbench code when running multiple testcases with the same testbench during regression. 

By using the simu_vlog and simu_vcom commands, you can create a compile_all.tcl that will work with either Questa/Modelsim or Viavado XSIM. If you require compiler features that are not accessible through this wrapper, then you can also use the simulator specific commands directly (e.g. vcom for questa). 

If your testbench targets a Xilinx device and uses any Xilinx IP, you should make sure to compile glbl.v, It can then be added to the simulation by adding the following to the file, near the bottom, outside any if statements.

lappend ::EXTRA_UNITS "work.glbl"

If your testbench requires additional design libraries beyond the standard “work”, such as when using vendor libraries, you can add them by name near the bottom of the file using, for example:

lappend ::EXTRA_LIBS "secureip"

**tb.{sv,v,vhd}**

This file contains the DUT instantiation, connections to the DUT and BFM interface instantiations for class-based BFMS. It may also contain checkers or other logic you want to share between testcases. This file should also instantiate the test_case module/entity, which will be defined by your tc_*.{sv/v/vhd} file

**tc_\*.tcl** 

This file is what is executed to run each testcase. The primary changes that need to be mad each time this file is copied is to update the values of TCFILENAME and TCSUBDIR to reflect the testcase and testbench name respectively. 

**tc_\*.{sv/v/vhd}**

This file defines your actual testcase flow. Make sure to update the TC_NAME parameter to match the filename. Typically an initial block is used to define the main test flow. The first thing in this block should be a call to the initSim function on the relevant version of sim_management. The last item in this block, at the conclusion of all testing should be a call to testComplete function of sim_management, which will report the test status and terminate the simulation. 

Between these calls you should exercise the DUT, checking the results and reporting errors using the printError function and successes using printPasss, or using the checkSig and checkInt functions. These will feed into the error reporting of sim_management, incrementing the counter appropriately. 

## 4.5 Simulation Files and Folders in dev/sim/

•**bfms****/** - Stock bus functional models for common interfaces. 

•Clock and reset generators, simple AXI-lite master

•fidus_axis_video_bfms/ - Class based BFMS for generating and receiving AXI-Stream video

•**cores/** - Location for storing generated IP cores. For xilinx contains a copy of glbl.v/vhd when if needed. Often has a script to regenerate all required IP for simulation. 

•**jenkins****/** - Files related to regression tests for Simu itself. Don’t copy to instantiated projects

•**libraries/** - Verilog/SV/VHDL libraries supporting testcase development and error reporting

•sim_management* - Main runtime and reporting library for each language. 

•**regression_results****/** - Contains tracking and output for regression runs

•**run/** - working directory

•runme_simu_shell.tcl - SIMU shell launcher

•modelsim.ini - Modelsim/Questa configuration. Replace with the version from your compiled simulation libraries if using Xilinx/Vendor IP/Primitives

•**scripts_config****/** - Simu TCL files including both configuration and core logic

•config_settings_general.tcl - Common definitions. 

•config_settings_testcases.tcl - List of testcase folders to search during regression 

•scripts_lib/ - Core logic for simu scripts

•tccommon_lib/ - Helper snippets/includes for testcase scripts

**testcases\*/** - Directories for testcases. Many projects will have just a single testcases directory, usually just called testcases

**tc_\*/** - Contains one testbench and one or more test-cases using the testbench. Often one of these folders per tested module, plus additional for top-level. 

compile_all.tcl - Compile scripts used for all simulators. Called automatically by the individual tc_*.tcl scripts

complile_all_<simulator>.tcl - Simulator specific compile script. Used in preference to the generic script if present. Not typically needed, but used in some legacy testbenches. 

tb.[sv/v/vhd] - Testbench. Simulation top-level, typically instantiates DUT, BFMs/Interfaces and related signals. Instantiates testcase. 

tc_*.tcl - Testcase script. Defines testcase name and location, then hands off to simu libraries for execution 

tc_*.[sv/v/vhd] - Testcase. Defines the module/entity test_case. Drives the test flow. 

global_signal_pkg.vhd - Interconnect package, used only for VHDL testcases

**_runNNNNN/** – multi-thread regression generated folder which can safely be deleted after the regression completes. Every simulation thread creates a unique folder with a random NNNNN number. Every simulation thread should be started in a separate terminal.



