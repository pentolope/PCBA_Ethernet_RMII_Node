# 100BASE-TX Ethernet Control Node

A 100BASE-TX Ethernet control board: MCU with RMII plus an external PHY, RJ45, link/activity LEDs, USB-C service power/debug, expansion header, 4 layers.

This repository seeds the design of a **100BASE-TX Ethernet control board** built from an MCU with an RMII interface driving an **external** Ethernet PHY. The brief pins down the functional set — RJ45 (with integrated magnetics "if convenient"), link/activity LEDs, USB-C for service power/debug, and a small expansion header — and pins down the hard signal-integrity constraints: a 50 MHz RMII reference-clock architecture *as recommended by the devices that are eventually selected*, Ethernet pairs routed as controlled differential pairs from PHY to magnetics, quiet PHY analog supplies, continuous reference planes under RMII and other high-speed signals, and four layers.

What the brief does **not** fix is everything downstream of those constraints: no MCU, no PHY, no specific RJ45 or USB-C part, no magnetics topology, no rails, no impedance target, no stackup, no board outline, no protection scheme. Those are recorded here as open decisions, and the design agent is expected to make and document them from device datasheets and fabricator capability data rather than inherit them from this scaffold.

> **This board has not been designed.** There is no schematic, no layout and no
> part selection here — only the brief, a reading of the brief, and the
> scaffolding a design run needs. That is the intended state of this repository,
> not a gap in it.

## What the brief fixes, and what it leaves open

The brief pins down 14 requirements and deliberately leaves
18 decisions to whoever designs the board. The `Source` column says
which is which: `brief` is quoted from [BRIEF.md](BRIEF.md), `metadata` comes
from the benchmark catalogue, and `open` means the brief does not fix it.

| Aspect | Value | Source |
|---|---|---|
| Board function | 100BASE-TX Ethernet control board | brief |
| Core architecture | An MCU with RMII driving an external (not MCU-integrated) Ethernet PHY | brief |
| RMII reference clock | 50 MHz RMII reference-clock architecture, as recommended by whichever devices are selected | brief |
| RJ45 and magnetics | An RJ45 is required; integrated magnetics are asked for only "if convenient" | brief |
| Status indication | Link and activity LEDs | brief |
| Service power and debug port | USB-C, serving service power and debug | brief |
| Expansion | A small expansion header | brief |
| Ethernet pair routing | Controlled differential pairs from PHY to magnetics | brief |
| PHY analog supplies | Must be kept quiet | brief |
| Reference planes | Continuous under RMII and other high-speed signals | brief |
| Layer count | Four layers, stated by the brief as required | brief |
| Likely layer count | 4 | metadata |
| Category / difficulty / brief detail | networking / 3 of 5 / 4 of 5 | metadata |
| Primary stressors | RMII clock/data, Ethernet magnetics, diff pairs, return paths | metadata |
| Outline, dimensions, mounting, power rails, impedance targets | Not fixed by the brief — design agent's choice, to be decided and documented | open |

The full split, with the verbatim brief text substantiating every fixed
requirement, is in [board/requirements.md](board/requirements.md) and
machine-readably in [board/requirements.json](board/requirements.json).

**Missing details are design freedom, not permission to fabricate unstated user
requirements.** A choice the brief left open is recorded as a decision, with its
reasoning — never promoted into a requirement.

## Benchmark position

| | |
|---|---|
| Benchmark id | 15 of 32 |
| Category | networking |
| Difficulty | 3 / 5 |
| Brief detail | 4 / 5 |
| Likely layer count | 4 |
| Primary stressors | RMII clock/data, Ethernet magnetics, diff pairs, return paths |

A mid-difficulty (3/5), high-detail (4/5) networking board whose real subject is mixed-domain signal integrity on a modest 4-layer stackup: the RMII clock/data timing budget, the analog Ethernet chain through magnetics to the RJ45, controlled differential pairs, and return-path continuity. The brief is deliberately explicit about *constraints* (50 MHz RMII architecture, controlled diff pairs, quiet PHY analog supplies, continuous reference planes, four layers) while naming no device, rail, impedance, or dimension — so it tests whether an agent can honour stated intent without inventing the parts and numbers needed to satisfy it. The phrase "recommended by the selected devices" makes the clock topology explicitly datasheet-derived, which is the benchmark's main trap for unsubstantiated assertion.

This repository is one of thirty-two. The suite, the protocol and the results
live in [PCBA_AutoDesignAndTest_Bench](https://github.com/pentolope/PCBA_AutoDesignAndTest_Bench).

## Repository layout

| Path | Contents |
|---|---|
| `BRIEF.md` | the supplied brief — authoritative, preserved byte for byte, never edited |
| `board/requirements.md` | what the brief fixes, what it leaves open, and where decisions get recorded |
| `board/requirements.json` | the same split, machine-readable, each fixed requirement bound to brief text |
| `board/manifest.template.json` | the toolkit's minimum manifest, pre-filled for this board |
| `board/toolchain.json` | where this board's build finds KiCad and the router |
| `benchmark/metadata.json` | the supplied catalogue entry — category, difficulty, detail, stressors |
| `docs/architecture.md` | the decisions this board must make, as questions, unanswered |
| `docs/sources.md` | the classes of evidence the design will have to cite |
| `docs/status.md` | what exists, what does not, and what is deliberately absent |
| `candidates/` | disposable search output, ignored by Git |
| `.claude/skills/` | the accountability-review skill [CLAUDE.md](CLAUDE.md) requires before a push |
| `tooling/PCBA_AutoDesignAndTest` | the shared verification/routing/release toolkit, as a pinned submodule |

## Getting the repository

The toolkit is a submodule and carries KiCad Routing Tools as a submodule of its
own, so clone recursively:

```bash
git clone --recursive https://github.com/pentolope/PCBA_Ethernet_RMII_Node.git
```

```bash
git submodule update --init --recursive
```

## Designing the board

Generic verification, routing and release logic is **not** written here. It is
consumed from `tooling/PCBA_AutoDesignAndTest`, which is board-agnostic by
construction and must stay that way; this repository owns the board and nothing
else. Start from
[the toolkit's onboarding guide](tooling/PCBA_AutoDesignAndTest/examples/onboarding.md),
and see [CLAUDE.md](CLAUDE.md) for the rules a design run works under.

```bash
python3 tooling/PCBA_AutoDesignAndTest/run.py preflight
```

## Brief integrity

`BRIEF.md` SHA-256 `0ff1c2a7403e522f1f1d15b41913e390ef8422d07f367d815823aaed69807917`

Every quotation in `board/requirements.json` is bound to those exact bytes. If
the brief ever changes, the bindings are stale by construction — which is the
point of recording the digest.
