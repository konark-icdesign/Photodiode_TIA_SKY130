# prework checks

keeping the rough work here so I dont forget why I picked the starting values.

before making the repo I did a few simple system level checks. these are not final SKY130 results, just enough to stop guessing blindly.

## feedback network

started with:

- Rf = 1 MOhm
- Cin = about 5 pF
- Cf sweep from 0 to a few pF

with no/small Cf the rough model had too much peaking. around 2.2 to 3.3 pF looked much calmer.

1 MOhm + 3.3 pF also gives an RC pole around 48 kHz, which is close to the bandwidth I want anyway.

so current starting point is still:

- Rf = 1 MOhm
- Cf = 3.3 pF
- target BW around 50 kHz

not calling this final until the transistor level loop is simulated.

## current range

first thought was 5 nA to 500 nA.

5 nA with 1 MOhm is only about 5 mV. after looking at offset/noise/leakage this felt too aggressive for V1.

so I moved the starting range to 50 nA to 500 nA.

if the finished circuit works below 50 nA I will measure it later instead of promising it now.

## OTA choice

I first went toward a two stage OTA because I thought I needed 50-60 dB gain.

then I checked a simple public SKY130 5T OTA reference with about 53 V/V gain. using the finite gain estimate with 1 MOhm feedback gives roughly 981 kOhm effective transimpedance.

that is only around 2 percent below ideal, so I decided the 5T OTA is worth trying first.

rough closed loop reference model using that amplifier data gave about:

- Zt ~ 981 kOhm
- BW ~ 47 kHz
- around 0.94 V output at 50 nA
- around 1.38 V output at 500 nA
- 50 nA -> 500 nA step rise around 7 us
- settle around 13 us

again these are planning numbers, not results from this repo.

## stress check

I also varied gain, bandwidth, Rf, Cf, input capacitance and a few operating assumptions in a numerical sweep (5000 cases).

rough bandwidth stayed around 37 to 58 kHz in that check. that was useful because it showed the idea is not depending on one exact perfect value.

still need real TT/FF/SS and temperature runs later.

## physical size check

rough layout sanity check:

- 1 MOhm high-R poly is around 500 squares if I use ~2 kOhm/square as a rough number
- 3.3 pF MIM is around 1650 um^2 at ~2 fF/um^2

so the passives may take more area than the 5 MOS devices. not a problem, just something to keep in mind for layout.

## what counts as real from here

actual project results start when I run the committed SPICE decks and save the raw output.

next I need:

- Id/Vgs + Id/Vds data for NMOS and PMOS
- gm and gm/Id
- try a few channel lengths
- pick a bias region
- then build the 5T OTA
