# Digital Logic Trainer
 
A virtual digital logic lab bench that runs in the browser. Place TTL chips on a breadboard, wire them to switches and LEDs, and watch the circuit work, the way you would on a physical trainer board.
 
It is a single HTML file with no dependencies, no build step and no backend.
 

 
## Why it exists
 
Lab manuals for digital logic assume you have a trainer board in front of you. This gives you the same workflow on any phone or laptop: power, ground, switches, LEDs, ICs and jumper wires, with a checker that points out the usual wiring mistakes.
 
## Features
 
- **Breadboard** with 60 columns, two halves split by the center gap, and four power rails. Holes in a column are connected the way a real board connects them.
- **Trainer panel** with power switch, +5V and GND jacks, 16 data switches, 16 LEDs, two BCD displays, two pulse buttons (with inverted outputs) and a clock output from 0.5 to 50 Hz.
- **IC library** (listed below). Chips are placed straddling the center gap, with pin 1 at the bottom left, and can be dragged sideways.
- **Jumper wires** drawn by tapping two points. Wire colors are picked automatically (red for +5V, black for ground) or chosen by hand.
- **Live pinout** for the selected chip, showing each pin name and its current level (1, 0 or unconnected).
- **Wiring check** that reports chips without power, +5V shorted to GND, outputs driving opposite levels on one node, oscillating feedback loops, and unconnected inputs.
- **Mobile friendly**: pinch to zoom, swipe to pan, and a toolbar that scrolls sideways.
- **Demo circuit** (7408 AND gate) to confirm everything works.
## Getting started
 
1. Open `logic-trainer.html` in any modern browser. No install needed.
2. Press **Load demo**. Flip SW0 and SW1 and watch LED 0.
3. To build your own circuit, pick a chip, press **Add IC**, then wire it:
   - Connect **+5V** to a `+` rail and **GND** to a `-` rail.
   - Wire the rails to the chip's VCC and GND pins.
   - Wire switches to inputs and outputs to LEDs.
A chip only runs when its VCC pin is on +5V and its GND pin is on ground, as on real hardware.
 
### Controls
 
| Action | How |
| --- | --- |
| Start or finish a wire | Tap a hole or jack, then tap the other end |
| Cancel a wire | Tap the same point again, or press Esc |
| Select a wire or IC | Tap it |
| Delete the selection | **Delete selected** button, or the Delete key |
| Move an IC | Drag it sideways |
| Toggle a data switch | Tap it |
| Pulse buttons | Press and hold (HIGH while pressed) |
| Zoom | Pinch, the + and - buttons, or Ctrl + scroll. **Fit** shows the whole trainer |
 
## Supported ICs
 
| Group | Parts |
| --- | --- |
| Gates | 7400, 7402, 7404, 7407, 7408, 7410, 7411, 7414, 7420, 7421, 7427, 7430, 7432, 7486 |
| Flip-flops | 7474 (D), 7476 (JK), 74112 (JK) |
| Counters and registers | 7490, 7493, 74161, 74163, 74194 |
| Decoders and multiplexers | 7447, 7448, 74138, 74139, 74151, 74153, 74157 |
| Arithmetic | 7483 (adder), 7485 (comparator) |
| Displays | 7-segment, common anode and common cathode |
 
LS and HC variants share the same pinouts, so they are covered by the same entries. Select any placed chip to see its pinout and a one-line description.
 
## How the simulation works
 
- Every hole, rail and jack belongs to a net. Wires merge nets, and the breadboard's column and rail connections are built in.
- Each source (switch, clock, pulse button, supply) and each chip output is a driver on a net. A net with drivers at both 0 and 1 is reported as a conflict.
- Chips are evaluated repeatedly until nothing changes, which lets feedback circuits such as SR latches settle. Edge-triggered parts all sample their inputs at the same moment, so synchronous designs behave correctly.
- Unconnected TTL inputs read HIGH, as on real chips.
- Flip-flops and counters start at 0 when power is applied and reset when power is lost.
## Limitations
 
- This is a digital simulator. There is no analog modeling: no function generator, potentiometers or variable supplies, and no resistors, capacitors or timing delays.
- 7-segment displays light without a series resistor.
- The 7476 is modeled as a falling-edge flip-flop, which is how most lab circuits use it. Master-slave behavior while the clock is high is not simulated.
- Saving your circuit between sessions relies on the Claude artifact storage API. In a plain browser tab the circuit is not saved when you close the page.
- Touch gestures have not been tested on every device.
## Project layout
 
Everything lives in `logic-trainer.html`:
 
- **IC library:** pinouts and behavior for each chip.
- **Static drawing:** the trainer panel and breadboard, built as SVG.
- **Simulation:** net building, driver resolution and the settle loop.
- **Interaction:** wiring, dragging, zoom and the toolbar.
To add a chip, define its pinout and a `drive` function (plus a `seq` function if it has memory) in the IC library section, and it appears in the part list automatically.
 
## Credits
 
Developed by Hamim.
