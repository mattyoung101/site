+++
title = "Projects"
menu = "main"
+++

# Projects

The following is a list of projects I'm working on, or have worked on.

<!-- mtoc-start -->

* [Active projects](#active-projects)
  * [TaMaRa](#tamara)
  * [Slingshot](#slingshot)
  * [storage.horse](#storagehorse)
* [Past projects](#past-projects)
  * [musicvis3d (2024)](#musicvis3d-2024)
  * [Hermes/Atlas (2023)](#hermesatlas-2023)
* [Other misc. things I've done](#other-misc-things-ive-done)

<!-- mtoc-end -->

## Active projects
### TaMaRa
_Category: Computer engineering/EDA_

_Licence: Mozilla Public License v2.0 (not yet released, however)_

TaMaRa is an automated Triple Modular Redundancy EDA flow for the open-source Yosys EDA tool to protect FPGAs
and ASICs from Single Event Upsets in spaceflight/high reliability environments. There is some existing
literature in this area, but TaMaRa is unique in that it's integrated into Yosys directly, and also uses a
combination of design-level (i.e. high-level, operating at the RTL level) and netlist-level (i.e. circuit
level, operating on circuit graph primitives) approaches. This is made possible by the user defining
`(* tamara_triplicate *)` or `(* tamara_ignore *)` annotations in their (System)Verilog RTL, which enables
better control over the level of granularity at which TMR is applied.

In further detail, copied from my thesis' description:

> **An automated triple modular redundancy EDA flow for Yosys**
>
> For safety-critical sectors such as aerospace and defence, silicon ICs and FPGA gateware must be designed to
> be fault tolerant in order to mitigate the effects of Single Event Upsets (SEUs) triggered by ionising
> radiation. One common fault-tolerant design technique is Triple Modular Redundancy (TMR), which mitigates
> SEUs by triplicating key parts of the design and using voter circuits to select a non-corrupted result if an
> SEU occurs. Typically, TMR is manually designed at the HDL level. However, this approach is an additional
> time-consuming and potentially error-prone step in the already complex design pipeline. Instead, I propose
> TaMaRa: a novel fully automated TMR flow for the open source Yosys EDA tool. TaMaRa will accept any
> Yosys-compatible HDL input, and use graph theory algorithms to synthesise a TMR netlist. I will also use a
> combination of formal methods and simulation to prove the correctness of the TMR flow.

I will be working on TaMaRa through to about July 2025. The plugin is written in C++20, and includes extensive
verification based on traditional testbenches, formal verification and also fuzzing. When it's finished,
_TaMaRa_ will be available under the permissive [Mozilla Public License
v2.0](https://www.mozilla.org/en-US/MPL/2.0/FAQ/), and the papers under CC-BY (n.b. the code licence is
already approved by my supervisor, but I need to check on the papers).

The code is currently unavailable while I work on it, but rest assured will be released when it's done. One of
the key elements of this project is making an end-to-end solution that's available to the wider open-source
EDA community.

### Slingshot

### storage.horse

## Past projects
### musicvis3d (2024)
_Category: Computer graphics_

_Licence: ISC_

This is a semi real-time 3D audio visualisation using OpenGL. The visualisation consists of offline spectral
data that is rendered in real-time in the form of 3D bars. A multitude of graphics techniques are used,
including: quaternion camera animation, camera shake using Simplex noise under fractal Brownian motion, a
skybox, and a post-processing stage that implements chromatic aberration. The application is written in a mix
of C++ (for rendering) and Python (for DSP). The spectrum of bars is computed using the Fast Fourier
Transform.

I implemented this for the course COSC3500 at UQ, in 2024.

[Visit the musicvis3d repo &rarr;](https://github.com/mattyoung101/musicvis3d)

### Hermes/Atlas (2023)

## Other misc. things I've done
- I [patched](https://github.com/intel/media-driver/commit/454a82ee3f82bb274494b97f4892fb8f88fe33f3)
Intel's media-driver to fix a [segfault bug](https://github.com/intel/media-driver/issues/1859).
- I [fuzzed Yosys in a Docker container](https://github.com/mattyoung101/yosys_honggfuzz_docker), which
uncovered [a bug](https://github.com/YosysHQ/yosys/issues/4599), that I should submit a patch for :)
