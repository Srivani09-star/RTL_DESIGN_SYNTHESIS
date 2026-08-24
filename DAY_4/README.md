# **Day 04 — RTL Design, Synthesis and Gate-Level Simulation**

## **Overview**

Day 04 focused on understanding how an RTL description is transformed into a synthesized gate-level circuit and how the resulting implementation can be verified through Gate-Level Simulation (GLS).

The experiments provided practical experience with **Verilog RTL coding, functional simulation, logic synthesis, standard-cell technology mapping, gate-level netlist generation, and waveform verification**.

The session also highlighted several important RTL coding practices and their impact on simulation results. Particular attention was given to **ternary-based MUX design, incomplete sensitivity lists, blocking assignments, combinational coding using `always @(*)`, and differences between RTL and gate-level simulation**.

### **Topics Covered**

* 2:1 MUX using the ternary operator
* RTL functional simulation
* Yosys-based synthesis
* Standard-cell technology mapping
* Gate-level netlist generation
* Gate-Level Simulation
* Incomplete sensitivity lists
* Blocking assignments
* Blocking versus non-blocking assignments
* RTL and GLS waveform comparison
* Simulation-synthesis mismatches
* Best practices for combinational RTL

---

## **Table of Contents**

1. [RTL-to-Gate-Level Verification Flow](#1-rtl-to-gate-level-verification-flow)
2. [MUX Using the Ternary Operator](#2-mux-using-the-ternary-operator)

   * [Operating Principle](#21-operating-principle)
   * [RTL Verification](#22-rtl-verification)
   * [Logic Synthesis](#23-logic-synthesis)
   * [Gate-Level Verification](#24-gate-level-verification)
3. [MUX with an Incomplete Sensitivity List](#3-mux-with-an-incomplete-sensitivity-list)

   * [Coding Issue](#31-coding-issue)
   * [RTL Simulation](#32-rtl-simulation)
   * [Synthesis and GLS](#33-synthesis-and-gls)
   * [Recommended Coding Style](#34-recommended-coding-style)
4. [Blocking Assignment Considerations](#4-blocking-assignment-considerations)

   * [Blocking Assignment](#41-blocking-assignment)
   * [RTL Simulation](#42-rtl-simulation)
   * [Synthesis](#43-synthesis)
   * [Gate-Level Simulation](#44-gate-level-simulation)
5. [Blocking and Non-Blocking Assignments](#5-blocking-and-non-blocking-assignments)
6. [Role of `always @(*)`](#6-role-of-always)
7. [Understanding Simulation-Synthesis Mismatch](#7-understanding-simulation-synthesis-mismatch)
8. [RTL Simulation and Gate-Level Simulation](#8-rtl-simulation-and-gate-level-simulation)
9. [Tools Used](#9-tools-used)
10. [Important Observations](#10-important-observations)
11. [Learning Outcomes](#11-learning-outcomes)
12. [Conclusion](#12-conclusion)

---

# **1. RTL-to-Gate-Level Verification Flow**

The experiments performed on Day 04 followed a sequence that begins with behavioral RTL and ends with verification of the synthesized circuit.

```text
        Verilog RTL
             |
             v
       RTL Simulation
             |
             v
       Yosys Synthesis
             |
             v
    Technology Mapping
             |
             v
    Gate-Level Netlist
             |
             v
   Gate-Level Simulation
             |
             v
         GTKWave
             |
             v
     Waveform Analysis
```

At the RTL stage, the objective is to verify whether the code implements the intended functionality.

After synthesis, the RTL is converted into a network of logic cells from the selected technology library. The resulting gate-level netlist is then simulated to check whether the synthesized implementation continues to exhibit the expected behaviour.

This provides an important verification path between the **RTL description and the synthesized hardware structure**.

---

# **2. MUX Using the Ternary Operator**

## **2.1 Operating Principle**

A multiplexer is a combinational circuit that selects one of several input signals and forwards the selected signal to its output.

For a 2:1 MUX, two data inputs are controlled by a single select signal.

```text
i0 --------\
            \
             >------ y
            /
i1 --------/
          |
         sel
```

The output relationship is:

```text
sel = 0  →  y = i0
sel = 1  →  y = i1
```

In Verilog, the same functionality can be expressed concisely using the ternary operator:

```verilog
assign y = sel ? i1 : i0;
```

The expression evaluates the select signal and forwards the appropriate input to `y`.

This makes the ternary operator particularly convenient for describing simple combinational selection logic.

---

## **2.2 RTL Verification**

The MUX was initially verified through RTL simulation before performing synthesis.

The important signals observed during simulation were:

```text
i0
i1
sel
y
```

The expected relationship was verified by changing the select signal and data inputs.

* When `sel` is `0`, the output follows `i0`.
* When `sel` is `1`, the output follows `i1`.

### **RTL Waveform**
<img width="1323" height="616" alt="image" src="https://github.com/user-attachments/assets/739a191f-998d-4cc7-b330-ca8e22ab7f32" />

[Ternary MUX RTL Waveform](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/ternary_mux_rtl.png)

The waveform confirms that the RTL implementation performs the expected input selection.

---

## **2.3 Logic Synthesis**

After functional verification, the MUX was synthesized using **Yosys**.

During synthesis, the high-level RTL expression was converted into an implementation based on cells available in the **SKY130 standard-cell library**.

The MUX was mapped to:

```text
sky130_fd_sc_hd__mux2_1
```

### **Synthesized Netlist**
<img width="777" height="372" alt="image" src="https://github.com/user-attachments/assets/24c5f7b0-6c80-4c97-8d99-095bf6214379" />

[Ternary MUX Synthesized Netlist](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/ternary_mux_netlist.png)

The generated netlist illustrates the transformation from an RTL statement into a technology-specific hardware structure.

The original RTL:

```verilog
assign y = sel ? i1 : i0;
```

is interpreted by the synthesis tool and implemented using a suitable standard cell.

---

## **2.4 Gate-Level Verification**

The synthesized netlist was subsequently used as the design under test for Gate-Level Simulation.

The same input combinations used during functional verification were applied through the testbench. The resulting signals were then inspected using GTKWave.

### **Gate-Level Waveform**
<img width="1325" height="640" alt="image" src="https://github.com/user-attachments/assets/738215c9-9a38-45ce-83f8-30c94fffc26b" />

[Ternary MUX Gate-Level Waveform](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/ternary_mux_gls.png)

Comparing the RTL waveform with the gate-level waveform helps confirm that synthesis has preserved the intended MUX functionality.

The overall process can be summarized as:

```text
        RTL MUX
           |
           v
   Ternary Expression
           |
           v
     Yosys Synthesis
           |
           v
sky130_fd_sc_hd__mux2_1
           |
           v
 Gate-Level Simulation
```

---

# **3. MUX with an Incomplete Sensitivity List**

## **3.1 Coding Issue**

A combinational MUX can also be implemented using a procedural `always` block.

For example:

```verilog
always @(sel)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

Although this code describes the intended MUX relationship, the sensitivity list contains only `sel`.

The output is actually dependent on three signals:

```text
sel
i0
i1
```

Therefore, if `i0` or `i1` changes while `sel` remains constant, the procedural block may not be triggered during RTL simulation.

This can cause the simulated output to remain unchanged even though the corresponding hardware logic should respond to the input change.

---

## **3.2 RTL Simulation**

The incorrectly coded MUX was simulated to demonstrate the effect of the incomplete sensitivity list.

### **RTL Waveform**
<img width="1270" height="607" alt="image" src="https://github.com/user-attachments/assets/e5396feb-34e9-4550-8505-7a1f1ae2aac2" />

[Bad MUX RTL Waveform](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/bad_mux_rtl.png)

The waveform illustrates that a change in one of the data inputs may not propagate to the simulated output if the select signal does not change.

The reason is that the simulator evaluates the `always` block only when an event occurs on a signal present in its sensitivity list.

This demonstrates why sensitivity lists must correctly represent all signals that influence combinational logic.

---

## **3.3 Synthesis and GLS**

An important point is that the sensitivity list is primarily associated with **simulation behaviour**. It is not a physical hardware element.

During synthesis, the synthesis tool examines the actual logical relationships described by the RTL and attempts to generate the corresponding hardware.

Consequently, the RTL simulation of an incorrectly specified sensitivity list can differ from the behaviour of the synthesized circuit.

### **Gate-Level Waveform**
<img width="1273" height="592" alt="image" src="https://github.com/user-attachments/assets/95a55910-3bf2-4a72-b76d-72a1ae7a1784" />

[Bad MUX Gate-Level Waveform](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/bad_mux_gls.png)

The gate-level waveform can be compared with the RTL waveform to identify the difference.

This experiment demonstrates how an RTL coding mistake can introduce a **simulation-synthesis mismatch**.

---

## **3.4 Recommended Coding Style**

For combinational procedural logic, the following form is preferred:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

The `@(*)` construct automatically includes the signals referenced by the procedural block in its sensitivity mechanism.

Thus, instead of manually writing:

```verilog
always @(sel)
```

the safer approach is:

```verilog
always @(*)
```

This significantly reduces the possibility of accidentally leaving an input signal out of the sensitivity list.

---

# **4. Blocking Assignment Considerations**

## **4.1 Blocking Assignment**

Verilog provides two major procedural assignment operators:

```text
Blocking assignment       =
Non-blocking assignment   <=
```

A blocking assignment updates the left-hand side immediately within the current procedural execution.

Consider:

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

Here, the assignment to `x` occurs before the statement calculating `d`.

Therefore, the second statement can use the newly calculated value of `x`.

This means that **statement order matters** when blocking assignments are used.

Blocking assignments are commonly associated with combinational procedural logic.

---

## **4.2 RTL Simulation**

The blocking-assignment example was first examined through RTL simulation.

The logic can be represented as:

```text
a ----\
       OR ---- x ----\
b ----/              AND ---- d
                     /
c ------------------/
```

### **RTL Waveform**
<img width="1258" height="570" alt="image" src="https://github.com/user-attachments/assets/82de7ad1-5260-4bc7-aaff-cffe6aac7850" />

[Blocking Assignment RTL Waveform](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/blocking_caveat_rtl.png)

The waveform provides a visual representation of the intermediate signal `x` and the resulting output `d`.

Since blocking assignments execute in procedural order, the updated value of `x` becomes available to the following statement.

---

## **4.3 Synthesis**

The RTL was synthesized using **Yosys** and mapped using the SKY130 standard-cell library.

One of the relevant cells generated during synthesis was:

```text
sky130_fd_sc_hd__o21a_1
```

### **Synthesized Netlist**
<img width="1309" height="538" alt="image" src="https://github.com/user-attachments/assets/9ae26d1f-a1d2-4e06-bf86-3d70aee055e5" />

[Blocking Assignment Synthesized Netlist](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/blocking_caveat_netlist.png)

The synthesized circuit represents the combinational logic inferred from the RTL.

The procedural statements are transformed by the synthesis tool into connections between standard cells that implement the required Boolean functionality.

---

## **4.4 Gate-Level Simulation**

The synthesized netlist was then subjected to Gate-Level Simulation.

### **Gate-Level Waveform**
<img width="1274" height="610" alt="image" src="https://github.com/user-attachments/assets/b6d84540-8a9d-43fd-80ae-2ebf16f42fb0" />

[Blocking Assignment Gate-Level Waveform](https://github.com/RTL_Design_Workshop/blob/main/Day_4/images/blocking_caveat_gls.png)

The GLS waveform represents the behaviour of the synthesized implementation.

By comparing the RTL and GLS results, it becomes easier to understand the distinction between **procedural execution in RTL simulation and the actual hardware structure produced by synthesis**.

The concept can be summarized as:

```text
       Blocking Assignment
                |
                v
      Sequential Statement
          Evaluation
                |
                v
      Intermediate Value
            Updated
                |
                v
      Next Statement Uses
       Updated Value
```

This highlights why the order of blocking assignments should be considered carefully when describing combinational logic.

---

# **5. Blocking and Non-Blocking Assignments**

### **Blocking Assignment**

The blocking assignment operator is:

```verilog
=
```

Example:

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

The statements execute in sequence, and the updated value of `x` can immediately be used by the following statement.

Blocking assignments are generally preferred when describing combinational procedural logic.

### **Non-Blocking Assignment**

The non-blocking assignment operator is:

```verilog
<=
```

Example:

```verilog
always @(posedge clk)
begin
    q <= d;
end
```

With a non-blocking assignment, the update is scheduled rather than taking effect immediately within the procedural sequence.

This makes non-blocking assignments well suited to modeling clocked sequential elements such as flip-flops and registers.

| **Characteristic**            | **Blocking `=`**                      | **Non-Blocking `<=`**         |
| ----------------------------- | ------------------------------------- | ----------------------------- |
| Update behaviour              | Immediate within procedural execution | Update is scheduled           |
| Execution                     | Sequential                            | Evaluated and updated later   |
| Typical application           | Combinational logic                   | Sequential logic              |
| Importance of statement order | High                                  | Different execution semantics |
| Common RTL usage              | `always @(*)`                         | `always @(posedge clk)`       |

---

# **6. Role of `always @(*)`**

When combinational logic is implemented inside an `always` block, every input that can influence the output needs to be considered for triggering the block.

An incomplete sensitivity list such as:

```verilog
always @(sel)
```

can cause simulation results that do not accurately represent the intended combinational hardware.

The recommended form is:

```verilog
always @(*)
```

For example:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

With `always @(*)`, changes in `sel`, `i0`, or `i1` can cause the block to execute.

This coding style therefore helps prevent simulation errors caused by manually maintaining an incomplete sensitivity list.

---

# **7. Understanding Simulation-Synthesis Mismatch**

A **simulation-synthesis mismatch** occurs when the behaviour observed during RTL simulation does not agree with the behaviour represented by the synthesized hardware.

The experiments performed on Day 04 demonstrated two important examples.

### **Incomplete Sensitivity List**

```verilog
always @(sel)
```

The RTL simulator reacts to changes in `sel`, but the output also depends on `i0` and `i1`.

As a result, the simulation may not update correctly when only a data input changes.

The synthesized circuit, however, is based on the logical relationship between all relevant inputs and the output.

### **Blocking Assignment**

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

The statements execute sequentially during RTL simulation because blocking assignments are used.

This means the order in which the statements are written can influence intermediate values during simulation.

The general verification process is:

```text
        RTL Coding
            |
            v
      RTL Simulation
            |
            v
         Synthesis
            |
            v
    Hardware Implementation
            |
            v
    Gate-Level Simulation
            |
            v
     Compare Results
```

Careful RTL coding is therefore essential for ensuring that simulation results correctly represent the intended synthesized hardware.

---

# **8. RTL Simulation and Gate-Level Simulation**

| **Parameter**          | **RTL Simulation**      | **Gate-Level Simulation**        |
| ---------------------- | ----------------------- | -------------------------------- |
| Design input           | RTL source              | Synthesized netlist              |
| Processing stage       | Before synthesis        | After synthesis                  |
| Primary objective      | Functional verification | Post-synthesis verification      |
| Circuit representation | Behavioural/RTL model   | Standard-cell implementation     |
| Timing information     | Generally abstracted    | May include cell and gate delays |
| Simulator              | Icarus Verilog          | Icarus Verilog                   |
| Waveform analysis      | GTKWave                 | GTKWave                          |

RTL simulation is mainly used to determine whether the original design specification has been correctly represented in Verilog.

Gate-Level Simulation provides an additional verification stage by running the synthesized netlist.

Comparing the two levels of simulation helps identify whether synthesis has preserved the intended functionality.

---

# **9. Tools Used**

| **Tool**           | **Purpose**                                  |
| ------------------ | -------------------------------------------- |
| **Yosys**          | RTL synthesis and netlist generation         |
| **Icarus Verilog** | Verilog compilation and simulation           |
| **GTKWave**        | Waveform visualization and analysis          |
| **SKY130 PDK**     | Standard-cell library for technology mapping |

---

# **10. Important Observations**

### **Ternary MUX**

```text
       RTL Description
             |
             v
assign y = sel ? i1 : i0;
             |
             v
       Yosys Synthesis
             |
             v
sky130_fd_sc_hd__mux2_1
             |
             v
    Gate-Level Simulation
```

The ternary operator provides a concise and readable way of representing a 2:1 multiplexer.

### **Incomplete Sensitivity List**

```verilog
always @(sel)
```

This implementation does not include every signal that affects the output.

The preferred combinational form is:

```verilog
always @(*)
```

This allows the simulator to respond to changes in all referenced input signals.

### **Blocking Assignment**

```verilog
x = a | b;
d = x & c;
```

Blocking assignments execute in procedural order, allowing the updated intermediate value to be used immediately by the following statement.

### **Gate-Level Simulation**

```text
       RTL
        |
        v
    Synthesis
        |
        v
 Gate-Level Netlist
        |
        v
 Gate-Level Simulation
        |
        v
 Waveform Analysis
```

GLS serves as an additional verification stage between synthesis and final hardware implementation.

---

# **11. Learning Outcomes**

After completing Day 04, I developed a better understanding of:

* RTL-to-gate-level verification methodology
* 2:1 MUX implementation using the ternary operator
* RTL functional simulation
* Logic synthesis using Yosys
* Standard-cell technology mapping
* Gate-level netlist generation
* Gate-Level Simulation
* Waveform inspection using GTKWave
* Verilog sensitivity lists
* Correct use of `always @(*)`
* Blocking assignment behaviour
* Non-blocking assignment behaviour
* RTL versus gate-level verification
* Simulation-synthesis mismatches
* Combinational RTL coding practices
* The relationship between RTL code and synthesized hardware

The practical experiments helped connect Verilog coding concepts with actual simulation waveforms and synthesized circuit structures.

---

# **12. Conclusion**

Day 04 provided hands-on experience with the transition from **RTL design to synthesized hardware and Gate-Level Simulation**.

The ternary MUX experiment demonstrated how a compact Verilog expression can be synthesized into a corresponding standard-cell implementation. The incomplete sensitivity-list experiment highlighted an important RTL coding issue that can cause RTL simulation to behave differently from the synthesized circuit.

The blocking-assignment experiment further illustrated the importance of procedural statement ordering and helped clarify the distinction between RTL execution semantics and the resulting hardware.

Through the combined use of **Icarus Verilog, GTKWave, Yosys, and the SKY130 standard-cell library**, the complete path from RTL description to gate-level verification was explored.

Overall, Day 04 strengthened my understanding of **synthesizable Verilog, combinational coding practices, technology mapping, gate-level netlists, waveform analysis, and simulation-synthesis consistency**.

