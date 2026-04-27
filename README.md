# FPGA-Based 2D Convolution Accelerator on Zynq

## Overview

This repository contains an RTL implementation of a **2D convolution accelerator** for image filtering on FPGA, tested using the **Lena** test image and integrated as a custom IP in a Zynq block design.

The project implements a streaming image-processing pipeline using:

* Custom Verilog RTL
* Line buffers for sliding window generation
* 3×3 convolution engine
* AXI4-Stream compatible custom IP
* Vivado block design integration
* TLAST-enabled streaming support for DMA/video pipelines

---

## Architecture

Pipeline:

```text
Input Pixel Stream
   ↓
Line Buffers (3x3 Window Generation)
   ↓
Convolution MAC Engine
   ↓
AXI Stream Output Buffer (TVALID/TREADY/TLAST)
   ↓
Filtered Output Image
```

### Modules

### `imageProcessTop.v`

Top-level accelerator integrating:

* Streaming interfaces
* Image control
* Convolution block
* Output AXI buffer
* Interrupt generation
* TLAST signaling

### `imageControl.v`

Responsible for:

* Sliding window generation
* Multi-line buffering
* Pixel scheduling
* Frame/line control
* TLAST generation

### `lineBuffer.v`

Implements row buffering for neighborhood extraction.

### `conv.v`

Implements 3×3 convolution kernel.
(Current kernel initialized as averaging filter.)

---

## Features

* RTL-based image convolution accelerator
* 3×3 filter support
* AXI4-Stream compatible IP
* TLAST support for ZCU102 integration
* Vivado block design ready
* Lena image verification

---

## Target Platform

* Xilinx ZCU102 Evaluation Kit
* Vivado Design Suite
* Verilog HDL

---

## Example Kernel

Current kernel:

```text
1 1 1
1 1 1
1 1 1
```

Implements averaging/blur:

Output = Sum(kernel × window)/9

Can be extended for:

* Sobel
* Gaussian
* Sharpen
* Edge detection

---

## Repository Structure

```text
fpga-convolution-accelerator/
├── rtl/
│   ├── imageProcessTop.v
│   ├── imageControl.v
│   ├── lineBuffer.v
│   └── conv.v
│
├── simulation/
│   ├── testbench/
│   └── lena_vectors/
│
├── block_design/
│   ├── vivado_bd.tcl
│   └── block_diagram.png
│
├── images/
│   ├── lena_input.png
│   └── filtered_output.png
│
└── docs/
    └── project_report.pdf
```

---

## Results



* Input Lena image
* Filtered output image
* Vivado block design
* Simulation waveforms
* Resource utilization report

---

## AXI Stream Notes

The IP supports:

* TDATA
* TVALID
* TREADY
* TLAST (line boundary signaling)

TLAST was added for correct DMA/video-stream interoperability.

---

## Possible Extensions

Future improvements:

* Parameterized kernel size (5×5, 7×7)
* Runtime-programmable kernels via AXI-Lite
* Multi-channel CNN convolution support
* Throughput and resource benchmarking
* Real-time video stream input

---

## Reference

Developed with learning references from a Zynq FPGA image-processing tutorial playlist and extended through custom RTL implementation, IP packaging, and system integration.

---

## Resume Description

Designed and implemented an RTL-based FPGA convolution accelerator, packaged as custom AXI-stream IP and integrated in a Zynq block design for image filtering.

---

## License

MIT License
