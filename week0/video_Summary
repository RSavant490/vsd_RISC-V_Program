> Task - 1
# 📘 Summary of Video: Chip Design Flow

This section summarizes the introductory video on chip design and SoC flow.  
It explains how an application written in C eventually runs on real silicon.

---

## 🔹 Key Steps in the Flow

1. **Application & Compiler**
   - Application (C code) compiled with GCC → output `O0`.
   - Processor specs modeled in C (e.g., RISC-V GCC) → output `O1`.
   - ✅ Check if `O0 = O1` → freeze specifications.

2. **Soft Hardware (RTL)**
   - Write hardware in Verilog/Chisel/Bluespec.
   - Run app again → output `O2`.
   - ✅ Ensure `O1 = O2`.

3. **SoC Partitioning**
   - Processor → synthesizable Verilog.
   - Peripherals:
     - Macros (synthesizable).
     - Analog IPs (ADC, PLL → not synthesizable).
   - Integration produces SoC → output `O3`.
   - ✅ Ensure `O1 = O2 = O3`.

4. **Processor vs Microcontroller**
   - Processor = CPU core only (needs external peripherals).
   - Microcontroller = processor + peripherals + IPs (single chip).

5. **Physical Design**
   - RTL → Gates → Layout → **GDSII file**.
   - Run **DRC/LVS checks** → tape-out to foundry.
   - Fabricated chip comes back as **tape-in**.

6. **Chip Bring-up**
   - Chip packaged & mounted on a board with memory, regulators, etc.
   - Application runs → output `O4`.
   - ✅ Final check: `O1 = O2 = O3 = O4`.

---

## 🔹 Timeline
- Complete chip cycle: **14–16 months**.  
- Foundry alone: **4–6 months**.

---

## 🔹 Applications
- Designed RISC-V chip runs at **100–130 MHz**.  
- Suitable for:
  - Arduino-like boards  
  - TV panels (low-freq side)  
  - AC controllers  
  - Embedded/IoT devices  

---

## 🔹 Workshop Scope
- Focus is on a small block (glue block).  
- Example: design & integrate a simple inverter IP.  
- Learn end-to-end flow: **C code → Chip → Board**.

---
