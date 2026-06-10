# AlGaN/GaN HEMT Device Design Using Sentaurus TCAD (SDE)
![image](https://github.com/shreyasingh2302vl10/TCAD-SDE-Sentaurus-AlGaN-GaN-HEMT-Device-Design-/blob/3418fb682c855c5fa074996ebe4ef3282b01071a/Intro_Image.png)
## Overview

High Electron Mobility Transistors (HEMTs) based on the AlGaN/GaN material system are widely used in modern power electronics due to their high breakdown voltage, high electron mobility, and low on-resistance. The formation of a Two-Dimensional Electron Gas (2DEG) at the AlGaN/GaN interface enables high-current operation and improved switching performance compared to conventional silicon-based devices.

This project focuses on the development of an AlGaN/GaN HEMT device structure using Synopsys Sentaurus TCAD. The complete device geometry is created through Scheme-based scripting in Sentaurus Structure Editor (SDE), providing a parameterized and scalable framework for semiconductor device design.

---

## Device Structure

The device consists of a Silicon substrate followed by a GaN buffer layer that supports the active region. An AlGaN barrier layer is introduced above the GaN buffer to induce the formation of the 2DEG channel at the heterojunction interface. A p-GaN gate structure is incorporated to realize enhancement-mode (normally-off) operation. Source and drain regions are formed on either side of the gate, while a metal gate electrode controls the channel conductivity.

| Parameter | Value |
|-----------|-------|
| Total Device Length | 16 µm |
| Silicon Substrate Thickness | 2 µm |
| GaN Buffer Thickness | 2 µm |
| AlGaN Barrier Thickness | 20 nm |
| p-GaN Layer Thickness | 100 nm |
| Gate Length | 1.5 µm |
| Gate-to-Source Spacing | 1.5 µm |
| Gate-to-Drain Spacing | 7 µm |
| Source Length | 3 µm |
| Drain Length | 3 µm |

---

## Design Methodology

The device structure is generated using a fully parameterized scripting approach in Sentaurus SDE.

### Geometry Definition
- Definition of device dimensions and layer thicknesses.
- Coordinate generation for scalable device construction.
- Creation of substrate, buffer, barrier, source, drain, and gate regions.

### Material Assignment
- Silicon substrate.
- Gallium Nitride (GaN) buffer and active regions.
- Aluminum Gallium Nitride (AlGaN) barrier layer.
- Aluminum gate metal.

### Doping Profiles
- Background substrate doping.
- GaN buffer doping.
- Magnesium-doped p-GaN gate region.
- Heavily doped source and drain regions for ohmic contact formation.

### Contact Engineering
- Source contact definition.
- Gate contact definition.
- Drain contact definition.

### Mesh Generation
- Localized mesh refinement in critical regions.
- Enhanced resolution near the barrier and gate regions.

---

## Features

- Fully parameterized device structure
- Scheme-based Sentaurus SDE scripting
- AlGaN/GaN heterostructure implementation
- p-GaN gate architecture
- Contact assignment and doping specification
- Automated mesh generation
- Easily scalable geometry for future device optimization

---

## Tools Used

- Synopsys Sentaurus TCAD
- Sentaurus Structure Editor (SDE)
- Scheme Scripting Language

---

## Repository Contents

| File | Description |
|--------|-------------|
| `HEMT.cmd` | Sentaurus SDE script for device structure generation |
| `README.md` | Project documentation |

---

## Author

**Shreya Singh**  
Dual Degree (B.Tech ECE + M.Tech VLSI)  
Indian Institute of Technology Patna
