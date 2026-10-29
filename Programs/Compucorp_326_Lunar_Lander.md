# Lunar Lander for the Compucorp 326 Scientist

Author: Brett Hallen (original 324G version, 4/FEB/2026). Revised for the 326 using labels, conditional jumps and a subroutine.

**Needs a 326:** JUMP, BRANCH, LABEL, RETURN and the conditional jumps don't exist on the 324G. See the [324G notes](#if-youre-still-on-the-324g) at the end.

## What changed from the original

| Original (324G) | This version (326) |
|---|---|
| Registers pre-loaded by hand before every game | Step 001 sets up a new game by itself |
| 4 × START/STOP presses each turn, plus the burn | Height and velocity flash past with PAUSE; you only key in the burn and press START |
| Program ends after one turn and has to be restarted | `JUMP 1` loops back automatically |
| No landing check (you watch for height ≤ 0) | `JUMP −= 5` detects touchdown, judges the landing and shows **8.** (safe) or **505.** ("SOS", crash) |
| Burn can be larger than the remaining fuel, or negative | Burn is made positive (ABS) and capped at the fuel that's left |
| A burn must be keyed in every turn | Pressing START with no entry repeats the last burn (`JUMP +−=` entry test) |
| Landing judged on end-of-turn velocity (lander may be "underground") | Landing judged on the actual speed at height 0 |
| 35 steps of physics, R4 and R5 as scratch | 19 steps of physics using direct register arithmetic |

The physics is unchanged: Vnew = Vold + G − B, Hnew = Hold − (Vold + Vnew)/2. The shortcut uses A = G − B, so Vnew = Vold + A and Hnew = Hold + A/2 − Vnew.

## Registers

| Reg | Purpose | Reg | Purpose |
|---|---|---|---|
| 0 | Height H | 5 | Scratch (Vnew²) |
| 1 | Velocity V (down = +) | 6, 7 | Free |
| 2 | Fuel F | 8 | Safe landing speed (5) |
| 3 | Burn B (also "last burn") | 9 | Lunar gravity G (5) |
| 4 | Net acceleration A = G − B for this turn | . , CHG SIGN | Free |

## Labels

| Label | Purpose |
|---|---|
| (step 001) | New game: `JUMP START/STOP` gets here |
| 1 | Main loop: one turn per pass. Also an entry point for a custom game |
| 2 | **Subroutine:** show velocity (pause), leave fuel in the display, RETURN |
| 3 | Validate the burn |
| 4 | Physics |
| 5 | Touchdown: impact speed and the verdict |
| 6 | Safe landing |
| 7 | Game over: reuses subroutine 2 to show impact speed and fuel left |

## Program listing (118 of 160 steps)

Key names follow the 326 keyboard; `f` is the gold function key. Codes are from Appendix D of the manual. In LOAD mode, check each step against the code column.

