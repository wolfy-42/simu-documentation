# Features

\[TOC]

## Features

The previously supported features will be maintained in the new SIMU reversion, unless explicitly mentioned that the support is obsolete.

### Test-case Simulation Features Supported

| TC Sim Feature                                                                                      | SIMU rev.                        | Feature ID |
| --------------------------------------------------------------------------------------------------- | -------------------------------- | ---------- |
| Universal test-case(TC) .tcl file (no need to edit each one for tc folder and name)                 | 5.0.b                            | f100       |
| Randomization, seed force                                                                           | 4.6                              | f101       |
| HDL Verilog, SystemVerilog and VHDL Compilation                                                     | 4.6                              | f102       |
| Elaboration phase                                                                                   | 4.6(xsim)                        | f103       |
| Optimization phase                                                                                  | 4.6(vsim), 5.0.a(xmsim)          | f104       |
| Simulation phase                                                                                    | 4.6                              | f105       |
| Coverage collection and report                                                                      | 4.6(vsim)                        | f106       |
| Open weveforms after closing simulation (using cheap viewer license ony)                            | 4.6, 5.0.a(xmsim)                | f107       |
| Open weveforms after  simulation is finished but not closed (using expensive simulator license ony) | 4.6, 5.0.a(xmsim)                | f108       |
| Execution in sim vendor TCL shell                                                                   | 4.6                              | f109       |
| Execution in Linux tclsh shell                                                                      | 5.0.a                            | f109       |
| Integration of FPGA vendor tool exported FPGA shell script                                          | 4.6(translation required), 5.0.a | f110       |
| Coloured print of sim result                                                                        | 4.6                              | f111       |
| Debug prints, enable/disable                                                                        | 5.0.a                            | f112       |
| Disable DUT RTL compilation for speed and debugging                                                 | 5.0.a                            | f113       |

### Regression Simulation Features Supported

| Regression Feature                                                                  | SIMU rev.                 | Feature ID |
| ----------------------------------------------------------------------------------- | ------------------------- | ---------- |
| Randomization, seed force                                                           | 4.6                       | f200       |
| Regression Pass/Fail statistics, per module, per TC, total                          | 4.6                       | f201       |
| Automatic regression TC list generation, white module list, black TC list           | 4.6                       | f202       |
| Regression TC list (manually or automatically generated) editing by hand            | 4.6                       | f203       |
| Regression history accumulation                                                     | 4.6                       | f204       |
| Regression running on many threads in parallel, using one sim license per thread    | 4.6                       | f205       |
| Send email when regression is completed                                             | 4.6                       | f206       |
| Manual regression (not identical to manually generated TC list), single thread only | 5.0.a                     | f207       |
| Coverage aggregation and report                                                     | 4.6(vsim)                 | f208       |
| Submit jobs on remote machines                                                      | (planned)                 | f209       |
| Regression run call with a number indicating the number of threads to execute       | (planned)                 | f210       |
| Regression progress for auto generated/manual/local/remote/threads regression       | 4.6(auto list), (planned) | f211       |
| run folder with date/time hash                                                      | 5.0.b                     | f212       |

### HDL Simulators Supported

| HDL Simulation Tool | Vendor    | Tool Version Tested(note) | SIMU Template                                          | SIMU rev.          | Feature ID |
| ------------------- | --------- | ------------------------- | ------------------------------------------------------ | ------------------ | ---------- |
| Questa®             | Mentor®   |                           | complete example                                       | 4.6                | f300       |
| ModelSim®           | Mentor®   |                           | complete example (identical to Questa)                 | 4.6                | f301       |
| Xsim®               | Xilinx®   |                           | complete examples                                      | 4.6                | f302       |
| ActiveHDL® GUI      | Aldec®    |                           | complete examples                                      | 4.6(most features) | f303       |
| Rviera®             | Aldec®    |                           | no examples                                            | (capable)          | f304       |
| Xcelium®            | Cadence®  |                           | partial configuration, complete TC, partial regression | 5.0.a              | f305       |
| VCS®                | Synopsys® |                           | no examples                                            | (capable)          | f306       |

### FPGA Vendors Supported

| FPGA Compile Tool | Vendor     | Tool Version Tested(note) | SIMU Template     | SIMU rev. | Feature ID |
| ----------------- | ---------- | ------------------------- | ----------------- | --------- | ---------- |
| Vivado®           | Xilinx®    |                           | IPI example       | 4.6       | f400       |
| ISE               | Xilinx ®   |                           | no examples       | 4.6       | f401       |
| Quartus®          | Altera®    |                           | Platform Designer | 4.6       | f402       |
| Diamond®          | Lattice®   |                           | no examples       | (capable) | f403       |
| Radiant®          | Lattice®   |                           | no examples       | (capable) | f404       |
|                   | Microchip® |                           | no examples       | (capable) | f405       |

### OS and TCL Interpreters

| OS       | Vendor     | Version | SIMU Mode                               | SIMU rev. | Feature ID |
| -------- | ---------- | ------- | --------------------------------------- | --------- | ---------- |
| Windows® | Microsoft® | 10      | sim vendor TCL shell                    | 4.6       | f500       |
| RedHat®  | RedHat®    | 7,8     | sim vendor TCL shell, Linux tclsh shell | 4.6       | f501       |
| Linux    | Linux      |         | sim vendor TCL shell, Linux tclsh shell | 4.6       | f502       |

### HDL Languge

| HDL Language  | Template          | SIMU rev. | Feature ID |
| ------------- | ----------------- | --------- | ---------- |
| Verilog       | complete examples | 4.6       | f600       |
| SystemVerilog | complete examples | 4.6       | f601       |
| VHDL          | complete examples | 4.6       | f602       |
| Vivado® HLS   | partial examples  | 5.0.a     | f603       |

### Frameworks and Libraries

| Frameworks and Libraries | Template | SIMU rev.(note) | Feature ID |
| ------------------------ | -------- | --------------- | ---------- |
| UVM                      |          | (planned)       | f700       |
| OSVVM                    |          | (planned)       | f701       |

### Notes

'complete example' - several example including simulator configuration, Test-Bench(TB), Test-Case(TC), Regression, Sim Vendor TCL shell, Linux tclsh shell, optimization, coverage

'Feature ID' - unique ID used to keep track of the features developemnt, for internal developes use ony
