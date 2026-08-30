# Architecture — 100BASE-TX Ethernet Control Node

**A worksheet, not a design.** Every line below is a question this board has to
answer, and none of them is answered here. Nothing in this file is a
recommendation, and the order of the sections carries no preference.

The questions were derived from [the brief](../BRIEF.md) and from what this
board is meant to stress in the benchmark:

- RMII clock/data
- Ethernet magnetics
- diff pairs
- return paths

Those are the places where a wrong answer shows up in copper.

Answer them in this file as the design is made, each answer carrying the
evidence that supports it, and record the corresponding choice against its
`OPEN-nn` entry in [board/requirements.md](../board/requirements.md). An answer
without evidence is a guess wearing a document's clothes — and this benchmark is
allowed to refuse an unsupported claim rather than invent one.

## MCU / PHY partitioning and the RMII interface

- Which MCU provides RMII, and does its MAC support the intended control application's throughput and interrupt load?
- Which external PHY is selected, and does the brief's requirement for an *external* PHY rule out any otherwise-attractive integrated-MAC/PHY MCU under consideration?
- Which RMII signals are actually needed (TXD[1:0], TX_EN, RXD[1:0], CRS_DV, RX_ER, REF_CLK) and which does the chosen PHY omit or repurpose?
- How is MDIO/MDC routed, and which device is the management master?
- What reset sequencing does the PHY datasheet require relative to MCU reset and to clock availability?
- Are any RMII pins shared with MCU boot straps or with the expansion header, and what conflict does that create?

## 50 MHz RMII reference-clock architecture

- Which clocking arrangement do the selected MCU and PHY datasheets actually recommend — PHY-sourced REF_CLK, MCU-sourced REF_CLK, or a common external 50 MHz oscillator feeding both?
- What frequency tolerance, jitter, and duty-cycle limit does each device impose on the 50 MHz reference, and which is the binding one?
- If a discrete oscillator is used, what supply and output type does it need, and where does its return current flow?
- How is the reference clock distributed — point-to-point, or fanned out — and does it need series termination or a matched stub-free topology?
- What is the allowed skew between REF_CLK and the RMII data lines, and does the routing plan meet it?
- Does the clock net cross any plane boundary or change layers, and if it does, how is its return path kept continuous?

## Ethernet analog chain: PHY to magnetics to RJ45

- Is the magnetics integrated into the RJ45 or discrete, and what makes that choice 'convenient' in the sense the brief means?
- What termination does the PHY datasheet require on the TX/RX pairs, and where must those components sit relative to the PHY pins?
- How are the magnetics centre taps terminated on both the PHY side and the line side?
- How is the differential pair geometry maintained continuously from the PHY pins through the termination components to the magnetics?
- What isolation requirement applies across the magnetics, where does that requirement come from, and how is copper and plane treatment in that region decided against the brief's requirement for continuous reference planes?
- How does the RJ45 shield connect — solidly to ground, through an RC network, or isolated — and what evidence supports that decision?

## Four-layer stackup, impedance, and return paths

- What is the layer ordering, and which layers are the continuous reference planes the brief requires under RMII and high-speed signals?
- What differential impedance target is set for the Ethernet pairs, from what source, and what trace width/gap realises it in this specific stackup?
- Does every RMII and Ethernet trace have an uninterrupted plane directly adjacent for its whole length, including under connectors and component pads?
- Where are the plane splits (if any), and does any high-speed signal cross one?
- For any signal layer change on a high-speed net, where does the return current flow, and what keeps that path continuous?
- Do the fabricator's controlled-impedance rules for a 4-layer build support the chosen geometry and tolerance?

## PHY power and analog supply quieting

- Which PHY supplies are analog, which are digital, and what noise or ripple limit does the datasheet place on each?
- What separates the analog rail from the digital rails — a dedicated regulator, a ferrite/RC filter, or plane partitioning — and how does that interact with the requirement for continuous reference planes?
- Where does each decoupling capacitor sit relative to its PHY pin, and what is its return via placement?
- Does the PHY require an internal regulator bypass or a specific bulk-capacitance value?
- How do switching regulators (if used) place their loops and their switching noise relative to the PHY analog region?
- What measurement or simulation will demonstrate that 'quiet' was actually achieved rather than asserted?

