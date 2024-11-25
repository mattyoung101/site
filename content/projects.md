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
* [Other fun things I've done](#other-fun-things-ive-done)

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
_Category: Computer engineering/EDA_

_Licence: Mozilla Public License v2.0_

Slingshot is a language server for the SystemVerilog hardware description language, with a focus on accurate
multi-file completion. The overarching goal is to make SystemVerilog as intuitive to edit as C++ or Python.
Slingshot is written in Kotlin and runs on a Java 17 JVM or higher. In the past, it was also written in Rust,
although I ended up rewriting it in Kotlin due to problems with ANTLR and problems with concurrency (no fault
against Rust, I just don't know how to write the language lmao).

Compared to other SV LSPs, the main feature that Slingshot brings to the table is a powerful completion
system, backed by ANTLR, that supports multi-file projects through automatic indexing. Slingshot also supports
instant linting, backed by Verilator.

At the moment, my time to work on Slingshot is limited, but I still consider it to be an active project. The
next upgrade will be rewriting the parsing system away from ANTLR, as its error recovery has issues that makes
it not suited for writing an LSP server. My current method to achieve this will be _slangd_, a gRPC daemon for
the [Slang](https://github.com/MikePopoloski/slang) SystemVerilog frontend, which is one of the highest
quality around. Another option is to rewrite the server (again...) in C++, but the C++ LSP library ecosystem
is severely lacking - so I'll either need to yoink
[clangd's](https://github.com/llvm/llvm-project/tree/main/clang-tools-extra/clangd), which is tied to LLVM, or
hack on [an existing library](https://github.com/kuafuwang/LspCpp).

[You can try Slingshot here &rarr;](https://github.com/mattyoung101/slingshot)

### storage.horse
_Category: Infrastructure_

_Licence: N/A; Nextcloud is AGPL though_

[storage.horse](https://storage.horse/) is a Nextcloud instance for my friends and family. It runs on FreeBSD
and uses Wasabi via rclone as the storage backend. I'm also going to blog about the process of setting this
up!

Here are the full specs:

- **OS:** FreeBSD 14
- **Cloud:**
    - **Servers**: Google Cloud Compute, datacentre in Sydney, Australia
    - **Storage**: [Wasabi](https://wasabi.com/), datacentre in Sydney, Australia; with offsite backups to
    [rsync.net](https://www.rsync.net/) (datacentre in Fremont, CA, USA)
- **Database:** PostgreSQL
- **Storage backend:** `rclone mount` with a bunch of tuning
- **Capacity:** ~1 TB (initial capacity; can be expanded)

Some additional features:

- Storage is encrypted between Google Cloud and Wasabi/rsync.net using rclone's `crypt` backend (XSalsa20 and
Poly1305 ciphers)
    - This means that data is effectively encrypted "at rest" and cannot be read by Wasabi or rsync.net
    - Data is (sadly) _not_ encrypted at rest per-user, so I (the admin) can still read everyone's files (!)
- Various tuning that I'll eventually write a blog about, including the use of the BBR TCP congestion
algorithm

**Please note** that storage.horse is invite only to my friends and family, and will almost certainly never be
open to public registrations due to issues with preventing abuse.

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

## Other fun things I've done
These are other cool (I think) things I've gotten up to, that don't really need their own category.

**2024**

- I [patched](https://github.com/intel/media-driver/commit/454a82ee3f82bb274494b97f4892fb8f88fe33f3)
Intel's media-driver to fix a [segfault bug](https://github.com/intel/media-driver/issues/1859).
- I [fuzzed Yosys in a Docker container](https://github.com/mattyoung101/yosys_honggfuzz_docker), which
uncovered [a bug](https://github.com/YosysHQ/yosys/issues/4599), that I should submit a patch for :)
