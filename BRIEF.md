# PCBA_Ethernet_RMII_Node — 100BASE-TX Ethernet Control Node
## Design brief

Create a 100BASE-TX Ethernet control board using an MCU with RMII and an external PHY. Provide an RJ45 with integrated magnetics if convenient, link/activity LEDs, USB-C for service power/debug, and a small expansion header. Use a 50 MHz RMII reference-clock architecture recommended by the selected devices. Route Ethernet pairs as controlled differential pairs from PHY to magnetics, keep PHY analog supplies quiet, and maintain continuous reference planes under RMII/high-speed signals. Four layers required.
