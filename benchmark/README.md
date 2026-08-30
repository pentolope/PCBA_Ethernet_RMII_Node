# Benchmark entry — board 15 of 32

[metadata.json](metadata.json) is the supplied catalogue entry for this board,
preserved byte for byte from the seed pack. It is the same record that appears
in `boards_index.json` in
[PCBA_AutoDesignAndTest_Bench](https://github.com/pentolope/PCBA_AutoDesignAndTest_Bench), and the two must agree.

| | |
|---|---|
| Repository | `PCBA_Ethernet_RMII_Node` |
| Board id | `ethernet_rmii_node` |
| Category | networking |
| Difficulty | 3 / 5 |
| Brief detail | 4 / 5 |
| Likely layer count | 4 |
| Primary stressors | RMII clock/data, Ethernet magnetics, diff pairs, return paths |

`difficulty` is how hard the board is. `detail` is how much of it the brief
states — and a low `detail` is not a low bar. A detail-1 brief leaves the
architecture open on purpose, and an agent that fills the silence with invented
user requirements has failed the board more thoroughly than one that designs it
badly.

A mid-difficulty (3/5), high-detail (4/5) networking board whose real subject is mixed-domain signal integrity on a modest 4-layer stackup: the RMII clock/data timing budget, the analog Ethernet chain through magnetics to the RJ45, controlled differential pairs, and return-path continuity. The brief is deliberately explicit about *constraints* (50 MHz RMII architecture, controlled diff pairs, quiet PHY analog supplies, continuous reference planes, four layers) while naming no device, rail, impedance, or dimension — so it tests whether an agent can honour stated intent without inventing the parts and numbers needed to satisfy it. The phrase "recommended by the selected devices" makes the clock topology explicitly datasheet-derived, which is the benchmark's main trap for unsubstantiated assertion.

## What goes here

Compact results only: metrics, verdicts, and the commit each was measured at.
The evidence for a result is the artefact the toolkit recomputes, not a summary
of it.

Routing search output, candidate pools, build trees and field-solver dumps do
**not** go here. They are ignored by [.gitignore](../.gitignore) and are
regenerated from what is committed. Thirty-two repositories share one benchmark
clone; weight here is paid thirty-two times.

## Protocol

The attempt protocol is defined once, in the umbrella repository, so that
thirty-two boards cannot drift into thirty-two protocols. See
[PCBA_AutoDesignAndTest_Bench/BENCHMARK.md](https://github.com/pentolope/PCBA_AutoDesignAndTest_Bench/blob/main/BENCHMARK.md).
