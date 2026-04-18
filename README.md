# CVFPU-lite Synthesis Exploration

A portfolio repository for an **implementation-oriented study of a bfloat16 CVFPU-lite multiplier**, with a focus on **RTL verification**, **automated synthesis-space exploration**, **post-synthesis simulation**, and **custom multiplier integration**.

## 🚀 Overview

This laboratory project focuses on the implementation flow of a **bfloat16 floating-point multiplication unit** inside a lightweight CVFPU configuration.

Rather than emphasizing architectural design alone, the work explores how different **synthesis strategies** affect:

- **timing closure**
- **maximum operating frequency**
- **cell area**
- **post-synthesis functional correctness**

A second part of the project replaces the original behavioral mantissa multiplier with a custom **Radix-4 Modified Booth Encoder (R4-MBE)** multiplier and evaluates its impact after integration into the FPU.

## ⚙️ Technical Focus

- **bfloat16 multiplier verification**
- **automated synthesis exploration**
- **timing-driven frequency search**
- **post-synthesis functional simulation**
- **comparison of multiple synthesis modes**
- **custom R4-MBE multiplier integration**
- **performance/area trade-off analysis**

## 🧠 Project Structure

The project is organized into three main parts:

### 1. RTL simulation of CVFPU-lite
The original testbench was extended to support more than ten vectors and to propagate expected reference results through a latency-aligned pipeline.

This enabled a **cycle-accurate comparison** between DUT outputs and golden results for a set of twelve multiplication cases, including:

- standard arithmetic operations
- zero operands
- quantization-sensitive cases
- near-overflow conditions

### 2. Automated synthesis exploration
The core of the work is an automated synthesis flow based on paired **Bash** and **Tcl** scripts.

This flow:

- launches synthesis under different optimization modes
- iteratively searches for the **minimum clock period** that still meets timing
- extracts timing and area reports automatically
- runs **post-synthesis functional simulation** using the generated netlist

Five synthesis strategies were evaluated.

### 3. Custom R4-MBE multiplier integration
A custom **Radix-4 Modified Booth Encoder (R4-MBE)** multiplier was designed and tested in standalone form, then integrated into the provided FPU in place of the original behavioral mantissa multiplier.

The custom multiplier includes:

- Booth encoding
- partial-product generation
- Wallace-tree reduction
- carry-propagate addition

After integration, the modified FPU was synthesized again and compared against the earlier synthesis configurations.

## 📊 Synthesis Results

| **Strategy** | **Period [ns]** | **Fmax [MHz]** | **Area [µm²]** |
|---|---:|---:|---:|
| **compile** | **3.28** | **304.88** | **3878.55** |
| **compile + optreg** | **2.28** | **438.60** | **3432.46** |
| **compile ultra** | **2.53** | **395.26** | **3351.60** |
| **CSA multiplier** | **2.23** | **448.43** | **3613.34** |
| **PPARCH multiplier** | **2.28** | **438.60** | **3432.46** |

## 📌 Integrated R4-MBE Result

| **Metric** | **Value** |
|---|---:|
| **Minimum clock period** | **2.52 ns** |
| **Maximum clock frequency** | **396.8 MHz** |
| **Total cell area** | **3696.1 µm²** |

## 🛠️ Automation Flow

A central part of the project is the automation infrastructure.

The scripts were designed in `.sh` / `.tcl` pairs:

- the **shell script** cleans the workspace and launches the tool
- the **Tcl script** drives compilation, synthesis, or simulation inside the tool

The synthesis flow uses environment variables such as:

- `CLK_PERIOD`
- `SYNTHMODE`

The frequency-search algorithm starts from a constrained clock target and iteratively updates the period based on the reported slack until timing is met. Once closure is achieved, the corresponding **post-synthesis simulation** is launched automatically.

This makes the flow much more reproducible and reduces the manual effort typically required for repeated synthesis runs.

## ✅ Verification

Verification is performed at multiple levels:

- **RTL simulation** of the original CVFPU-lite setup
- **post-synthesis netlist simulation** for each synthesis strategy
- **standalone simulation** of the custom R4-MBE multiplier
- **RTL simulation after R4-MBE integration into the FPU**

This keeps the project grounded in implementation realism rather than in synthesis-only reporting.

## 📄 Repository Content

- **`CVFPU_Lite_Report.pdf`** — full technical report

## 📘 Report

[**Open the report**](./CVFPU_Lite_Report.pdf)

## 🔒 Source Code Availability

The source code is **not publicly available** for academic reasons.  
It can be shared upon request for **recruiting purposes**.