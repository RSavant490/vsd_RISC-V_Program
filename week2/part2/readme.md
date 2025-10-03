## **Task 2 – BabySoC Simulation**

### **Objective**

* Simulate the BabySoC design (VSDBabySoC) using Icarus Verilog.
* Generate waveforms to visualize the digital signals.
* Understand the interaction between RVMYTH (CPU), PLL, and DAC.

---

### **Step 1: Directory & Files Setup**

Make sure your project structure is like this:

```
VSDBabySoC/
├── src/
│   ├── include/
│   │   └── header files (.vh)
│   └── module/
│       ├── vsdbabysoc.v
│       ├── rvmyth.v
│       ├── avsdpll.v
│       ├── avsddac.v
│       └── testbench.v
└── output/
```

* `testbench.v` is the top-level simulation file.
* `output/` will hold compiled binaries and waveform `.vcd` files.

---

### **Step 2: Compile with Icarus Verilog**

From the root directory (`VSDBabySoC`):

```bash
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM -DPRE_SYNTH_SIM \
    -I src/include -I src/module     src/module/testbench.v 
```

**Explanation:**

* `-o output/pre_synth_sim.out`: Compiled executable path.
* `-DPRE_SYNTH_SIM`: Preprocessor flag for simulation.
* `-I src/include -I src/module`: Include directories for Verilog headers & modules.
* Last arguments: all Verilog source files needed for simulation.

---

### **Step 3: Run the Simulation**

After compilation, run:

```bash
cd output/pre_synth_sim
./output/pre_synth_sim.out
```
---

### **Step 4: View Waveforms in GTKWave**

Open the waveform:

```bash
gtkwave output/pre_synth_sim.vcd
```
<p align="center">
   <img src="images/waveform.png" alt="BabySoC Waveform" width="600"/>
</p>
* GTKWave GUI opens, showing all signals.
* Typical signals to analyze:

  * `CLK`: Clock from PLL
  * `RV_TO_DAC[9:0]`: Digital values from RVMYTH
  * `OUT`: Analog signal from DAC (if using real-valued simulation)

**Tip:** You can select, zoom, and add signals in GTKWave to study transitions.
---
