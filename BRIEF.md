# PCBA_Ethernet_RMII_Node — 100BASE-TX Ethernet Control Node

**Benchmark ID:** 15  
**Difficulty:** 3/5  
**Brief detail:** 4/5  
**Category:** networking  
**Likely layer count:** 4  
**Primary stressors:** RMII clock/data, Ethernet magnetics, diff pairs, return paths

## Design brief

Create a 100BASE-TX Ethernet control board using an MCU with RMII and an external PHY. Provide an RJ45 with integrated magnetics if convenient, link/activity LEDs, USB-C for service power/debug, and a small expansion header. Use a 50 MHz RMII reference-clock architecture recommended by the selected devices. Route Ethernet pairs as controlled differential pairs from PHY to magnetics, keep PHY analog supplies quiet, and maintain continuous reference planes under RMII/high-speed signals. Four layers required.

## Benchmark intent

This brief is intentionally one member of a heterogeneous PCBA-autodesign benchmark. Treat stated requirements as authoritative; where the brief leaves choices open, make and document reasonable engineering decisions rather than inventing hidden user requirements. The repository should remain a consumer of the shared `PCBA_AutoDesignAndTest` toolkit rather than accumulating board-specific logic in the toolkit.
