# Compucorp 324G Component List

## ICs
Note:
- ACL = American Microsystems, Inc.
- TCL = Texas Instruments, Inc.
- The ACL and TCL chips are not interchangeable due to different pin numbering indicated by Axx/Txx pin numbers

| IC       |Pins   | Code   | Location        | Purpose                     |
|----------|-------|--------|-----------------|-----------------------------|
| TMC1871  |DIP-40 | TCL03  | Memory Board    | Memory addressing           |
| C2102    |DIP-16 |        | Memory Board    | 1k x 1-bit SRAM             |
| 8KR218   |DIP-28 |        | Memory Board    | 8kbit ROM                   |
| 8KR029   |DIP-28 |        | Memory Board    | 8kbit ROM                   |
| 8KR11A   |DIP-28 |        | Memory Board    | 8kbit ROM                   |
| 93L00    |DIP-16 |        | Memory Board    | 4-bit shift register        |
| SN74L75N |DIP-16 |        | Memory Board    | 4-bit bi-stable latch       |
| 2N4403   |3      |        | Memory Board    | PNP BJT on data bus         |
| TMC1872  |DIP-40 | TCL07  | Processor Board | Adder control               |
| TMC1866  |DIP-40 | TCL06  | Processor Board | Adder/entry                 |
| TMC1867  |DIP-40 | TCL05  | Processor Board | Index                       |
| TMC1870  |DIP-40 | TCL04  | Processor Board | Instruction control         |
| TMC1869  |DIP-40 | TCL02  | Scan Board      | Keyboard scanner            |
| TMC1884  |DIP-40 | TCL08  | Scan Board      | Display controller          |
| UHP480   |DIP-14 |        | Scan Board      |                             |
| 2N5400   |TO-92  |        | Scan Board      | PNP BJT for anodes?         |
| 2N4401   |TO-92  |        | Power Board     | NPN BJT                     |
| 2N5193   |TO-126 |        | Power Board     | PNP power BJT               |
| SN74L00N |DIP-14 |        | Power Board     | Quad nand                   |
| SN74L04N |DIP-14 |        | Power Board     | Hex inverter                |
| SN74L93N |DIP-14 |        | Power Board     | 4-bit binary ripple counter |
| 93L0059X |DIP-16 |        | Power Board     | 8-bit binary counter?       |

## Other Components
- Teledyne Kinetics Series K D401K028A 28-way ribbon cable connecting keyboard to Scan board ([possible replacement](https://sites.google.com/site/bobscalculatorsandsliderules/home/electronic-calculators/compucorp): 30-way 2.54mm/0.1" pitch, or two 14-way?)
- Burroughs Panaplex II BR 16252 16-digit display
- Teledyne Kinetics TKC B408U040 40-way interboard connectors
