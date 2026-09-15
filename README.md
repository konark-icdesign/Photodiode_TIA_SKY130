# Photodiode TIA in SKY130

This project is a low-light photodiode readout circuit built around a transimpedance amplifier (TIA) in SKY130.

I am starting from the sensor side rather than treating this as just another op-amp exercise. A photodiode produces a small current, so the first problem is to convert that current into a useful voltage while keeping the circuit stable with detector capacitance at the input.

## V1 scope

The first version will use an external silicon photodiode model and a SKY130 CMOS TIA.

Current starting targets:

- supply: 1.8 V
- external photodiode
- photocurrent range: 50 nA to 500 nA
- nominal detector/input capacitance: 5 pF
- feedback resistor: 1 MOhm
- feedback capacitor: 3.3 pF starting value
- target signal bandwidth: around 50 kHz
- manual analog layout
- DRC, LVS and extracted post-layout simulation before calling the design complete

These are design targets, not measured results. They can change once the actual SKY130 device simulations start.

## Why this range

I first considered pushing the minimum current lower, but a very small signal quickly makes OTA offset, low-frequency noise and leakage much more important. For V1 I am keeping the problem realistic and will characterize how far the design can be pushed after the basic circuit works.

## Planned flow

SKY130 PDK -> Xschem -> ngspice -> C/gnuplot for result processing -> Magic -> Netgen -> parasitic extraction -> post-layout ngspice -> KLayout

## First technical step

Characterize the SKY130 NMOS and PMOS devices before fixing the amplifier sizing.

The first plots I want are:

- Id vs Vgs
- Id vs Vds
- gm vs Vgs
- gm/Id vs Vgs

The amplifier and TIA design will be built from those results rather than starting with final transistor sizes copied into the repository.
