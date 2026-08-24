

# **Day 01 — RTL Design, Simulation & Synthesis**

## **Experiment Objective**

The main goal of Day 01 was to develop a basic understanding of **RTL design, Verilog-based simulation, and digital design verification**.

The experiment followed the RTL design flow starting from writing the Verilog module and testbench, compiling the design, running simulations, observing signal waveforms, and finally synthesizing the RTL into a gate-level implementation.

A **2:1 Multiplexer** was selected as the design example to demonstrate these concepts practically.

---

## **Contents**

* [Digital Design Verification](#1-digital-design-verification)
* [Simulation Workflow with Icarus Verilog](#2-simulation-workflow-with-icarus-verilog)
* [Practical Exercise – 2:1 Multiplexer](#3-practical-exercise--21-multiplexer)
* [Multiplexer RTL Design](#4-multiplexer-rtl-design)
* [Introduction to Yosys](#5-introduction-to-yosys)
* [RTL Design and Synthesis](#6-rtl-design-and-synthesis)
* [Understanding `.lib` Files and Cell Flavors](#7-understanding-lib-files-and-cell-flavors)
* [Yosys Synthesis of the Multiplexer](#8-yosys-synthesis-of-the-multiplexer)
* [Synthesis Results and Gate-Level Representation](#9-synthesis-results-and-gate-level-representation)
* [Generated Gate-Level Netlist](#10-generated-gate-level-netlist)
* [Conclusion](#11-conclusion)

---

# **1. Digital Design Verification**

Digital hardware should be functionally verified before moving toward physical implementation. Simulation provides a way to check the expected behavior of a circuit without requiring actual hardware.

A basic Verilog verification environment generally consists of three important elements:

**Design + Testbench + Simulator**

### **Simulator**

A simulator runs the Verilog description and models the behavior of the digital circuit over time. It makes it possible to apply different input conditions and examine how the outputs respond.

### **Design**

The design is the RTL module that represents the intended hardware functionality. It defines the required inputs, outputs, and logic relationships of the circuit.

### **Testbench**

A testbench is written specifically for verification. It generates test inputs, applies them to the design under test, and observes the resulting outputs.


### **Verification Flow**

```text
      RTL Design
          +
      Testbench
          ↓
      Simulator
          ↓
  Simulation Results
          ↓
 Waveform Observation
```

### **Testbench**
<img width="1227" height="709" alt="testbench" src="https://github.com/user-attachments/assets/004cff17-9d91-4483-bfbf-0b3b284859b0" />


---

# **2. Simulation Workflow with Icarus Verilog**

**Icarus Verilog (`iverilog`)** is a freely available Verilog compiler and simulator used for RTL functional verification.

It compiles the design and testbench and executes the specified simulation. During simulation, signal transitions can be stored in a **Value Change Dump (`.vcd`)** file.

The generated VCD file can be opened in **GTKWave**, allowing the behavior of individual signals to be examined graphically.

## **Simulation Flow**

```text
   Verilog RTL
       +
   Testbench
       ↓
 Icarus Verilog
       ↓
   Simulation
       ↓
    .vcd File
       ↓
    GTKWave
       ↓
 Waveform Analysis
```

### **Simulation Flow Diagram**

The simulation process demonstrates how the Verilog source files are compiled and executed before the resulting signal activity is examined using GTKWave.

---<img width="1263" height="778" alt="simulationflow" src="https://github.com/user-attachments/assets/9ab660cf-935d-49e4-b296-ceca83a61a30" />


# **3. Practical Exercise — 2:1 Multiplexer**

For the practical implementation, a **2:1 Multiplexer** was chosen because it provides a simple example for understanding RTL coding, simulation, and synthesis.

The design was simulated using **Icarus Verilog**, while **GTKWave** was used to inspect the generated signal waveforms.

## **Step 1 — Install Required Tools**

The required simulation and waveform-analysis tools can be installed on Ubuntu using:

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

---

## **Step 2 — Compile the Design**

The RTL design and its corresponding testbench are compiled using:

```bash
iverilog good_mux.v tb_good_mux.v
```

This command processes both Verilog files and generates the simulation executable.

---

## **Step 3 — Run the Simulation**

The compiled simulation can be executed with:

```bash
./a.out
```

Running the executable starts the testbench and generates the waveform dump file specified in the testbench.

---

## **Step 4 — Open the Waveform**

The generated VCD file can be loaded into GTKWave using:

```bash
gtkwave tb_good_mux.vcd
```

GTKWave provides a graphical view of the input, select, and output signals, making it easier to verify the functional behavior of the multiplexer.

### **GTKWave Output**

The waveform can be inspected to confirm that the multiplexer output changes according to the selected input.

<img width="960" height="1020" alt="muxtbscreenshot" src="https://github.com/user-attachments/assets/fbef3598-e00e-421b-927b-89d7089ab3be" />

---

# **4. Multiplexer RTL Design**

The following Verilog module represents the functionality of the 2:1 Multiplexer.

## **Verilog Implementation**

```verilog
module good_mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*)
begin
    if (sel)
        y <= i1;
    else
        y <= i0;
end

endmodule
```

## **Signal Description**

| **Signal** | **Description**         |
| ---------- | ----------------------- |
| `i0`       | First data input        |
| `i1`       | Second data input       |
| `sel`      | Input selection control |
| `y`        | Multiplexer output      |

## **Operation**

The value of `sel` determines which data input is connected to the output.

* `sel = 0` → `y = i0`
* `sel = 1` → `y = i1`

Therefore, the circuit forwards one of the two input signals depending on the selection control.

### **Verilog Code**

The RTL implementation shown above describes the required combinational behavior of the 2:1 multiplexer.
<img width="600" height="180" alt="MUXcode" src="https://github.com/user-attachments/assets/ea1f8149-e824-4801-b2ba-aca8e3182778" />

---

# **5. Introduction to Yosys**

**Yosys** is an open-source RTL synthesis framework used to convert hardware descriptions written in Verilog into a synthesized logic representation.

During synthesis, the behavioral RTL is analyzed and transformed into logic structures that can subsequently be mapped to cells available in a target technology library.

## **Basic Yosys Flow**

```text
   RTL Design
       ↓
 Technology Library
       ↓
      Yosys
       ↓
 Logic Synthesis
       ↓
Technology Mapping
       ↓
 Gate-Level Netlist
```

A basic synthesis sequence can be performed using:

```text
read_liberty -lib <library>.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty <library>.lib
write_verilog synthesized_mux.v
```

The resulting synthesized Verilog represents the circuit after logic optimization and technology mapping.

### **Yosys Output**

The Yosys output provides information about the synthesis process and the resulting hardware structure.
<img width="1256" height="695" alt="yoyssetuo (1)" src="https://github.com/user-attachments/assets/b8086b30-5f1c-4a39-9048-a57cdbe24aeb" />
<img width="1250" height="703" alt="yosyssim" src="https://github.com/user-attachments/assets/80cd7d0c-02da-4afb-8bbe-2b97af8cd152" />

---

# **6. RTL Design and Synthesis**

Synthesis acts as the transition point between the **RTL description and the gate-level implementation**.

At the RTL level, the designer focuses on describing the required functionality. During synthesis, that description is transformed into a logic structure suitable for implementation using cells from the target technology.

```text
      RTL Design
          ↓
       Synthesis
          ↓
    Logic Structure
          ↓
  Technology Mapping
          ↓
  Gate-Level Netlist
```

## **RTL vs Gate-Level Design**

| **RTL Design**                                     | **Gate-Level Design**                                |
| -------------------------------------------------- | ---------------------------------------------------- |
| Describes the intended circuit behavior using HDL. | Represents the synthesized hardware implementation.  |
| Relatively simple to develop and modify.           | Contains gates or technology-specific cells.         |
| Commonly used for functional verification.         | Used for implementation and post-synthesis analysis. |

### **Gate-Level Representation**

After synthesis and technology mapping, the RTL description is converted into a lower-level representation consisting of logic cells and their interconnections.

This representation provides a closer view of how the described functionality can be implemented in hardware.
<img width="975" height="623" alt="yosyssynthesis" src="https://github.com/user-attachments/assets/350702b6-473f-4e4a-aa30-1942698cd30c" />

---

# **7. Understanding `.lib` Files and Cell Flavors**

A **Liberty (`.lib`) file** describes the characteristics of standard cells provided by a semiconductor technology library.

It contains information that synthesis and timing tools can use when selecting appropriate cells for a design.

Typical information available in a Liberty library includes:

* Cell functionality
* Timing characteristics
* Power information
* Area information
* Operating conditions
* Input characteristics
* Output characteristics
* Delay information

Technology libraries often provide multiple versions of similar cells. These different cell variants allow the implementation tools to make trade-offs between performance, power, and area.

## **Faster and Slower Cell Variants**

### **Faster Cells**

Faster cells are designed to provide improved timing performance.

They are generally useful when:

* A path has a strict timing requirement.
* Propagation delay needs to be reduced.
* Performance is more important than minimum area or power.

However, faster cells can involve increased **power consumption and area**.

### **Slower Cells**

Slower cell variants have greater delay but may be suitable for paths where timing is not critical.

They can be useful when:

* Timing constraints are relaxed.
* Lower power is preferred.
* Area optimization is important.

Therefore, selecting cells is an optimization problem involving **timing, power, and area**, rather than simply choosing the fastest available cell.

---

# **8. Yosys Synthesis of the Multiplexer**

Yosys can be started from the Linux terminal with:

```bash
yosys
```

### **Launching Yosys**

Once Yosys is started, synthesis commands can be entered through its interactive shell.
<img width="744" height="160" alt="yosysinvoke" src="https://github.com/user-attachments/assets/dd3ddbce-6139-4e68-b1f0-92f4fc9e9748" />

The `good_mux` module was synthesized using the following sequence:

```text
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr good_mux_net.v
```

## **Command Breakdown**

| **Command**     | **Purpose**                                                  |
| --------------- | ------------------------------------------------------------ |
| `read_liberty`  | Imports the selected technology library.                     |
| `read_verilog`  | Loads the RTL Verilog source.                                |
| `synth -top`    | Performs synthesis for the specified top-level module.       |
| `abc`           | Maps the synthesized logic to cells from the target library. |
| `write_verilog` | Writes the resulting gate-level netlist to a Verilog file.   |

The synthesized output can then be examined to understand the resulting implementation.

---

# **9. Synthesis Results and Gate-Level Representation**

Once synthesis is complete, Yosys generates statistics describing the synthesized design.

These reports can provide details about the number of ports, wires, cells, and other elements present in the resulting circuit.

### **Synthesis Statistics**

The synthesis statistics provide an overview of how the original RTL was transformed during the synthesis process.
<img width="975" height="623" alt="yosyssynthesis" src="https://github.com/user-attachments/assets/a8cc199c-855e-4093-b6a4-db6a42e18b5b" />

The synthesized circuit can also be viewed graphically by using:

```text
show
```

This command generates a visual representation of the synthesized logic, making it easier to understand the resulting circuit structure.

### **Gate-Level Logic**

The gate-level representation shows the logic elements and their connections after synthesis.

---

# **10. Generated Gate-Level Netlist**

The synthesized Verilog netlist is generated with:

```text
write_verilog -noattr good_mux_net.v
```

The resulting file can be inspected from the Linux terminal using:

```bash
cat good_mux_net.v
```

The generated netlist represents the original multiplexer functionality using cells selected from the specified technology library.

This provides a useful connection between the high-level RTL description and the actual technology-specific implementation.

### **Gate-Level Logic**

The resulting synthesized logic can be examined to understand how the multiplexer functionality has been represented using standard cells.
<img width="1254" height="407" alt="yosysgoodmux" src="https://github.com/user-attachments/assets/3d13992f-5488-40df-ad60-eaa2f4eaddb1" />

---

# **11. Conclusion**

Day 01 introduced the fundamental stages involved in moving from **Verilog RTL to a synthesized gate-level design**.

Through the 2:1 Multiplexer experiment, I gained practical experience in creating RTL and testbench files, compiling them with **Icarus Verilog**, running simulations, and examining signal transitions using **GTKWave**.

The session also introduced **Yosys synthesis**, technology mapping, Liberty (`.lib`) files, standard-cell variants, synthesis statistics, and gate-level netlist generation.

Overall, this experiment provided a practical understanding of how a simple RTL description can progress through **simulation, synthesis, technology mapping, and finally into a gate-level hardware representation**.

