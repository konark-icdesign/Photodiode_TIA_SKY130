# problems / changes so far

keeping this here because a few things already changed before the actual transistor work started.

## photodiode capacitance was not as simple as I first thought

I was looking at a real photodiode datasheet and saw a capacitance of a few pF. first thought was just use that value.

problem was the datasheet capacitance was measured at a certain reverse bias. so that number is not automatically the capacitance I will get in my setup.

for now I am using about 5 pF as a starting model and later I will sweep the input capacitance. exact photodiode can be locked after the TIA itself is working.

## 5 nA was probably too low for the first version

at first I wanted something like 5 nA to 500 nA.

with 1 MOhm feedback, 5 nA is only around 5 mV output change. that starts getting too close to offset, mismatch, leakage and low frequency noise for a first analog design.

so I moved the V1 range to about 50 nA to 500 nA.

if the real circuit later works below 50 nA, good. I would rather measure that later than claim it now.

## feedback capacitor matters a lot

in the rough TIA model, using the detector capacitance with just the feedback resistor gave too much peaking / bad damping.

I tried different Cf values. small Cf gave more bandwidth but worse damping. increasing Cf reduced bandwidth and made the response calmer.

around 3.3 pF with 1 MOhm puts the response close to the ~50 kHz region I want, so that is the current starting value. not final yet.

## I first went toward a two-stage OTA

first plan was a two-stage OTA, around 50-60 dB gain and a few MHz GBW.

then I checked whether I actually need that much for this TIA. a public SKY130 5T OTA reference has about 53 V/V gain (~34.5 dB). with 1 MOhm feedback the simple finite-gain estimate is about 981 kOhm effective transimpedance, roughly 2% below ideal.

that is not bad for V1, so I decided to start with the simpler 5T OTA instead of adding another stage for no reason.

this can still change if my own simulation shows the 5T version is not enough.

## one thing still not proven: real loop stability

some of the early stability numbers came from a simplified OTA model. that was useful for checking direction, but it does not prove the real transistor circuit has the same phase margin.

the actual OTA has more poles/parasitics than the simple model. so I should not put a final phase-margin number in the README until I run the full SKY130 circuit and measure it properly.

## integrated R and C are not tiny

1 MOhm on chip is a fairly long resistor. 3.3 pF MIM cap also takes noticeable area.

roughly this looks manageable, but the feedback components may take more area than the five MOSFETs themselves. parasitics from these parts can also move the final bandwidth.

this is something I will only know properly after layout + extraction.

## I was treating SKY130 like it was the only option

not really a circuit error but I got too fixed on SKY130. there are other open/public PDKs too (GF180MCU, IHP SG13G2 etc.).

for this project I am staying with SKY130 because the work is already scoped around it and it is enough for the TIA. for later projects I dont want to force the same PDK everywhere.

## biggest thing still missing

all the numbers above are planning / reference work. the real project evidence is not there yet.

still need to do:

- my own NMOS/PMOS characterization
- actual 5T OTA operating point
- actual DC gain and AC response
- TIA DC current sweep
- full AC/stability check
- transient test
- noise
- corners
- layout
- DRC
- LVS
- extraction
- post-layout simulation

so right now the project idea is figured out, but the actual IC work starts from device characterization.
