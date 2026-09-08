![](../../workflows/gds/badge.svg) ![](../../workflows/docs/badge.svg)

# NS-SAR ADC — 8-bit Differential SAR Core

**Tiny Tapeout analog and digital submission (shuttle `ttsky26c`, SKY130, 2×2 analog tiles)**

> ### Submission status: intermediate design
>
> This repository contains an **intermediate design submission** that forms part of
> our ongoing Final Year Project on a *Low-Power Noise-Shaping SAR ADC in the SKY130
> CMOS process for energy-constrained sensing applications*.
>
> **What is submitted here is only the 8-bit differential SAR ADC core.** The
> noise-shaping loop is **not** part of this tapeout. This submission is intended to
> verify the fundamental SAR conversion path — CDAC, comparator, and SAR control
> logic — in silicon, independently of the additional noise-shaping and calibration
> circuitry.
>
> In future revisions we will **extend this design into a full noise-shaping SAR
> (NS-SAR) ADC**, adding the residue-extraction and shaping paths on top of the same
> SAR core, with the goal of selectable 8-/10-/12-bit effective resolution.

Read the full datasheet: [docs/info.md](docs/info.md)

## What is included in this submission

- Fully differential **8-bit charge-redistribution SAR ADC**
- Differential **capacitive DAC (CDAC)** with bottom-plate sampling and dedicated
  common-mode switching
- **Dynamic comparator** (preamplifier + Armstrong latch + SR latch)
- **Digital SAR controller** providing the sampling, comparison and successive-
  approximation phases, with timing already arranged for the future NS-SAR sequence
- **Non-overlapping clock generator** derived from the external conversion clock
- Full custom layout, GDS and LEF prepared for the Tiny Tapeout analog flow

The CDAC is laid out in a **DEM-ready segmented form**: each binary weight is built
from equal-sized unit-capacitor groups (28 groups of 8C, 7 groups of 4C, 1 group of
2C, 2 groups of 1C per side) driven by independent select lines, so that Dynamic
Element Matching can later be enabled without redrawing the array.

## What is *not* included yet

| Feature | Status in this submission |
|---|---|
| Noise-shaping loop (residue extraction, passive SC FIR, dynamic amplifier) | Not implemented — planned for the next revision |
| Dynamic Element Matching (DEM) | Array and control lines designed for it, but **not enabled** |
| Selectable 8/10/12-bit effective-resolution modes | Not implemented — depends on the noise-shaping loop |

Physical area has been deliberately reserved in the floorplan to accommodate these
blocks in the follow-up design.

## Design specifications

| Parameter | Value |
|---|---|
| Architecture | Fully differential conventional-switching SAR |
| Resolution | 8 bits |
| Process | SKY130 (sky130A), 1.8 V |
| Conversion clock | 96 kHz |
| Conversion cycles | 12 clock cycles per conversion |
| Output data rate | 8 kS/s |
| Positive reference (`VREF_P`) | 1.8 V |
| Negative reference (`VREF_N`) | 0 V |
| Common-mode voltage (`VCM`) | 0.9 V |
| Bias current (`IBIAS`) | 5 µA |
| Target DNL / INL | ±0.5 LSB / ±1 LSB |
| Die area | 334.88 µm × 225.76 µm (2×2 analog tiles) |

## Pinout

| Pin | Signal | Description |
|-----|--------|-------------|
| `ua[0]` | `V_N` | Differential negative input (0–1.8 V) |
| `ua[1]` | `V_P` | Differential positive input (0–1.8 V) |
| `ua[2]` | `VCM` | Common-mode voltage (0.9 V) |
| `ua[3]` | `VREF_P` | Positive reference voltage (1.8 V) |
| `ua[4]` | `VREF_N` | Negative reference voltage (0 V) |
| `ua[5]` | `IBIAS` | Bias current input (5 µA) |
| `ui_in[0]` | `CLK` | Conversion clock (96 kHz) |
| `ui_in[1]` | `RST_N` | Active-low reset |
| `ui_in[2]` | `EN` | Active-high enable |
| `uo_out[7:0]` | `ADC_OUT` | 8-bit conversion result |
| `uio_out[0]` | `EOC` | End-of-conversion indicator |

Note that the conversion clock and reset are taken on `ui_in[0]` and `ui_in[1]`; the
dedicated Tiny Tapeout `clk` and `rst_n` pins are **not** used by this design.

## How to test

Connect `V_P` and `V_N` within the specified input range, `VREF_P` to 1.8 V,
`VREF_N` to 0 V and `VCM` to 0.9 V, and supply 5 µA into `IBIAS`. Apply a 96 kHz
clock to `ui_in[0]`, hold `ui_in[1]` (`RST_N`) high and set `ui_in[2]` (`EN`) high.
Each conversion takes 12 clock cycles; the 8-bit code appears on `uo_out[7:0]` and
`uio_out[0]` flags end of conversion. No external hardware is required beyond the
clock source, reference voltages and bias current source.

## Repository layout

| Path | Contents |
|---|---|
| [src/project.v](src/project.v) | Black-box port declaration for the custom-GDS flow |
| [gds/](gds/) | Submitted GDS layout |
| [lef/](lef/) | Abstract LEF view |
| [mag/](mag/) | Magic bootstrap script and the Tiny Tapeout 2×2 analog pin-frame DEF |
| [docs/info.md](docs/info.md) | Project datasheet |
| [info.yaml](info.yaml) | Tiny Tapeout project metadata |

## Project context

This work is carried out as a Final Year Project in the Department of Electronic and
Telecommunication Engineering, University of Moratuwa, Sri Lanka, under the course
module EN4203. The wider project investigates a reconfigurable low-power NS-SAR ADC
together with an application-level wireless-powered sensing prototype. This tapeout
covers the SAR core only; the noise-shaping extension follows in a later revision.

## Contributors

**Department of Electronic and Telecommunication Engineering, University of Moratuwa**

- Viduranga J. K. A.
- Jayakody J. A. K.
- Rajinthan R.
- Munavvar M. A. A.

**Supervisors**

- Dr. Chamira U. S. Edussooriya — University of Moratuwa
- Dr. Nilan Udayanga — Cirtec Medical

## About Tiny Tapeout

Tiny Tapeout is an educational project that aims to make it easier and cheaper than
ever to get your designs manufactured on a real chip. To learn more and get started,
visit [tinytapeout.com](https://tinytapeout.com).

- [Analog project specifications](https://tinytapeout.com/specs/analog/)
- [FAQ](https://tinytapeout.com/faq/)
- [Join the community](https://tinytapeout.com/discord)

## License

Apache-2.0 — see [LICENSE](LICENSE).
