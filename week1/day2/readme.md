# Day 2 – Timing Libraries and Synthesis (Hierarchical vs Flattened)

## SKY130 PDK Overview
The **SKY130 Process Design Kit (PDK)** is an open-source library provided for 130nm CMOS technology.  
It includes:
- **Standard cell libraries** (NAND, NOR, Flip-Flops, etc.)
- **Timing information** for different process, voltage, and temperature (PVT) conditions
- **Power and area characterization**

This information is stored inside `.lib` (Liberty) files.

---

## Decoding `tt_025C_1v80` in the SKY130 PDK
The library name has important information about the PVT (Process, Voltage, Temperature) conditions:

- **tt** → *Typical-Typical process corner*  
  - `ss` = Slow-Slow  
  - `ff` = Fast-Fast  
- **025C** → Temperature of characterization (25°C here)  
- **1v80** → Voltage used for characterization (1.8 V)

**P, V, T variations** affect performance:
- **P (Process):** Variations due to fabrication  
- **V (Voltage):** Supply voltage differences  
- **T (Temperature):** Environmental temperature  

Together, these determine **delay, power, and leakage** of a design.

---

## Opening and Exploring the `.lib` File
The `.lib` file contains **cell-level information** like delay, power, area, and timing models.

Open the file:
```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
````

Tips:

* `:se nu` → Show line numbers
* `:syntax off` → Disable syntax highlighting (in gvim)
* Editing is **not allowed**, as these are reference libraries.

### Example Cell Entry

```liberty
cell(sky130_fd_sc_hd__a2111o_1) {
    area : ...;
    pin(...) { ... }
    power(...) { ... }
    timing(...) { ... }
}
```

* `a2111o` → A logic cell (2-input AND + 3-input OR)
* `_0, _1, _2, ...` → Different drive strengths

  * Higher number = Wider transistor, larger area, higher power, but less delay
* Example:

  * Cell with 5 pins → `2^5 = 32` input combinations
  * `.lib` stores leakage power, timing, and power info for each combination

---

## Hierarchical vs Flattened Synthesis

### Example Verilog Design

```verilog
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
    assign y = a & b;
endmodule

module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a), .b(b), .y(net1));  // net1 = a & b
    sub_module2 u2(.a(net1), .b(c), .y(y));  // y = (a & b) | c
endmodule
```

---

### Hierarchical Synthesis

In **hierarchical synthesis**, Yosys keeps module boundaries. Each submodule is preserved, and connected together in the top module.

#### Commands:

```tcl
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules_hier.v
!gedit multiple_modules_hier.v
```

* **Result:**

  * Netlist is generated **per module**
  * Submodules are **visible separately**
  * Useful for debugging

📌 Screenshot was taken here (hierarchical diagram).

---

### Flattened Synthesis

In **flattened synthesis**, Yosys removes hierarchy. All submodules are merged into a **single module** of gates.

This makes it easier for tools to optimize globally, but module boundaries are lost.

#### Commands:

```tcl
flatten
write_verilog -noattr multiple_modules_flat.v
!gedit multiple_modules_flat.v
show
```

* **Result:**

  * Only one module (no submodule separation)
  * Fully optimized at gate level
  * Easier for backend tools, but harder for debugging

📌 Screenshot was taken here (flattened diagram).

---

## Key Differences: Hierarchical vs Flattened

| Aspect           | Hierarchical Synthesis | Flattened Synthesis |
| ---------------- | ---------------------- | ------------------- |
| **Structure**    | Preserves modules      | Single flat module  |
| **Debugging**    | Easier (module-wise)   | Harder (all merged) |
| **Optimization** | Limited (per module)   | Global optimization |
| **Netlist**      | Modular netlist        | Flat gate netlist   |

---
