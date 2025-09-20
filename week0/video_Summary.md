> Task - 1
## 📺 Video Summary — Getting started with Digital VLSI SOC Design and Planning

## Introduction
The session explains the step-by-step process of chip design, using a silicon-proven chip as an example.

“Silicon proven” is an important term in the industry—if a design has been successfully fabricated and tested, it carries high credibility.

**Goal:** Ensure an application (like PowerPoint, Firefox, Zoom, or even a simple calculator in C) runs on the designed processor chip.

---

## Step-by-Step Flow

### Application in C
- Start with an application written in C.  
- Use **GCC compiler** to compile and test correctness → output = `O0`.

### Specification Modelling (C model)
- Model processor specifications in C-like format (RISC-V GCC, ARM GCC, or x86 GCC).  
- Run the same application and measure output = `O1`.  
- **Goal:** Ensure `O0 = O1`, confirming specifications are frozen.

### Soft Copy of Hardware (RTL stage)
- Convert specifications into a hardware description (using Verilog or higher-level languages like BlueSpec/Chisel).  
- Run the application on this RTL model → output = `O2`.  
- **Goal:** `O1 = O2`, ensuring functionality is retained.

### SoC Design Partitioning
- Split Verilog design into:
  - **Processor** → must be synthesizable to gates.
  - **Peripherals (Macros & IPs):**
    - **Macros:** reusable synthesizable blocks (e.g., clock divider).  
    - **Analog IPs:** like ADCs, PLLs—functional only, not synthesizable (eventually replaced by transistor-level design).  
- Integrate blocks into an SoC and connect via GPIOs.  
- Run application again → output = `O3`.  
- **Goal:** `O1 = O2 = O3`.

### Processor vs Microcontroller
- **Processor:** just the core (e.g., 8085, 8086 kits—large boards with external peripherals).  
- **Microcontroller:** processor + peripherals + IPs integrated into one chip (e.g., Arduino boards with ATmega51).

### Physical Design (RTL → GDSII)
- Flow includes floorplanning, placement, CTS (clock tree synthesis), routing.  
- Output = **GDSII file** (contains metal layers for fabrication).  
- Verification: **DRC** (design rule check), **LVS** (layout vs schematic).  
- Sending design to foundry = **tape out**.  
- Receiving fabricated chips = **tape in**.

### Board Bring-Up & Final Test
- Chips need to be mounted on a board with memory, USB controller, regulators, crystals, etc.  
- Application is loaded onto the board → output = `O4`.  
- **Goal:** `O1 = O2 = O3 = O4`, confirming chip works as intended.

---

## Timeline
- Entire chip design cycle: ~14–16 months.  
- Foundry alone takes ~4–6 months for fabrication and returning chips.

---

## Markets / Use Cases
- Processor being designed runs at 100–130 MHz (not GHz range).  
- Suitable for:
  - Arduino boards (replacement for ATmega chips)  
  - TV panel low-frequency side  
  - AC applications

---

## Workshop Scope
- Participants won’t design the full chip (too large and time-consuming).  
- Focus will be on a **glue block** within the SoC.  
- They will design a small IP (like an inverter), integrate it, and learn the real flow hands-on.

---

## ✅ Core Idea
The whole process ensures the **same application runs consistently** across all stages—from C model to specifications, RTL, SoC, physical chip, and final board—validating correctness at each step.
