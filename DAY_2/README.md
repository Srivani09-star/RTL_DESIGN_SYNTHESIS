# **Day 02 — Timing Libraries, Synthesis & Flip-Flop RTL**

## **Experiment Objective**

The objective of Day 02 was to study the role of **technology libraries and timing data** in the RTL-to-gate-level synthesis process.

The experiment also focused on understanding **hierarchical versus flattened synthesis** and learning how different types of D flip-flops can be represented using Verilog RTL. The flip-flop examples covered **asynchronous reset, asynchronous set, and synchronous reset** behavior.

The designs were functionally verified using **Icarus Verilog**, their waveforms were inspected with **GTKWave**, and the RTL was synthesized and mapped using **Yosys** with the **SKY130 standard-cell library**.

---

## **Contents**

* [Technology Libraries](#1-technology-libraries)
* [Hierarchical and Flattened Synthesis](#2-hierarchical-and-flattened-synthesis)
* [Flip-Flop RTL Coding](#3-flip-flop-rtl-coding)
* [Simulation and Synthesis](#4-simulation-and-synthesis)
* [Conclusion](#5-conclusion)

---

# **1. Technology Libraries**

RTL describes the intended functionality of a digital circuit, but synthesis ultimately has to implement that functionality using physical cells supported by a particular semiconductor technology.

A **technology library** contains the information required by synthesis tools to select and analyze these cells. Depending on the library, this information can include:

* Logic functionality
* Cell timing characteristics
* Power characteristics
* Physical area
* Operating conditions
* Input and output electrical properties

For this experiment, the **SKY130 high-density standard-cell library** was used.

### **Library Used**

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
```

The name of the Liberty file identifies the technology and operating conditions represented by the library.

| **Field**  | **Description**                    |
| ---------- | ---------------------------------- |
| `sky130`   | SKY130 process technology          |
| `fd_sc_hd` | High-density standard-cell library |
| `tt`       | Typical process corner             |
| `025C`     | Operating temperature of 25°C      |
| `1v80`     | Supply voltage of 1.8 V            |

The Liberty file can be inspected from the terminal using:

```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```

By examining the file, it is possible to identify the standard cells provided by the library along with their associated timing and electrical information.

### **SKY130 Liberty File**
<img width="1646" height="819" alt="day2_fistimage" src="https://github.com/user-attachments/assets/e9dd4704-d31a-4908-9f34-d639e782285a" />

---

# **2. Hierarchical and Flattened Synthesis**

A practical RTL design is normally divided into multiple modules. During synthesis, the tool can either retain these module boundaries or remove them and create a unified representation of the logic.

This distinction is important because it influences **optimization, debugging, design visibility, and modularity**.

## **Hierarchical Synthesis**

Hierarchical synthesis maintains the relationships between the different RTL modules.

```text
                 Top Module
                 /        \
                /          \
           Module A      Module B
```

The individual modules continue to remain identifiable in the synthesized representation.

### **Advantages**

* Preserves the original modular organization.
* Makes individual design blocks easier to locate.
* Helps with debugging and design analysis.
* Supports designs built from reusable functional blocks.

---

## **Flattened Synthesis**

In flattened synthesis, the boundaries between modules are removed and their logic is combined into a common design representation.

```text
Module A ──┐
           ├──► Flattened Design
Module B ──┘
```

Since the module boundaries no longer restrict optimization, the synthesis tool can optimize logic across the original hierarchy.

### **Advantages**

* Enables optimization across module boundaries.
* Can eliminate redundant or unnecessary logic.
* Creates a unified implementation.
* Provides the synthesis tool with greater optimization freedom.

## **Comparison**

| **Feature**          | **Hierarchical**     | **Flattened**             |
| -------------------- | -------------------- | ------------------------- |
| Module hierarchy     | Preserved            | Removed                   |
| Optimization         | Mainly within blocks | Across block boundaries   |
| Debugging            | Generally easier     | Relatively more difficult |
| Design organization  | Modular              | Unified                   |
| Block identification | Straightforward      | Less direct               |

### **Hierarchical / Flattened Synthesis**
<img width="859" height="178" alt="day2_secpic" src="https://github.com/user-attachments/assets/a783f145-e609-4a34-8904-4bc99462afb3" />

---

# **3. Flip-Flop RTL Coding**

A **flip-flop** is a sequential logic element used to store one bit of information. Its output normally changes in response to a specified clock event and may also be controlled by reset or set signals.

The behavior of a flip-flop in hardware depends on how its control signals are intended to interact with the clock.

Three commonly used D flip-flop configurations were implemented during this experiment:

1. Asynchronous reset
2. Asynchronous set
3. Synchronous reset

---

## **3.1 Asynchronous Reset D Flip-Flop**

An asynchronous reset can change the flip-flop output without waiting for a clock transition.

When the reset input is asserted, the stored value is immediately cleared.

```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);

always @(posedge clk, posedge async_reset)
begin
    if (async_reset)
        q <= 1'b0;
    else
        q <= d;
end

endmodule
```

### **Operation**

* `async_reset = 1` → `q` is immediately driven to `0`
* `async_reset = 0` → `q` captures `d` at the rising edge of `clk`

The reset operation is therefore independent of the clock.

---

## **3.2 Asynchronous Set D Flip-Flop**

An asynchronous set allows the output of the flip-flop to be forced to logic `1` independently of the clock.

When the set signal becomes active, the output changes immediately.

```verilog
module dff_async_set (
    input clk,
    input async_set,
    input d,
    output reg q
);

always @(posedge clk, posedge async_set)
begin
    if (async_set)
        q <= 1'b1;
    else
        q <= d;
end

endmodule
```

### **Operation**

* `async_set = 1` → `q` immediately becomes `1`
* `async_set = 0` → `q` captures `d` on the rising edge of `clk`

The set operation does not depend on a clock edge.

---

## **3.3 Synchronous Reset D Flip-Flop**

A synchronous reset is different from an asynchronous reset because the reset condition is checked only when the active clock edge occurs.

Changing the reset input between clock edges does not directly modify the output.

```verilog
module dff_syncres (
    input clk,
    input sync_reset,
    input d,
    output reg q
);

always @(posedge clk)
begin
    if (sync_reset)
        q <= 1'b0;
    else
        q <= d;
end

endmodule
```

### **Operation**

At every rising edge of `clk`:

* `sync_reset = 1` → `q` is set to `0`
* `sync_reset = 0` → `q` captures the value of `d`

---

## **Reset Behavior Comparison**

The key difference between asynchronous and synchronous control is **when the control signal is allowed to affect the stored value**.

```text
Asynchronous Reset

Reset asserted ─────────────► Q changes immediately


Synchronous Reset

Reset asserted ─────► Clock Edge ─────► Q changes
```

| **Control Type**       | **When does it affect the output?** |
| ---------------------- | ----------------------------------- |
| **Asynchronous Reset** | Immediately when reset is asserted  |
| **Asynchronous Set**   | Immediately when set is asserted    |
| **Synchronous Reset**  | Only at the active clock edge       |

### **Flip-Flop RTL / Code**
<img width="1252" height="157" alt="day2_3" src="https://github.com/user-attachments/assets/b7f41d71-226d-49fa-9ac1-e804152efa76" />

---

# **4. Simulation and Synthesis**

After writing the flip-flop RTL, the designs were first checked through **RTL simulation** to ensure that their behavior matched the intended functionality.

Once the simulation results were verified, the RTL was processed through **Yosys synthesis** and mapped to suitable cells from the SKY130 standard-cell library.

This demonstrates the complete process of moving from a behavioral RTL description toward a technology-specific implementation.

---

## **4.1 Icarus Verilog Simulation**

The RTL source and its corresponding testbench can be compiled using **Icarus Verilog**:

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
```

After compilation, the generated simulation executable can be run with:

```bash
./a.out
```

The simulation produces a VCD waveform file, which can be opened in **GTKWave**:

```bash
gtkwave tb_dff_asyncres.vcd
```

GTKWave makes it possible to inspect the timing relationship between the **clock, reset, data input, and flip-flop output**.

The waveform can be used to confirm that the flip-flop responds correctly to both clock transitions and reset conditions.

### **Simulation Result**
<img width="1258" height="652" alt="day2_4" src="https://github.com/user-attachments/assets/b6695bf6-4a74-42f2-bad6-3847d66a2f28" />

---

## **4.2 Yosys Synthesis**

**Yosys** was used to convert the RTL description into a synthesized representation and subsequently map the inferred sequential element to a cell from the SKY130 library.

### **Launch Yosys**

```bash
yosys
```

### **Load the Technology Library**

```text
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

This makes the selected standard-cell library available to Yosys.

### **Read the RTL**

```text
read_verilog /path/to/dff_asyncres.v
```

The Verilog design is loaded into the synthesis environment.

### **Perform Synthesis**

```text
synth -top dff_asyncres
```

The `synth` command synthesizes the RTL with `dff_asyncres` specified as the top-level module.

### **Map the Flip-Flop**

```text
dfflibmap -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The `dfflibmap` command identifies an appropriate flip-flop implementation from the selected Liberty library and maps the inferred RTL flip-flop to that cell.

### **Technology Mapping**

```text
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The `abc` command performs logic optimization and technology mapping using the characteristics of the selected cell library.

### **Display the Synthesized Design**

```text
show
```

The `show` command generates a graphical representation of the synthesized circuit, allowing the resulting structure to be inspected.

### **Gate-Level Representation**
<img width="1251" height="194" alt="da2_last" src="https://github.com/user-attachments/assets/af8ea893-f911-4664-b683-cd54e0966814" />

---


## **Command Summary**

| **Command**    | **Purpose**                               |
| -------------- | ----------------------------------------- |
| `read_liberty` | Loads the standard-cell Liberty library   |
| `read_verilog` | Imports the Verilog RTL                   |
| `synth -top`   | Performs RTL synthesis                    |
| `dfflibmap`    | Maps inferred flip-flops to library cells |
| `abc`          | Optimizes and performs technology mapping |
| `show`         | Displays the synthesized circuit graph    |

---

## **RTL-to-Implementation Flow**

```text
          Flip-Flop RTL
                │
                ▼
        Icarus Verilog
                │
                ▼
          GTKWave
        Waveform Check
                │
                ▼
             Yosys
                │
                ▼
          dfflibmap
                │
                ▼
              ABC
                │
                ▼
       Technology-Mapped
          Flip-Flop
```

The flow illustrates how an RTL description is verified and progressively transformed into an implementation based on cells available in the target technology.

---

# **5. Conclusion**

Day 02 demonstrated how **technology libraries, timing information, RTL coding, simulation, and synthesis** are connected in a digital design flow.

The experiment introduced the **SKY130 Liberty library** and examined how technology, process corner, temperature, and supply voltage are represented in the library filename. The difference between **hierarchical and flattened synthesis** was also studied, highlighting the trade-off between design structure and optimization freedom.

Three D flip-flop configurations were implemented using Verilog: **asynchronous reset, asynchronous set, and synchronous reset**. Their behavior was verified through **Icarus Verilog simulation** and analyzed using **GTKWave**.

Finally, **Yosys** was used to synthesize the RTL and map the inferred flip-flop to cells from the SKY130 technology library using `dfflibmap` and `abc`.

Overall, the experiment provided a practical understanding of the path from **RTL description to simulation, synthesis, and technology-specific cell mapping**, while reinforcing the importance of timing libraries in the digital implementation process.

