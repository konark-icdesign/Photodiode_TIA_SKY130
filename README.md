# Photodiode TIA - SKY130

starting this as my analog IC project. goal for now is simple: take the small current from a photodiode and convert it into a useful voltage using a CMOS transimpedance amplifier.

I did some pre-work before making this repo so I already have a rough direction, but these numbers are just starting points. they can change when I start the actual device simulations.

- supply: 1.8 V
- photodiode: external silicon photodiode for V1
- photocurrent range to start with: 50 nA to 500 nA
- feedback resistor: 1 MOhm
- feedback capacitor: around 3.3 pF
- input/detector capacitance: around 5 pF nominal
- bandwidth target: around 50 kHz
- first amplifier I want to try: simple 5 transistor OTA

I first thought about going lower than 50 nA and also looked at a bigger two-stage OTA, but for now I dont want to make the circuit complicated for no reason. I will see what the actual SKY130 simulations show first.

first actual work here will be device characterization. I want to check the NMOS and PMOS first and then build the OTA from there, not put a finished circuit in the repo on day one.

first plots:

- Id vs Vgs
- Id vs Vds
- gm vs Vgs
- gm/Id vs Vgs

rough flow I plan to use:

`SKY130 -> Xschem -> ngspice -> C/gnuplot -> Magic -> Netgen -> KLayout`

this repo is just starting. no DRC/LVS/GDS or final performance claims yet. I will add those only when I actually have the reports and results.
