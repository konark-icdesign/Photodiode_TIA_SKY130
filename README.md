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

first actual work here is device characterization. I added the first NMOS/PMOS Id-Vgs and Id-Vds sweep decks. no run data committed yet, so I am not treating them as results yet.

next after the first runs:

- gm vs Vgs
- gm/Id vs Vgs
- repeat for a few channel lengths
- pick a useful bias region
- build the first 5T OTA

rough flow I plan to use:

`SKY130 -> Xschem -> ngspice -> C/gnuplot -> Magic -> Netgen -> KLayout`

there is also some pre-project numerical work in `notes/prework_checks.md`. that includes why I landed near 1 MOhm / 3.3 pF, the 50-500 nA range, the 5T OTA choice and the rough stress test. those numbers are just planning/reference work, not final project measurements.

`notes/problems_so_far.md` has the assumptions that already changed and what is still not proven.

no DRC/LVS/GDS or final performance claims yet. I will add those only when I actually have the reports and results.
