# Requirements — 100BASE-TX Ethernet Control Node

Two lists. The difference between them is the whole point of this file.

A **fixed requirement** is something [BRIEF.md](../BRIEF.md) asks for. Each one
below quotes the brief text that substantiates it; if a statement cannot be
quoted, it is not a requirement here. An **open decision** is a choice the brief
deliberately left to whoever designs this board.

> Missing details are design freedom, not permission to fabricate unstated user
> requirements.

Promoting a decision into a requirement is the failure this file exists to
prevent. Record a choice under the decision it answers, with the reasoning that
made it — never by adding it to the list above.

Bound to `BRIEF.md` SHA-256 `0ff1c2a7403e522f1f1d15b41913e390ef8422d07f367d815823aaed69807917`.

## Fixed by the brief

### REQ-01 — The board is a 100BASE-TX Ethernet control board.

Brief text:

> Create a 100BASE-TX Ethernet control board using an MCU with RMII and an external PHY.

### REQ-02 — The design uses an MCU that provides an RMII interface.

Brief text:

> using an MCU with RMII and an external PHY. Provide an RJ45

### REQ-03 — The Ethernet PHY is external to the MCU (an integrated-PHY MCU does not satisfy the brief).

Brief text:

> an MCU with RMII and an external PHY. Provide an RJ45 with integrated magnetics if convenient

### REQ-04 — An RJ45 is provided; magnetics integrated into it are asked for on the stated condition 'if convenient', so integration is conditional, not mandated, and the scaffold takes no position on which way it should go.

Brief text:

> Provide an RJ45 with integrated magnetics if convenient, link/activity LEDs

### REQ-05 — Link and activity LEDs are provided.

Brief text:

> if convenient, link/activity LEDs, USB-C for service power/debug

### REQ-06 — A USB-C port is provided, serving service power and debug.

Brief text:

> link/activity LEDs, USB-C for service power/debug, and a small expansion header.

### REQ-07 — A small expansion header is provided.

Brief text:

> USB-C for service power/debug, and a small expansion header.

### REQ-08 — The RMII reference clock is a 50 MHz architecture, and the specific architecture must be the one recommended by the devices actually selected.

Brief text:

> Use a 50 MHz RMII reference-clock architecture recommended by the selected devices.

### REQ-09 — The Ethernet pairs are routed as controlled differential pairs over the whole run from PHY to magnetics.

Brief text:

> Route Ethernet pairs as controlled differential pairs from PHY to magnetics

### REQ-10 — The PHY analog supplies are kept quiet.

Brief text:

> from PHY to magnetics, keep PHY analog supplies quiet, and maintain continuous reference planes

### REQ-11 — Reference planes are continuous beneath RMII and other high-speed signals.

Brief text:

> maintain continuous reference planes under RMII/high-speed signals.

### REQ-12 — The board is four layers.

Brief text:

> under RMII/high-speed signals. Four layers required.

### REQ-13 — Stated brief requirements are authoritative; open choices must be made and documented as engineering decisions, not converted into invented user requirements.

Brief text:

> Treat stated requirements as authoritative; where the brief leaves choices open, make and document reasonable engineering decisions rather than inventing hidden user requirements.

### REQ-14 — This repository stays a consumer of the shared PCBA_AutoDesignAndTest toolkit; board-specific logic must not accumulate inside the toolkit.

Brief text:

> The repository should remain a consumer of the shared `PCBA_AutoDesignAndTest` toolkit rather than accumulating board-specific logic in the toolkit.

## Open — the design agent decides

### OPEN-01 — Which MCU is used — family, core, package, memory, peripheral set, and how much headroom the control application needs.

The brief asks only for 'an MCU with RMII'. No vendor, family, package, or resource figure appears anywhere in the brief or metadata.

*Decision:* **not yet made.**

### OPEN-02 — Which external Ethernet PHY is used, and in what package and supply configuration.

