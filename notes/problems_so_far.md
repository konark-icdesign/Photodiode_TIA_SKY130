# problems / changes so far

keeping this file because I dont want to only show the final clean circuit later. some assumptions already broke before even starting the actual transistor sizing.

## photodiode capacitance was not as simple as I first thought

I was looking at a real photodiode datasheet and saw capacitance of a few pF. first thought was just use that number directly.

problem: that capacitance was specified at a certain reverse bias. it is not automatically the capacitance I will get in my setup.

so for now I am using about 5 pF as a starting electrical model, then I will sweep it. exact photodiode can be locked later after the TIA itself works.

## 5 nA was too aggressive for V1

first range I was thinking about was around 5 nA to 500 nA.

with 1 MOhm feedback, 5 nA gives only about 5 mV output change. then OTA offset, mismatch, leakage and low frequency noise start becoming a big part of the signal.

I could go into chopping / auto-zero / much more careful leakage design, but that is not the point of the first version.

so V1 moved to about 50 nA to 500 nA.

if the real circuit later works below 50 nA I will measure it and then add it. not claiming it before that.

## feedback capacitor was not optional

in the first rough TIA model I basically had the detector capacitance + feedback resistor and the response was too peaky / badly damped.

then I swept Cf. small Cf gave more bandwidth but worse damping. bigger Cf calmed it down but reduced bandwidth.

around 3.3 pF with 1 MOhm puts the response close to the ~50 kHz region I want. this is why 3.3 pF is the starting value now.

still not final. actual transistor circuit can move this.

## first amplifier idea was too complicated

I first went toward a two-stage OTA, roughly 50-60 dB gain and a few MHz GBW.

then I checked whether I actually need that much gain for this TIA.

a public SKY130 5T OTA reference has around 53 V/V gain (~34.5 dB). with 1 MOhm feedback, the simple finite-gain estimate gives about 981 kOhm effective transimpedance, roughly 2% below ideal.

for V1 that is fine.

so instead of adding another gain stage, another pole and extra compensation just to make the circuit look bigger, I am starting with the 5T OTA.

if my own simulation shows it is not enough then I will change it. that would be a real reason to move to two-stage.

## some early phase margin numbers were not real SKY130 results

this was another thing I had to correct.

some early stability numbers came from a simplified OTA / loop model. useful for checking direction, but not proof of the real transistor circuit.

real OTA will have device capacitances, extra poles, layout parasitics etc.

so I am not putting a final phase margin claim in the main result table until the actual SKY130 circuit is running and I measure it properly.

## public reference result is not my project result

I used a public SKY130 5T OTA result as a sanity check because I needed a real transistor-level anchor instead of only hand assumptions.

that helped decide that the simple 5T architecture is worth trying.

but its gain/current/power are not my result.

I need to reproduce the operating point and AC response in my own setup before using any number as project evidence.

## actual tool setup is still a blocker right now

I have the first NMOS/PMOS sweep decks in the repo, but I have not committed run data yet.

the decks depend on a working SKY130 + ngspice environment and the correct PDK model path (`$PDK_ROOT/.../sky130.lib.spice`).

until I run them in the actual environment, I dont know if I will hit model-path/version/binning issues.

so the current files are test decks, not characterization results.

first thing to check when I run them:

- model library path is correct
- nfet/pfet subcircuit names match installed PDK
- sweep polarity for PMOS is correct
- current sign is plotted consistently
- W/L is inside a valid model bin
- ngspice actually converges across the whole sweep

if any of these break, I will keep the fix here instead of hiding it.

## integrated R and C are not tiny

1 MOhm on chip is a fairly long resistor. 3.3 pF MIM cap also takes noticeable area.

rough estimate says both are manageable, but the passives may take more area than the five MOSFETs themselves.

also their parasitics can shift the final bandwidth.

so even if schematic looks perfect, post-layout can still move the result. need extraction before calling it done.

## I was treating SKY130 like it was the only open option

not a circuit failure, but it was a project-planning mistake.

there are other public/open flows too: GF180MCU, IHP SG13G2, academic platforms like ASAP7/FreePDK45 etc.

for this TIA I am still staying with SKY130 because the project is already scoped around it and it is enough for the job.

for later projects I dont want to pick SKY130 automatically. process choice should have a reason.

## another thing I almost did wrong: making the repo look finished before doing the work

I already had a lot of pre-work calculations before opening this repo. dumping all of that as if it was completed project evidence would make the history useless.

so I am keeping the pre-work as planning/reference only and starting the actual repo work from device characterization.

I want the commits to show what really happens: first sweeps, bad assumptions, fixes, OTA, TIA, layout, DRC/LVS problems, extraction, then final comparison.

## biggest things still not proven

right now the project direction is figured out. actual IC evidence is still missing.

still need:

- my own NMOS/PMOS characterization
- gm and gm/Id data
- channel-length comparison
- actual 5T OTA operating point
- actual DC gain and AC response
- check output swing / headroom
- TIA DC current sweep
- full AC/stability check
- Cf sweep on the real transistor circuit
- detector capacitance sweep
- transient test
- noise
- TT/FF/SS + temperature checks
- layout
- DRC
- LVS
- extraction
- post-layout simulation
- pre-layout vs post-layout table

so at this point: project idea is not the problem anymore. next problem is getting the real device data and seeing where the first transistor-level assumptions fail.
