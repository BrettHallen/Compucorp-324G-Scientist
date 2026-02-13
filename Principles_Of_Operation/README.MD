# Principles of Operation
From the Service Manual.<br>

Work in progress.<br>

## 2.3 Keyboard And Keyscanner Principles of Operation
The computer contains a highly sophisticated means of detecting and encoding switch closures.  Since all circuitry performing this operation is contained on the Display Drive board, with the exception of the switches themselves, most of the following discussion treats the keyboard and keyscanner portion of the Display Drive board as a single unit.<br>

### 2.3.1 Keyboard
All models use electrically identical keyboards, which contain 43 momentary action pushbutton switches and five slide switches. With the 392 Cassette Drive connected to the computer, the READ FROM TAPE and WRITE ON TAPE switches on the Cassette Drive keyboard are also part of the Keyscanning circuit. The keyboard insert covers switches not used by the computer.  Figure 2-9 is the schematic diagram of the keyboard.<br>

The keyscanner circuitry transmits eightsampling pu Iseson the input lines labelled with the decimal designators Y0, Yl, Y2, Y3, Y5, Y6, Y8, and Y9. A ninth pulse enters on line YA. A closed switch allows a sampling pulse to return to the keyscanner on one of the output lines. YA pulses passing through a closed slide switch return to the keyscanner via the lines marked AX1 through AXB. A closed key switch allows a pulse to return to the keyscanner on one of the lines labelled KX1 through KXD.<br>

The keyswitches operate by distorting the shape of a metal dome so it closes a circuit between an input and an output line. See Fig. 2-10. The dome is installed concave side down on a pair of concentric printed circuit lands. When the operator presses the key, the actuator presses downward against the dome, causing the center to bend toward the printed circuit board. When the key is fully depressed, the center of the dome contacts the inner printed circuit land, completing the connection between the input and output lines.<br>

![Fig. 2-10 Key Switch Construction](/Principles_Of_Operation/Fig2-10.jpeg)

The slide switches are comprised partly of contacts on the keyboard printed circuit board and partly of contacts on a movable assembly held captive by the actuator holder.<br>

Table 2-1 tabulates the keyswitch functions of the computer. Table 2-2 does the same for the slide switches, and Table 2-3 tabulates the keyswitch functions of the 392 Cassette Drive.<br>

### 2.3.2 Keyscanner Chip ACL-02/TCL-02

### 2.3.3 Key Switch Scanning

### 2.3.4 Switch Group A Scanning
During part of the routine, the computer executes the Read Switch Group A (RSWA) instruction.  Switch Group A consists of the five slide switches on the keyboard. When the RSWA command is executed, the YA pulse passes to the AX lines through the closed slide switches and into the Input Register, where it sets bits corresponding to the switches which are closed. The Input Register contents then shift serially through the Output Control, to IXDI, and are read into the
Index Register on the Control board.<br>

As a result of the Switch Group A settings, the computer modifies its operation according to microinstructions written into the ROM memory. For example, closing SS1 on a Scientist model affects only the trigonometric functions, causing the computer to interpret angle units as gradients rather than degrees. On the other hand, closing SS5 causes the content of program memory rather than data memory to be written on a peripheral device. See Table 2-2 for Switch
Group A functions.<br>

## 2.4 Display And Display Control Principles of Operation
Three assemblies have a major role in displaying the results of numeral entries or calculations.  They are the display assembly, the display interconnect and the display control portion of the Display Drive board.  The display interconnect simply provides the connections between the display control circuits and the display assembly.<br>

### 2.4.1 Display Assembly
The display assembly operates in a multiplexed mode wherein the cathode drive and decode logic are time-shared among all the digit positions.  The display has ten cathode connections: seven segment connections, a decimal point connection and two comma connections.  Each digit has a separate anode connection.  The characters are displayed by successively applying positive potentials to the anodes corresponding to digit positions while applying negative potentials to the appropriate cathode busses.  See Fig. 2-12 (Panaplex II Display Assembly) for the display connections and segment configuration.

![Fig. 2-12 (Panaplex II Display Assembly)](/Principles_Of_Operation/Fig2-12.jpeg)