The brief names only 'an external PHY'. Every downstream constraint the brief imposes (clock architecture, analog supply quieting, MDIO/strapping) depends on this choice, which the brief leaves entirely to the design agent.

*Decision:* **not yet made.**

### OPEN-03 — The 50 MHz RMII reference-clock topology: which device sources the clock, whether a discrete oscillator is used, and how the clock is distributed to the devices that need it and terminated.

The brief fixes 50 MHz and fixes that the architecture be 'recommended by the selected devices' — which means the topology is undetermined until OPEN-01 and OPEN-02 are resolved and their datasheets are read.

*Decision:* **not yet made.**

### OPEN-04 — Integrated-magnetics RJ45 versus a plain RJ45 with discrete magnetics, and the associated isolation/creepage handling.

The brief hedges with 'if convenient', explicitly declining to mandate integration. It gives no part number, no isolation rating, and no availability constraint to decide it on.

*Decision:* **not yet made.**

### OPEN-05 — The magnetics-side network: centre-tap treatment, the line-side termination network and its topology, and how the RJ45 shield and any chassis ground relate to signal ground.

The brief mentions magnetics only as a routing endpoint. It is silent on termination topology, shield bonding, and chassis-ground strategy, so the network follows from the magnetics datasheet and the designer's reasoning, not from this scaffold.

*Decision:* **not yet made.**

### OPEN-06 — Link/activity LED count, colours, placement (in-connector versus board-mounted), and whether they are driven by the PHY, the MCU, or link hardware.

The brief states only 'link/activity LEDs'. It fixes neither quantity, colour, drive polarity, nor which device drives them.

*Decision:* **not yet made.**

### OPEN-07 — The USB-C port's electrical role: what power role the port takes, how that role is configured at the connector, what current budget it is designed to, and whether the connector also carries a USB data interface or only debug signalling.

The brief says 'USB-C for service power/debug' and stops there — no power role, no power level, no data role, and no negotiation or configuration method is stated.

*Decision:* **not yet made.**

### OPEN-08 — How the MCU is programmed and debugged — the physical debug interface, and whether it is exposed on the USB-C port, on the expansion header, on test pads, or on a dedicated connector.

The brief attaches 'debug' to the USB-C port but never names a programming or debug interface, so the mechanism is unfixed.

*Decision:* **not yet made.**

### OPEN-09 — Expansion header pinout, signal set, pin count, pitch, gender, and location.

The brief asks for 'a small expansion header' with no signal list, no pin count, no pitch, and no definition of 'small'.

*Decision:* **not yet made.**

### OPEN-10 — Power architecture: what supplies the board in normal operation besides USB-C service power, the rail voltages, the regulator topologies, and how the PHY analog rail is separated from digital rails.

The brief imposes an outcome ('keep PHY analog supplies quiet') but names no rail, no regulator type, no filtering method, and no primary power input other than USB-C service power.

*Decision:* **not yet made.**

### OPEN-11 — The four-layer stackup: layer ordering and roles, dielectric thicknesses, copper weights, and which layers carry the continuous reference planes the brief requires.

The brief fixes the layer count at four and requires plane continuity, but states nothing about stackup ordering, materials, or thicknesses.

*Decision:* **not yet made.**

### OPEN-12 — The differential impedance target for the Ethernet pairs and the single-ended impedance treatment (if any) for RMII, plus the trace geometry that realises them in the chosen stackup.

The brief requires 'controlled differential pairs' but states no impedance value, tolerance, or geometry — those follow from the stackup and the PHY's requirements, neither of which is fixed.

*Decision:* **not yet made.**

### OPEN-13 — The RMII timing/skew budget: allowed trace-length mismatch, total lengths, and layer-change and return-path policy for RMII clock and data.

The brief lists RMII clock/data as a stressor and requires continuous reference planes, but gives no length, skew, or timing number and prescribes no layer-change or stitching technique; these must come from the selected devices' datasheets and the designer's own reasoning.

