# device characterization notes

Starting here before touching the OTA sizing.

For now I am using W = 10um and L = 0.5um just to get a feel for the SKY130 1.8V devices. These are not final transistor sizes.

First checks:

- NMOS Id vs Vgs
- NMOS Id vs Vds
- PMOS current vs gate voltage
- PMOS current vs drain voltage

After the first runs I want to add gm and gm/Id plots and then repeat some sweeps for a few channel lengths. Main thing I want from this is a sensible bias region before I build the differential pair.

Nothing in this folder should be treated as a measured result until the ngspice run data is committed too.
