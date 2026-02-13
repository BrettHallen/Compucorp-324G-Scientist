# Principles of Operation
From the Service Manual.<br>

Work in progress.<br>

## Display And Display Control Principles of Operation
Three assemblies have a major role in displaying the results of numeral entries or calculations.  They are the display assembly, the display interconnect and the display control portion of the Display Drive board.  The display interconnect simply provides the connections between the display control circuits and the display assembly.

### Display Assembly
The display assembly operates in a multiplexed mode wherein the cathode drive and decode logic are time-shared among all the digit positions.  The display has ten cathode connections: seven segment connections, a decimal point connection and two comma connections.  Each digit has a separate anode connection.  The characters are displayed by successively applying positive potentials to the anodes corresponding to digit positions while applying negative potentials to the appropriate cathode busses.  See Fig. 2-12 (Panaplex II Display Assembly) for the display connections and segment configuration.

![Fig. 2-12 (Panaplex II Display Assembly)](/Principles_Of_Operation/Fig2-12.jpeg)

### Display Encoder Chip ACL-08/TCL-08
Display Encoder chips come in two versions: an ACL-08 manufactured by American Microcircuits, Inc., and a TCL-08 manufactured by Texas Instruments, Inc.  The chips operate identically, but are not interchangeable because the pin connections differ.  However a Display Drive board using the ACL-8 may replace a Display Drive board using the TCL-08 and vice versa.  Refer to the display control schematic, Fig. 2-13, for the following discussion.<br>

Control transmits control instructions via CTRL to the Next Instruction Register in the Display Encoder chip.  After an instruction has entered the register, it transfers in parallel to the Execution Register, where the Instruction Decode Logic converts the instruction to control signals for the other logic circuits within the chip.  The Instruction Decode Logic also receives inputs from the sixteen-bit control word store and an external timing signal, C16P.  While the instruction in the Execution Register is being decoded and executed, a new instruction is being shifted into the Next Instruction Register, eliminating the need for the logic to wait while a new instruction shifts to the chip.

The sixteen-digit Display Register stores the number to be displayed and provides an input to the Anode Time, Comma Comparator.  The Anode Time, Comma Comparator controls the timing of Anode Select Register and determines the placement of commas and the decimal point in the display.  The Display Register transfers the number, one digit at a time, to the Segment Encoder.
