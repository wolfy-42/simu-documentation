# SIMU Releases



## Release 5.1.b - beta - 2024-08-08

Beta release features:

* many features are tested
* added VitisHLS support for multiple test-cases and test-benches and regression 

Planned features:

1. HLS simulation library with common print error, print warning, print message functions
2. Single TC report all stages pass/fail at the end detecting native Vitis messages
3. Single TC aggregate reports into one
4. HLS regression results aggregation, including with RTL aggregation
5. HLS synthesizable code and TBs and TCs for AXI-MM burst write and read
6. Reduce/optimize HLS settings file
7. Reduce/optimize HLS paths file in TB/TC folder and in build/scripts folder
8. Test RTL simulation and regression on Windows, should be functional but it needs retesting



## Release 5.0.a - alpha - 2024-06-12

Alpha release:

* many features are not well tested
* somewhat significant architecture change
* added universal compile/optimize/simulation/coverage calls to enable simulator seamless switching and easy addition of new simulators
* added non vendor TCL shell simulation calls
* added easy integration of FPGA vendor exported simulation scripts 
* simplified compile scripts 
* unified simulation calls
* added manual regression



## Release 4.6 - stable - 2018-31-10

Stable release:

* almost all features are tested and functional