| Step | Keys | Code | Comment |
|---|---|---|---|
| | **New game (step 001)** | | |
| 001 | `f` `CLEAR` | 137 | Clear all 12 registers (last burn = 0) |
| 002 | `5` | 005 | Starting height |
| 003 | `0` | 000 |  |
| 004 | `0` | 000 |  |
| 005 | `ST` | 300 | H → R0 |
| 006 | `0` | 000 |  |
| 007 | `5` | 005 | Starting velocity (down = +) |
| 008 | `0` | 000 |  |
| 009 | `ST` | 300 | V → R1 |
| 010 | `1` | 001 |  |
| 011 | `1` | 001 | Starting fuel |
| 012 | `2` | 002 |  |
| 013 | `0` | 000 |  |
| 014 | `ST` | 300 | F → R2 |
| 015 | `2` | 002 |  |
| 016 | `5` | 005 | Lunar gravity |
| 017 | `ST` | 300 | G → R9 … |
| 018 | `9` | 011 |  |
| 019 | `ST` | 300 | … and safe landing speed → R8 |
| 020 | `8` | 010 |  |
| | **Label 1 – main loop: show status, read burn** | | |
| 021 | `f` `LABEL` `1` | 201 | Start of each turn |
| 022 | `RCL` | 310 | Height |
| 023 | `0` | 000 |  |
| 024 | `f` `EXP` | 114 | PAUSE ~3 s to show height |
| 025 | `f` `BRANCH` | 360 | Subroutine: show velocity, leave fuel in display |
| 026 | `2` | 002 |  |
| 027 | `START/STOP` | 033 | STOP: key in burn, press START |
| 028 | `JUMP` `+` `-` `=` | 357 | Burn keyed in? → Label 3 |
| 029 | `3` | 003 |  |
| 030 | `RCL` | 310 | No entry: repeat last burn |
| 031 | `3` | 003 |  |
| 032 | `f` `LABEL` `3` | 203 | Validate burn |
| 033 | `f` `2` | 102 | ABS: negative burns not allowed |
| 034 | `ST` | 300 | B → R3 |
| 035 | `3` | 003 |  |
| 036 | `RCL` | 310 | Fuel … |
| 037 | `2` | 002 |  |
| 038 | `RCL` `-` | 312 | … minus burn |
| 039 | `3` | 003 |  |
| 040 | `JUMP` `+` `=` | 355 | Enough fuel? → Label 4 |
| 041 | `4` | 004 |  |
| 042 | `RCL` | 310 | Not enough: burn whatever is left |
| 043 | `2` | 002 |  |
| 044 | `ST` | 300 | B = F |
| 045 | `3` | 003 |  |
| | **Label 4 – physics** | | |
| 046 | `f` `LABEL` `4` | 204 |  |
| 047 | `RCL` | 310 |  |
| 048 | `3` | 003 |  |
| 049 | `ST` `-` | 302 | F = F − B |
| 050 | `2` | 002 |  |
| 051 | `RCL` | 310 |  |
| 052 | `9` | 011 |  |
| 053 | `RCL` `-` | 312 | E = A = G − B (net acceleration) |
| 054 | `3` | 003 |  |
| 055 | `ST` | 300 | A → R4 (for impact speed) |
| 056 | `4` | 004 |  |
| 057 | `ST` `+` | 301 | V = V + A |
| 058 | `1` | 001 |  |
| 059 | `÷` | 024 |  |
| 060 | `2` | 002 |  |
| 061 | `=` | 020 | E = A/2 |
| 062 | `RCL` `-` | 312 | E = A/2 − Vnew = −(Vold+Vnew)/2 |
| 063 | `1` | 001 |  |
| 064 | `ST` `+` | 301 | H = H − average velocity |
| 065 | `0` | 000 |  |
| 066 | `RCL` | 310 |  |
| 067 | `0` | 000 |  |
| 068 | `JUMP` `-` `=` | 356 | H ≤ 0? → touchdown, Label 5 |
| 069 | `5` | 005 |  |
| 070 | `JUMP` | 350 | Otherwise next turn |
| 071 | `1` | 001 |  |
| | **Label 5 – touchdown** | | |
| 072 | `f` `LABEL` `5` | 205 |  |
| 073 | `RCL` | 310 |  |
| 074 | `1` | 001 |  |
| 075 | `x²` | 162 | Vnew² |
| 076 | `ST` | 300 | → R5 |
| 077 | `5` | 005 |  |
| 078 | `RCL` | 310 | H (≤ 0, how far "underground") |
| 079 | `0` | 000 |  |
| 080 | `RCL` `×` | 313 | × A |
| 081 | `4` | 004 |  |
| 082 | `×` | 023 |  |
| 083 | `2` | 002 |  |
| 084 | `=` | 020 | E = 2·A·H |
| 085 | `RCL` `+` | 311 | E = Vnew² + 2·A·H = Vimpact² |
| 086 | `5` | 005 |  |
| 087 | `f` `2` | 102 | ABS (guards against rounding) |
| 088 | `√` | 062 | Impact speed |
| 089 | `ST` | 300 | → R1 |
| 090 | `1` | 001 |  |
| 091 | `RCL` `-` | 312 | Impact speed − safe limit |
| 092 | `8` | 010 |  |
| 093 | `JUMP` `-` `=` | 356 | ≤ 0 → safe, Label 6 |
| 094 | `6` | 006 |  |
| 095 | `5` | 005 | Crash: shows "505." (SOS) |
| 096 | `0` | 000 |  |
| 097 | `5` | 005 |  |
| 098 | `f` `9` | 111 | IDENTIFIER |
| 099 | `f` `EXP` | 114 | PAUSE |
| 100 | `JUMP` | 350 |  |
| 101 | `7` | 007 |  |
| 102 | `f` `LABEL` `6` | 206 | Safe landing |
| 103 | `8` | 010 | Shows "8." |
| 104 | `f` `9` | 111 | IDENTIFIER |
| 105 | `f` `EXP` | 114 | PAUSE |
| 106 | `f` `LABEL` `7` | 207 | Game over |
| 107 | `f` `BRANCH` | 360 | Show impact speed, then fuel left (= score) |
| 108 | `2` | 002 |  |
| 109 | `START/STOP` | 033 | STOP |
| 110 | `JUMP` | 350 | START again → new game |
| 111 | `START/STOP` | 033 | (JUMP START/STOP = go to step 001) |
| | **Label 2 – subroutine: show velocity and fuel** | | |
| 112 | `f` `LABEL` `2` | 202 |  |
| 113 | `RCL` | 310 | Velocity |
| 114 | `1` | 001 |  |
| 115 | `f` `EXP` | 114 | PAUSE |
| 116 | `RCL` | 310 | Fuel (left in display) |
| 117 | `2` | 002 |  |
| 118 | `f` `BACK` | 030 | RETURN (gold RETURN on BACK key) |

