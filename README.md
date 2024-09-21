# pcb_design

# Best Practices

## Schematics
1. Flow from left to right, top to bottom
2. Main power rails shall be named like so: `12V0`, `3V3`, `N5V0`. No decimals. Negative voltages prefixed with `N`.
3. Differential pairs shall have the `_P` and _N` suffixes.
4. Active low logic signals shall be prefixed `N_`, i.e. `N_EN`.

## Symbols
1. Group GND pins on the bottom right
2. Power pins go on the top left
3. Break out ALL the pins individually. ICs exceeding 100 pins, such as FPGAs, should be split into sub-parts.
4. Inputs on the left, outputs on the right
5. Try not to include a block diagram in the symbol. It blows up the size.

## Footprints
1. Don't put vias in your footprints as any custom vias may get in the way of consolidating vias for manufacturability
2. 5 mil wide silkscreen and courtyard
3. Origin of component should be the physical XY midpoint, so rotating the component becomes easy

## Layout
1. Define size, shape, and mechanical features prior to starting layout
2. Start with a small layer count then increase if you need it
3. You don't need GND between every single power plane. Just need to make sure all signal layers have at least 1 GND reference
4. Soldermask affects RF transmission line fields, use at own risk
5. Place the components such that their footprint origins are on a regular grid. I prefer 5 mil. Don't just put the pad center on the grid. Pads may not be symmetric about the center.
6. Places vias on a 5 mil grid unless absolutely necessary. Exceptions include vias that help transition the RF GCPW to the IC pads, and tight BGA breakouts.

## Design for Test
1. Always leave exposed copper pads for voltage test points, including GND, and keep them away from high density power circuits that are prone to shorting out if probed directly
2. Leave exposed GND copper close to signal and power nets inorder to facilitate oscilloscope ground spring probing
3. Make silkscreen labels at minimum 0.1" tall text, preferably 0.25" or larger. Make it bigger than you'd want to, and it *might* be legible
4. At least one status LED on each microcontroller or FPGA, preferably two (one for heartbeat and one for other status)
5. Arrange capacitors so that their power nets are facing the same direction, avoids shorting things while probing
6. Don't forget pullup resistors on I2C lines...
7. Put series 0 ohm jumpers on digital lines so you can add termination resistors later of your choosing if SI is bad
8. Put DNP pullups/pulldowns for signals you may want to configure
9. Rounded edges so you don't cut yourself when handling the board
10. At least 2 mounting holes (sized for #4 or larger hardware) so you can mount it if you need to
11. Plate your mounting holes and tie them to GND. They become free GND test points, and plated holes are easier to design EMI shield lands around them than non plated holes (at least in Altium)
12. Ensure your connectors are robustly mounted to the PCB and will not rip off. Especially critical for RF connectors. Through hole posts are your friend.
13. Leave several DNP shunt capacitor locations on buck regulators (both input and output)

## Design for Manufacturing
1. Check PCB fab shop capabilities before starting. Preferably have a matrix of their capabilities handy so you can design conservatively. Consider things like via aspect ratio, board thickness, min. trace/space, soldermask web thickness, soldermask to silkscreen clearance, max board size, max board thickness, available stackup materials, ability to process RF materials such as Rogers 4350 and Isola MT40, copper thickness, via fill requirements, max via density, etc.
2. Consider how to efficiencly fit your board into a standard panel - consider panelization efficiency.
3. Use soft termination capacitors above 1210 in size otherwise you risk cracking them with board reflow.
4. Don't put capacitors close to mounting holes. You can mechanically shock them by putting the fastener on them.
5. Thermal reliefs on passives GND pads
6. Thermal reliefs on through hole connectors and components
7. Solder paste windowing on large belly pads - follow manufacturer recommendation. If no recommendation, do 70% area windowing
8. ENIG has excellent solderability, flatness, and does not tarnish, use for most things
9. Class 2 calls for a minimum 5 mil annular ring (10/20 vias) while class 3 calls for a minimum 7 mil annular ring (10/24 vias)
10. Consolidate via types - the fewer the number of unique vias, the faster the board will be to make

# Board Review Checklists

## Schematic
1. Check nets for duplicate nets
2. Ensure all floating pins are intentional - leave a note and put an "x" on them

## Layout
1. After generating files, always perform an independent check by looking at them with a gerber / ODB++ viewer (like ViewMate) to catch any errors.
2. Decoupling caps are placed in order of smallest value closest to IC
3. Check controlled impedance (including RF) line trace/space meet targeted impedances
4. Return vias on high speed digital
5. lambda/8 or tighter spacing on GND fence vias for GCPW or stripline routing

## Fabrication Drawing
1. Board view with basic dimensions shown
2. Board view with datums including stackup height tolerance called out
4. Drill table exists with callouts to special vias
5. Stackup table exists with callouts to stackup height
6. Controlled impedance traces called out
7. Note exists calling out which IPC class to manufacture the board to
8. Board finish called out
9. Soldermask color and silkscreen color called out
10. Pin and slot called out, if exists

## Assembly Drawing
1. Type of solder paste to use called out
2. Mounting hole size
3. Large components that require staking called out


