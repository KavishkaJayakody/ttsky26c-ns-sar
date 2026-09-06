<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This is an 8-bit differential Successive Approximation Register (SAR) ADC
intended to be the core of a Noise Shaping SAR ADC.

The design output rate is 8kHz. The conversion requires 12 clock cycles per conversion. The digital
controller was designed with the timing and operation of the intended
noise-shaping SAR (NS-SAR) architecture in mind, and area has been
allocated for the future integration of the noise-shaping circuitry.
However, the noise-shaping loop is not included in this version of the
fabricated core.

The CDAC was designed with Dynamic Element Matching (DEM) capability in
mind, but DEM is not enabled in this version. This allows the basic SAR
core functionality to be verified independently before integrating the
noise-shaping and DEM features.

## How to test

Connect the differential analog inputs `V_P` and `V_N` within the
specified input range. Connect `VREF_P` to 1.8 V and `VREF_N` to 0 V.
`VCM` should be connected to 0.9 V.

A 5 µA bias current should be provided to `IBIAS`. Apply a 96 kHz clock
to `CLK`. The `RST_N` input is active-low and should be tied high for
normal operation. Set `EN` high to enable the ADC.

Each conversion takes 12 clock cycles. The resulting 8-bit conversion
code is available on `uo_out[7:0]`. `uio_out[0]` provides the End of
Conversion (`EOC`) indicator.

## Pinout

| Pin | Signal | Description |
|-----|--------|-------------|
| `ua[0]` | `V_N` | Differential negative input(0-1.8V) |
| `ua[1]` | `V_P` | Differential positive input(0-1.8V) |
| `ua[2]` | `VCM` | Common-mode voltage (0.9 V) |
| `ua[3]` | `VREF_P` | Positive reference voltage (1.8 V) |
| `ua[4]` | `VREF_N` | Negative reference voltage (0 V) |
| `ua[5]` | `IBIAS` | Bias current input (5 µA) |
| `ui_in[0]` | `CLK` | Conversion clock (96 kHz) |
| `ui_in[1]` | `RST_N` | Active-low reset |
| `ui_in[2]` | `EN` | Active-high enable |
| `uo_out[7:0]` | `ADC_OUT` | 8-bit digital conversion result |
| `uio_out[0]` | `EOC` | End of Conversion indicator |

## External hardware

No external hardware is required apart from the clock source, reference
voltage sources, and the 5 µA bias current source.