## USB-C service power and debug

- What power role does the port take, on what basis is that role chosen, and how is that role configured at the connector?
- What current is drawn from the port, and does the on-board regulation chain support it at the worst-case rail load?
- Does the USB-C port carry a data interface, and if so what handles it — native MCU USB, a bridge device, or debug-only signalling?
- How is the USB-C connector shield handled relative to signal ground and to the RJ45 shield?
- What protection, if any, is applied to the port against ESD and reverse/over-voltage, and does that protection's capacitance affect any data lines it sits on?
- If the board also has a non-USB power source, how are the two inputs OR'd or isolated?

## Expansion header definition

- What signals does the header expose, and what is the design rationale for that set given this is a control node?
- How many pins, at what pitch, in what orientation, and what makes it 'small'?
- Does the header expose any RMII, clock, or high-speed signal, and if so what does that do to the return-path and stub requirements?
- Are power rails brought out, at what current limit, and what protects the board from a shorted or mis-wired accessory?
- Is the header keyed or otherwise protected against reversed insertion?
- How is the header placed so its routing does not force high-speed traces across plane discontinuities?

## Link and activity indication

- How many LEDs, showing what states, and driven by the PHY, the connector's own LEDs, or the MCU?
- If the RJ45 has integrated LEDs, what drive polarity and current does that connector require, and does the PHY match it?
- Where do the LED drive traces run, and do they cross the Ethernet pairs or the analog region?
- What current-limiting is used, and is LED current sourced from a rail whose noise could reach the PHY analog supply?
- Are the LEDs visible in the intended mechanical arrangement, given no enclosure is specified?

## Protection, shielding, and EMC

- What threats is the Ethernet port designed against — ESD at the connector, cable-borne surge, common-mode noise — and on what basis was that set chosen?
- If protection devices are added to the pairs, what is their capacitance and does it stay within what the PHY and 100BASE-TX signalling tolerate?
- Does any common-mode choke sit on the correct side of the magnetics for the topology chosen?
- How is common-mode current on the cable managed given the shield-ground decision?
- Which nets are the most likely radiators (the 50 MHz clock, RMII data, any switching regulator) and how is each contained?
- What guard/keepout policy applies around the connector region and the magnetics, and what justifies it?

## Mechanical outline and placement

- What board outline and dimensions are chosen, and what drives them, given the brief states none?
- Where do the RJ45 and USB-C sit relative to each other and to board edges, and does that placement serve a plausible use case?
- What mounting-hole pattern is used, and how do mounting-hole ground pads interact with the shield-ground decision?
- Does the placement put the PHY close enough to the magnetics to keep the differential run short, without crowding the analog supply filtering?
- What keepouts do the chosen connectors require, and are they respected in the layout?

## Manufacturability and sourcing

- Which fabricator capability class is targeted, and does the chosen impedance geometry fit its 4-layer standard stackup without a custom quote?
- What are the minimum trace/space and via sizes used, and how much margin is left against the process limit?
- Are all selected parts in production with adequate stock and no end-of-life notice?
- Is assembly single- or double-sided, and does the connector and magnetics selection support the intended process?
- What surface finish is chosen, and does the RJ45/USB-C mating or the magnetics selection constrain it?

## Bring-up, test, and evidence

- What test points are needed to observe the 50 MHz reference, RMII activity, PHY reset, and each supply rail?
- How is a working 100BASE-TX link demonstrated, and at what point in bring-up?
- What evidence — field-solver output, fabricator stackup, datasheet excerpts — will be cited to substantiate the impedance and clock-architecture claims?
- How will plane continuity under RMII and the differential pairs be checked and shown, rather than asserted?
- What is the failure-triage order if the link does not come up: clock, straps, MDIO, magnetics, or termination?
- Which checks run in the shared toolkit, and does any of it require board-specific logic that must instead live in this repository?

## Answers still owed

All of them. See [status.md](status.md).