### 2.4.2 Display Encoder Chip ACL-08/TCL-08
Display Encoder chips come in two versions: an ACL-08 manufactured by American Microcircuits, Inc., and a TCL-08 manufactured by Texas Instruments, Inc.  The chips operate identically, but are not interchangeable because the pin connections differ.  However a Display Drive board using the ACL-8 may replace a Display Drive board using the TCL-08 and vice versa.  Refer to the display control schematic, Fig. 2-13, for the following discussion.<br>

Control transmits control instructions via CTRL to the Next Instruction Register in the Display Encoder chip.  After an instruction has entered the register, it transfers in parallel to the Execution Register, where the Instruction Decode Logic converts the instruction to control signals for the other logic circuits within the chip.  The Instruction Decode Logic also receives inputs from the sixteen-bit control word store and an external timing signal, C16P.  While the instruction in the Execution Register is being decoded and executed, a new instruction is being shifted into the Next Instruction Register, eliminating the need for the logic to wait while a new instruction shifts to the chip.<br>

The sixteen-digit Display Register stores the number to be displayed and provides an input to the Anode Time, Comma Comparator.  The Anode Time, Comma Comparator controls the timing of Anode Select Register and determines the placement of commas and the decimal point in the display.  The Display Register transfers the number, one digit at a time, to the Segment Encoder.<br>

### 2.4.3 Display Control Operational Description
When the Display Encoder chip receives an output instruction from Control, it enables cathode and anode select lines. The following example is for a Scientist model displaying the number "67" with the decimal point set at 0.<br>

The Anode Select Register enables the anode select lines in the following sequence: /DA4, /DA7, /DA10, /DA13, /DA0, /DA3, /DA6, /DA9, /DA12, /DA15, /DA2, /DA5, /DA8, /DA11, /DA14, and /DA1.  The Anode Select Register first enables DA3 to display the "6". The enable signal, approximately -8V, turns on transistor Q16. With Q16 on, V<sub>SS</sub> is applied through Q16 to AN4. At the same time the Segment Encoder selects the segments to display the "6".<br>

When a specific segment is unselected, the select line from the Display Encoder chip remains at -8V. The -8V forward-biases the cathode drive transistor for that segment, placing the collector at nearly V<sub>SS</sub>. An isolation diode applies -120V to the cathode buss of the display assembly. The -120V is less than the ionisation potential of the glow tube, so the segment remains off.<br>

When the segment is selected, that line from the Display Encoder chip rises to V<sub>SS</sub>, turning off the drive transistor. A coupling capacitor couples this transistion to the display cathode connection, superimposing the -60V on the -120V already present. At the same time, the anode in the selected digit position rises from -38V to V<sub>SS</sub> as a result of the anode drive transistor turning on. The potential across the anode and the selected cathode segment
becomes approximately 180V, sufficient to ionize the glow tube.<br>

The coupling capacitor discharges almost immediately after the glow tube ionises; however, the -120V applied through the diode is sufficient to maintain the glow as long as the anode remains at V<sub>SS</sub>.<br>

The first digit position enabled being /DA3, anode transistor Q16 turns on as the Segment Encoder selects the appropriate cathodes to display the numeric "6": S<sub>a</sub>, S<sub>c</sub>, S<sub>d</sub>, S<sub>e</sub>, S<sub>f</sub>, and S<sub>g</sub>. These segments turn on at display position AN4, and remain on while this position is enabled.<br>

At the end of the digit time, /DA3 returns to V<sub>SS</sub>, blanking the digit. The Anode Select Register advances through the sequence listed above until it comes to the next digit to be displayed, DA2.<br>

Since the character for /DA2 is a numeric "7", the Segment Encoder selects cathodes S<sub>a</sub>, S<sub>b</sub>, and S<sub>c</sub> as Q3 applies to anode AN3.<br>

The last position enabled is /DA1. Q2 applies V<sub>SS</sub> to AN2 as the Segment Encoder selects the DP cathode.<br>

Each character remains lighted only a small fraction of the time; however, the cycle repeats frequently enough that the display appears to glow continuously.<br>

