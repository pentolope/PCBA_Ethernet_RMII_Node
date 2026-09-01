# PCBA_Ethernet_RMII_Node — 100BASE-TX Ethernet Control Node
## Design brief

Create a 100BASE-TX Ethernet control board using an MCU with RMII and an external PHY. Provide an RJ45 with integrated magnetics if convenient, link/activity LEDs, USB-C for service power/debug, and a small expansion header. Use a 50 MHz RMII reference-clock architecture recommended by the selected devices. Route Ethernet pairs as controlled differential pairs from PHY to magnetics, keep PHY analog supplies quiet, and maintain continuous reference planes under RMII/high-speed signals. Four layers required.

## Functional requirements

- Link up at 100BASE-TX with auto-negotiation, with no host action beyond power and cable.
- Strapping shall leave the PHY in a valid address, mode and clock state before firmware runs; the MCU shall reach its management interface where one exists.

## Power and rails

- All rails shall derive from USB-C VBUS (5 V nominal) alone, at the tolerance and current the chosen MCU and PHY need, with PHY reset held until they are in regulation.
- CC pins shall be sink-terminated for the advertised current; VBUS, CC and USB data pins shall carry ESD protection.
- PHY analog supplies shall be quieted by supply-path filtering, never by splitting or slotting a reference plane.

## Ethernet front end

- The MDI shall be transformer-isolated to the 1500 Vrms IEEE 802.3 requires, magnetics in the jack or separate, with nothing crossing that boundary.
- Common-mode termination, unused pairs and any shield return shall be defined by the chosen magnetics and connector.
- The PHY shall sit so MDI routing to the magnetics is short and within that PHY's published layout guidance.

## RMII interface and clocking

- Exactly one 50 MHz reference clock shall serve both MAC and PHY, in whichever architecture the chosen devices recommend.
- The clock shall hold ±50 ppm over range and life as 100BASE-TX requires; REF_CLK shall run stub-free and terminated to the trace impedance.
- RMII data and control shall meet setup and hold at both receivers from datasheet numbers, with the budget and length limits recorded.

## Layout and signal integrity

- Four copper layers, with stack-up and impedance targets stated on the fabrication drawing.
- MDI pairs shall be 100 Ω controlled pairs, PHY to magnetics on one layer where possible, ground-stitched at any transition.
- MDI, RMII and REF_CLK shall run over unbroken reference plane end to end; a crossed split or void is a defect.
- All layers shall be clear of copper between magnetics and jack contacts.

## Connectors, indicators and expansion

- External interfaces shall be limited to the RJ45, the USB-C receptacle and the expansion header.
- Link and activity LEDs shall be visible with the board mounted, and each LED's meaning documented.
- The header shall carry ground, a defined supply rail and MCU I/O, and shall not multiplex RMII, REF_CLK or PHY control.

## Test and bring-up

- The MCU shall be programmable on-board without removing parts, by a documented access path.
- Every rail shall have a probe point with a nearby ground return, and REF_CLK shall be probeable assembled.

## Open choices

- MCU: any part with an RMII MAC, I/O for the header, supplies compatible with the PHY, and USB-C debug reach.
- PHY: any RMII 100BASE-TX PHY with published layout guidance, and the clock architecture the pair calls for.
- Integrated-magnetics jack versus plain jack plus discrete magnetics, the USB-C debug transport, and header pitch and signal mix.
