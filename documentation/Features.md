

[TOC]

# Features 

The previously supported features will be maintained in the new SIMU reversion, unless explicitly mentioned that the support is obsolete.

## Test-case Simulation Features Supported

| TC Sim Feature                                               | Support |
| ------------------------------------------------------------ | ------- |
| universal test-case(TC) .tcl file (no need to edit each one for tc folder and name) | 5.0.b   |
| Randomization, seed force                                    | 4.6     |

## Regression Simulation Features Supported

| Regression Feature                                           | SIMU rev.(note) |
| ------------------------------------------------------------ | --------------- |
| Randomization, seed force                                    | 4.6             |
| Regression Pass/Fail statistics, per module, per TC, total   | 4.6             |
| Automatic regression TC list generation, white module list, black TC list | 4.6             |
| Regression TC list (manually or automatically generated) editing by hand | 4.6             |
| Regression history accumulation                              | 4.6             |
| Regression running on many threads in parallel, using one sim license per thread | 4.6             |
| Regression run call with a number indicating the number of threads to execute | (planned)       |
| Send email when regression is completed                      | 4.6             |
| Manual regression (not identical to manually generated TC list) | 5.0.a           |
| Coverage collection and report                               | 4.6             |
| Submit jobs on remote machines                               | (planned)       |

## HDL Simulators Supported

| HDL Simulation Tool | Vendor        | Tool Version Tested(note) | SIMU Template                          | SIMU rev.(note)    |
| ------------------- | ------------- | ------------------------- | -------------------------------------- | ------------------ |
| Questa&reg;         | Mentor&reg;   |                           | complete example                       | 4.6                |
| ModelSim&reg;       | Mentor&reg;   |                           | complete example (identical to Questa) | 4.6                |
| Xsim&reg;           | Xilinx&reg;   |                           | complete example                       | 4.6                |
| ActiveHDL&reg; GUI  | Aldec&reg;    |                           | configuration example only             | 4.6(most features) |
| Rviera&reg;         | Aldec&reg;    |                           | no example                             | (capable)          |
| Xelium&reg;         | Cadence&reg;  |                           | configuration example only             | 5.0.a              |
| VCS&reg;            | Synopsys&reg; |                           | no example                             | (capable)          |



## FPGA Vendors Supported

| FPGA Compile Tool | Vendor         | Tool Version Tested(note) | SIMU Template     | SIMU rev.(note) |
| ----------------- | -------------- | ------------------------- | ----------------- | --------------- |
| Vivado&reg;       | Xilinx&reg;    |                           | IPI example       | 4.6             |
| ISE               | Xilinx &reg;   |                           | no example        | 4.6             |
| Quartus&reg;      | Altera&reg;    |                           | Platform Designer | 4.6             |
| Diamond&reg;      | Lattice&reg;   |                           | no example        | (capable)       |
| Radiant&reg;      | Lattice&reg;   |                           | no example        | (capable)       |
|                   | Microchip&reg; |                           | no example        | (capable)       |

## OS and TCL Interpreters

| OS           | Vendor         | Version | SIMU Mode                               | SIMU rev.(note) |
| ------------ | -------------- | ------- | --------------------------------------- | --------------- |
| Windows&reg; | Microsoft&reg; | 10      | sim vendor TCL shell                    | 4.6             |
| RedHat&reg;  | RedHat&reg;    | 7,8     | sim vendor TCL shell, Linux tclsh shell | 4.6             |
| Linux        | Linux          |         | sim vendor TCL shell, Linux tclsh shell | 4.6             |

## HDL Languge

| HDL Language    | Template             | SIMU rev.(note) |
| --------------- | -------------------- | --------------- |
| Verilog         | full sim environment | 4.6             |
| SystemVerilog   | full sim environment | 4.6             |
| VHDL            | full sim environment | 4.6             |
| Vivado&reg; HLS |                      | 5.0.a           |

## Frameworks and Libraries

| Frameworks and Libraries | Template | SIMU rev.(note) |
| ------------------------ | -------- | --------------- |
| UVM                      |          | (planned)       |
| OSVVM                    |          | (planned)       |