Notes on keying it in:

* `f LABEL n`, `JUMP + − =`, `ST −`, `RCL ×` and similar multi-key sequences load into a **single** step; the step counter doesn't move until the sequence is complete (manual p. 94).
* The register or label number after `ST`, `RCL`, `JUMP` or `BRANCH` is a separate step.
* `x²` is code 162 and `√` is 062. If unsure which key or shift you need, watch for the code in LOAD mode.

## How to play

1. Switch to RUN and press `JUMP` `START/STOP`, then `START/STOP`.
2. Each turn the **height** shows for about 3 s, then the **velocity** for about 3 s. The program then stops with **fuel** displayed.
3. Key in a burn and press `START/STOP`. To repeat the previous burn, just press `START/STOP` without keying anything (on the first turn the "previous" burn is 0).
4. On touchdown you see **8.** (safe) or **505.** (crash), then the impact speed, then the fuel left (your score).
5. Press `START/STOP` for a new game.

**Custom game:** in RUN mode store your own values in R0, R1, R2, R8 and R9, then press `JUMP` `1` `START/STOP`. This enters at Label 1 and skips the set-up at step 001.

**Test game** (useful for checking the keying): press START with no entry for 6 turns (free fall), then burn 73, then 2, then 7. After that, just press START for every remaining turn. You should touch down on turn 14 with an impact speed of 4.7958 and 3 fuel left. The heights shown before each burn are 500, 447.5, 390, 327.5, 260, 187.5, 110, 64, 50.5, 36.5, 24.5, 14.5, 6.5, 0.5.

## Things the manual warns about

* A label must be defined before it is used. Jumping to a missing label goes silently to step 001 (p. 95). A typo in a label therefore looks like the game restarting.
* If a label appears twice, only the one at the lowest step is used (p. 95).
* A subroutine must start with a LABEL and end with RETURN. BRANCH can be nested up to 6 levels. A RETURN without a BRANCH can hang the machine; recover with `JUMP` and any label (p. 107).
* Avoid a STOP while an arithmetic operation is pending (p. 127). Every STOP and PAUSE in this listing comes after `=` or a plain recall.
* For debugging: STEP mode runs one instruction per START press. At a stop, the **Look Ahead** feature shows the next instruction without running it (pp. 123–126).

## Ideas for further versions

* **Turn counter:** add `1 ST+ 6` in the physics block and show R6 at game over (3 steps).
* **Burn limit:** cap the burn at, say, 30 per turn, the same way the fuel cap works. This makes the game harder and stops one-shot "suicide burns".
* **Random start:** keep a seed in R7 and set `R7 = FRAC(R7 × 997 + π)` (`f 5` is FRACTION, `f 7` is π) each game. Use the result to vary the starting height or velocity.
* **Save to tape:** with a 392 drive, the program and a high-score register can be saved to tape.

## If you're still on the 324G

According to the Monroe 324 manual, the 324 has two independent 80-step programs and no jumps, labels or subroutines. It "makes decisions" by deliberately causing an error, e.g. 1/x of a register that has reached zero. The labels and subroutine above can't be used there, but these improvements carry over:

* **Direct register arithmetic exists on the 324** (`ST+ − × ÷ n` and `RCL+ − × ÷ n`, manual pp. 26–28). The physics block `RCL 9  RCL− 3  ST+ 1  ÷ 2 =  RCL− 1  ST+ 0` replaces steps 14–48 of the original. Then `RCL 3  ST− 2` replaces steps 49–56.
* Steps 18–19 (`RCL 4` straight after `ST 4`) are redundant, because the value is already in the display.
* The separate STOP at step 11 isn't needed. At the step-10 stop (fuel displayed) you can key in the burn and press START.
* **Touchdown halt, untested:** ending the turn with `RCL 0  √` should stop the program with an ERROR as soon as the height goes negative, using the same idea as the manual's 1/x trick. Check on the machine that √ of a negative number gives ERROR on the 324.
