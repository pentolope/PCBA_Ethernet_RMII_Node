# Sources — 100BASE-TX Ethernet Control Node

The evidence this board's design will have to cite. **Classes of document, not
documents:** the specific parts are not chosen yet, so naming a datasheet here
would be choosing one.

A number that reaches the board carries its provenance: source, document id or
URL, retrieval date, units, and the condition it applies under. A number without
that is not evidence, and no live network lookup may change a validation or
release result.

| Kind of source | What the design needs from it |
|---|---|
| Ethernet PHY datasheet and application notes | Fixes the RMII pin set, the recommended 50 MHz reference-clock architecture the brief defers to, PHY address/mode strapping, MDIO behaviour, reset sequencing, required TX/RX termination, and the analog supply noise limits behind 'keep PHY analog supplies quiet'. |
| MCU datasheet and reference manual | Establishes RMII pin mapping and alternate-function conflicts, MAC configuration, reference-clock input requirements and tolerance, boot/strap pins, package and pinout for placement, and what the debug interface can be. |
| RMII interface specification | The brief names RMII and a 50 MHz reference clock explicitly; the specification defines the signal set, which signals are optional, the clock definition, and the timing budget that the length/skew plan must fit inside. |
| 100BASE-TX / IEEE 802.3 clauses for the physical layer | The brief fixes 100BASE-TX; the standard bounds what the PHY-to-magnetics-to-RJ45 chain must deliver, including isolation and signalling requirements the analog front end has to satisfy. |
| Magnetics or integrated-magnetics RJ45 datasheet | Needed to resolve the 'integrated magnetics if convenient' choice: turns ratio, insertion and return loss, isolation rating, centre-tap and termination pinout, integrated-LED drive requirements, and mechanical keepouts. |
| USB Type-C connector and power-delivery/sink configuration documentation | The brief requires a USB-C port for service power and debug; the receptacle pinout and the port's power-role configuration — whichever role and configuration method is chosen — determine whether the port actually delivers power and what current is available. |
| Fabricator capability and 4-layer stackup documentation | The brief fixes four layers but no stackup; the fabricator's standard 4-layer stackups, dielectric thicknesses, copper weights, minimum trace/space, and impedance-control tolerance constrain every geometry choice. |
| Impedance / field-solver calculation output for the chosen stackup | Turns the brief's 'controlled differential pairs' from an assertion into a number: the trace width, gap, and reference layer that actually realise the PHY's required differential impedance in this specific build. |
| Power regulator and passive component datasheets | Supports the rail architecture — dropout, output noise and PSRR, switching frequency, thermal data — and in particular substantiates the separation and filtering that keeps the PHY analog rail quiet. |
| ESD / TVS / common-mode choke datasheets | Any protection added to the Ethernet pairs or the USB-C port must have its line capacitance and clamping behaviour shown compatible with 100BASE-TX signalling rather than assumed. |
| Connector and header mechanical drawings plus land-pattern guidance | The RJ45, USB-C receptacle, and expansion header footprints, keepouts, and mounting features must be verified against the manufacturer drawing and an accepted land-pattern method, since the brief fixes no mechanical envelope. |
| Distributor stock, pricing, and lifecycle data | The brief names no part, so every device is a design decision that has to be justified as available and in production at the time of the design. |

## Recording a source, once one is chosen

Replace the class with the actual document — manufacturer, part number, revision
and date — and state the fact taken from it, in the units the document uses.
Keep the class row: it says why the document was needed.

JLCPCB-wide process limits are **not** recorded here. They live in the toolkit's
`profiles/jlcpcb/`, with their own provenance; this board records only its own
tighter targets and its own selected options. A limit copied into two places is
a rival threshold, and the toolkit has a gate that says so.
