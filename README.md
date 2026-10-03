# Max Gabriel Susman

Electrical engineering student at the University of Utah, building neural
interface hardware. The current project:

## Argus Cybernetics

A neural acquisition and decoding pipeline, end to end, on real hardware.
Recorded motor-cortex data → simulated Intan headstage chips → a spike-feature
codec in Zynq fabric → UDP → ROS 2 → a decoder → `/cmd_vel`, twenty times a
second.

[![Argus Cybernetics v1.0 — live run](https://asciinema.org/a/1266932.svg)](https://asciinema.org/a/1266932)

- The codec is **bit-exact against its Python model on silicon** —
  139,200 / 139,200 (bin, channel) pairs, counts and spike-band power.
- The decoder, trained on the fabric's own feature set, scores **53.5 %**
  four-way intent accuracy (5-fold CV) against **49.9 %** for the lab's
  spike-sorted units on the same session.
- Replay into the fabric at real time, zero underruns; fabric timing closed
  at 125 MHz; ~14,500 frames on the wire without an error.
- One command brings the stack up, one command tests it on the board.

**Start here → [argus_bringup](https://github.com/Max-Gabriel-Susman/argus_bringup)** —
the overview, the launch, the hardware test harness, and the full results table.

The stack, eight repositories, Apache-2.0:

| | |
| --- | --- |
| [argus-neural-codec](https://github.com/Max-Gabriel-Susman/argus-neural-codec) | the fabric: SPI master, chip models, the codec, AXI register block (VHDL, GHDL benches) |
| [argus_safety_controller](https://github.com/Max-Gabriel-Susman/argus_safety_controller) | bare-metal Zynq firmware: replay client, feature reads, UDP telemetry; headless build and program tools |
| [argus_core](https://github.com/Max-Gabriel-Susman/argus_core) | the wire contract, shared by firmware and ROS, enforced by CI |
| [argus_sensors](https://github.com/Max-Gabriel-Susman/argus_sensors) | UDP receiver and telemetry bridge into the ROS graph |
| [argus_inference](https://github.com/Max-Gabriel-Susman/argus_inference) | the decoder node |
| [argus_sim](https://github.com/Max-Gabriel-Susman/argus_sim) | dataset relay, the bit-exact feature model, decode validation, on-silicon comparison |
| [argus_data](https://github.com/Max-Gabriel-Susman/argus_data) | dataset provenance and derivation scripts |
| [argus_bringup](https://github.com/Max-Gabriel-Susman/argus_bringup) | the launch, the harness, the overview |

Hardware: Digilent Arty Z7-20 (Zynq XC7Z020). Data: O'Doherty, Cardoso, Makin
& Sabes, macaque M1, 96-channel Utah array (Zenodo, CC-BY-4.0).