*Decision:* **not yet made.**

### OPEN-14 — Protection strategy for the Ethernet port, the USB-C port, and the expansion header — whether protection is added at all, and if so ESD, surge, and common-mode suppression, including whether any added device's capacitance is compatible with 100BASE-TX pairs.

The brief is silent on protection entirely. It names no environment, no ESD level, and no standard to design against.

*Decision:* **not yet made.**

### OPEN-15 — Board outline, dimensions, mounting-hole pattern, keepouts, connector edge placement, and any enclosure interface.

The brief states no mechanical constraint of any kind — no size, no shape, no mounting, no enclosure.

*Decision:* **not yet made.**

### OPEN-16 — PHY configuration provisioning: PHY address and mode strapping, MDIO access, reset sequencing between MCU and PHY, and any MAC-address storage.

The brief does not mention strapping, MDIO, reset order, or MAC-address provisioning at all, though a working PHY link requires decisions on each.

*Decision:* **not yet made.**

### OPEN-17 — Manufacturing targets: fabricator and process class, minimum trace/space and via geometry, surface finish, assembly side(s), and component sourcing/lifecycle criteria.

The brief names no vendor, process, finish, or sourcing constraint; only the four-layer count is fixed.

*Decision:* **not yet made.**

### OPEN-18 — Bring-up and test provisions: test points, link-verification method, and what evidence will be produced to show the impedance, plane-continuity, and clock requirements were actually met.

The brief states design constraints but prescribes no verification method or acceptance evidence for them.

*Decision:* **not yet made.**

## Where a decision gets recorded

1. Set `chosen` and `rationale` on the matching entry in
   [requirements.json](requirements.json). **That file is the authoritative
   record**, and the only one the benchmark's scripts read: a decision written
   only in prose is invisible to `board_status.py` and to any result that
   counts how many decisions an attempt actually made.
2. Answer it under its `OPEN-nn` heading here as well, with the reasoning and
   the evidence that made the choice. This file is the readable copy; where the
   two disagree, the JSON is what happened.
3. Cite the datasheet or standard in [docs/sources.md](../docs/sources.md).

A choice recorded this way stays visibly a choice. That is what lets a later
reader tell this board's engineering apart from its brief.

## Where this board is most likely to be faked

Places where a design run would be tempted to assert something it cannot
substantiate:

- The 50 MHz clock topology is the headline fabrication risk: the brief explicitly defers it to 'the selected devices', so any clock source, direction, or termination stated without a cited PHY/MCU datasheet recommendation is fabricated, not designed.
- Impedance claims without a stackup. Naming a differential impedance target is easy; showing where that target comes from and what trace width and gap produce it in a named 4-layer stackup, at the fabricator's stated tolerance, is the actual requirement.
- Return-path continuity is asserted far more often than demonstrated. Plane splits under RMII, layer changes on the differential pairs with no stated return path, and planes broken by connector cutouts or via fields all violate a requirement the brief states outright.
- 'Integrated magnetics if convenient' invites quietly skipping the magnetics analysis entirely. If discrete magnetics are chosen, centre-tap termination, the isolation treatment, and the shield-ground decision all become explicit work that must be shown.
- RJ45 shield and chassis grounding is commonly stated as a one-line decision with no supporting reasoning. Either bonding choice needs a rationale, and the isolation requirement has to survive it.
- The RMII skew and length budget can be invented as a round number. It must come from the two device datasheets' timing figures, not from a remembered rule of thumb.
- USB-C is frequently drawn as a bare receptacle. The brief asks for 'USB-C for service power/debug', so the port's power role, the method by which that role is configured, and the current budget behind it are design work to be decided and documented — a footprint alone does not make the port carry service power.
- No dimension, rail voltage, connector, or part number exists anywhere in the brief or metadata. Any such value appearing in the design must be labelled a documented design decision, never restated as a user requirement.
