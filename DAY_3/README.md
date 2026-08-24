# **Day 03 — RTL Optimization & Synthesis**

## **Experiment Objective**

The objective of Day 3 is to understand how synthesis tools analyze, optimize, and transform RTL descriptions into efficient gate-level hardware implementations.

The experiments focus on Boolean logic optimization, constant propagation, D flip-flop optimization, sequential logic optimization, and counter optimization. The synthesized designs are analyzed to understand how Yosys simplifies RTL and maps the optimized logic to hardware structures.

---

## **Table of Contents**

* [Objective](#objective)
* [1. RTL Optimization](#1-rtl-optimization)
* [2. Logic Optimization](#2-logic-optimization)

  * [AND Logic](#and-logic)
  * [OR Logic](#or-logic)
  * [Three-Input AND Logic](#three-input-and-logic)
* [3. Constant Propagation](#3-constant-propagation)
* [4. D Flip-Flop Optimization](#4-d-flip-flop-optimization)

  * [DFF Constant 1](#dff-constant-1)
  * [DFF Constant 2](#dff-constant-2)
  * [DFF Constant 3](#dff-constant-3)
* [5. Counter Optimization](#5-counter-optimization)
* [6. Importance of Optimization](#6-importance-of-optimization)
* [Key Observations](#key-observations)
* [Conclusion](#conclusion)

---

# **1. RTL Optimization**

RTL optimization is the process of improving the hardware implementation of an RTL design while preserving its intended functionality.

An RTL description defines the required behavior of a digital circuit, but the same functionality can often be implemented using different hardware structures. During synthesis, the tool analyzes the RTL and applies optimization techniques to generate a more efficient implementation.

Common optimization techniques include:

* Boolean logic simplification
* Constant propagation
* Removal of redundant logic
* Elimination of unused signals
* Logic restructuring
* Technology mapping
* Sequential logic optimization

The main purpose of optimization is to achieve the required functionality using an efficient hardware structure.

RTL optimization can directly influence important design parameters such as:

* **Area**
* **Power consumption**
* **Timing**
* **Standard-cell count**
* **Switching activity**

Therefore, optimization plays an important role in converting an RTL design into an implementation suitable for further physical design.

---

# **2. Logic Optimization**

Logic optimization focuses on simplifying combinational logic without changing its logical behavior.

Synthesis tools use Boolean algebra and other optimization techniques to identify unnecessary logic and generate an efficient implementation.

Some basic Boolean identities used during optimization are:

```text
A · 1 = A
A · 0 = 0
A + 0 = A
A + 1 = 1
```

These identities allow synthesis tools to simplify expressions before mapping them to the target technology.

The following experiments demonstrate how basic Boolean operations are represented after synthesis.

---

## **AND Logic**

The first experiment implements a two-input AND operation.

An AND gate produces a logic HIGH at the output only when both inputs are HIGH.

### **Boolean Expression**

```text
Y = A · B
```

### **Truth Table**

| A | B | Y |
| - | - | - |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

### **Synthesized Result**
<img width="1233" height="370" alt="image" src="https://github.com/user-attachments/assets/3eb7b6b2-28cf-41b6-b6dc-37948f258a0b" />


The RTL description is synthesized using Yosys and mapped to the appropriate logic structure available in the target technology library.

The synthesized representation demonstrates how a simple Boolean expression is converted into a corresponding hardware implementation.

---

## **OR Logic**

The second experiment implements a two-input OR operation.

An OR gate produces a logic HIGH whenever at least one of its inputs is HIGH.

### **Boolean Expression**

```text
Y = A + B
```

### **Truth Table**

| A | B | Y |
| - | - | - |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

### **Synthesized Result**
<img width="1257" height="428" alt="image" src="https://github.com/user-attachments/assets/4789a49c-119c-4c48-91a7-2060130a17f3" />

During synthesis, Yosys analyzes the Boolean function and generates the corresponding hardware representation.

This experiment demonstrates the relationship between an RTL Boolean expression and its synthesized hardware structure.

---

## **Three-Input AND Logic**

The third experiment extends the AND operation to three inputs.

### **Boolean Expression**

```text
Y = A · B · C
```

The output becomes HIGH only when all three inputs are HIGH.

### **Truth Table**

| A | B | C | Y |
| - | - | - | - |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 |

### **Synthesized Result**
<img width="812" height="296" alt="image" src="https://github.com/user-attachments/assets/6f4a41db-e799-4f01-bf37-5e827c55e716" />

The synthesis tool identifies the required three-input logic function and generates an appropriate hardware implementation.

This experiment demonstrates how increasing the number of inputs affects the synthesized combinational structure.

---

# **3. Constant Propagation**

Constant propagation is an optimization technique in which known constant values are propagated through the logic of a design.

When a signal is permanently assigned a known value such as `0` or `1`, the synthesis tool can use this information to simplify the surrounding logic.

For example:

```text
A · 0 = 0
A · 1 = A
A + 0 = A
A + 1 = 1
```

If one input of an AND operation is permanently `0`, the output is always `0`, so the synthesis tool can eliminate unnecessary logic.

Similarly, if one input of an OR operation is permanently `1`, the output will always remain `1`.

Constant propagation can also be applied to sequential circuits when the synthesis tool can determine that a stored value remains fixed.

This optimization reduces unnecessary hardware and can improve the overall efficiency of the synthesized design.

---

# **4. D Flip-Flop Optimization**

The next experiments focus on optimization of sequential logic using D flip-flops.

A D flip-flop is a basic storage element used in synchronous digital systems. The data present at the D input is transferred to the output on the active clock edge.

For a positive-edge-triggered D flip-flop:

```text
Q(next) = D
```

When the D input is permanently connected to a known constant, the synthesis tool can determine the resulting behavior of the storage element.

### **Constant D Input**

For:

```text
D = 0
```

the stored value becomes `0` after the appropriate clock event.

Similarly, for:

```text
D = 1
```

the stored value becomes `1`.

The synthesis tool can use this constant information to simplify the sequential logic and remove unnecessary hardware wherever possible.

---

## **DFF Constant 1**

This experiment investigates the behavior and synthesis of a D flip-flop whose input is driven by a constant value.

### **Synthesized Circuit**
<img width="1264" height="354" alt="image" src="https://github.com/user-attachments/assets/24738dc5-f99a-4c35-b7ae-8442cc94ec2f" />

The synthesized circuit shows how the constant input is represented in the resulting sequential hardware structure.

### **Simulation Waveform**
<img width="1269" height="617" alt="image" src="https://github.com/user-attachments/assets/b6112fa1-b79c-4f11-9bf6-b8651f382bc1" />

The simulation waveform verifies the output behavior with respect to the clock.

This experiment demonstrates how constant information can influence the implementation of a sequential element.

---

## **DFF Constant 2**

The second experiment further examines constant propagation through a D flip-flop.

When the synthesis tool identifies that a signal remains constant, it can propagate this information through the connected logic and simplify the resulting circuit.

### **Synthesized Circuit**
<img width="764" height="610" alt="image" src="https://github.com/user-attachments/assets/229a9993-3b0c-447a-b57b-4879d6239ce6" />

The synthesized representation demonstrates the optimized sequential structure.

### **Simulation Waveform**
<img width="1275" height="629" alt="image" src="https://github.com/user-attachments/assets/0987cc0c-9fea-4d8a-9a8b-43eca14abbef" />

The waveform is used to verify the relationship between the clock and output signals.

The simulation confirms that the optimized design maintains the expected functionality.

---

## **DFF Constant 3**

The third experiment continues the study of constant propagation and sequential optimization.

The synthesized result provides a clearer view of how constant information can affect the final sequential hardware structure.

### **Synthesized Circuit**
<img width="1254" height="273" alt="image" src="https://github.com/user-attachments/assets/cdc61e9d-5ccf-4d7b-ad2d-033fbedb021e" />

The synthesized circuit represents the optimized implementation generated from the RTL description.

### **Simulation Waveform**
<img width="1264" height="618" alt="image" src="https://github.com/user-attachments/assets/1a61e6a7-358d-4a43-a2d3-15333d2e5722" />

The waveform is analyzed to confirm that the optimized circuit produces the expected output behavior.

This experiment highlights the importance of verifying both the synthesized structure and its functional behavior.

---

# **5. Counter Optimization**

A counter is a sequential circuit that moves through a predefined sequence of states.

A binary counter generally consists of multiple flip-flops along with combinational logic that determines the next state.

For an **N-bit binary counter**, the number of possible states is:

```text
2^N
```

For example, a 3-bit counter follows the sequence:

```text
000 → 001 → 010 → 011
    → 100 → 101 → 110 → 111
    → 000
```

Counters are useful for studying sequential optimization because they combine storage elements with next-state combinational logic.

During synthesis, the tool analyzes the RTL description and determines an efficient hardware implementation.

### **Original Counter**
<img width="1259" height="310" alt="image" src="https://github.com/user-attachments/assets/5887f08d-3b9e-41da-82f7-105fe77d51eb" />

The original counter is synthesized to observe the hardware structure generated from the RTL description.

### **Modified Counter**
<img width="1269" height="407" alt="image" src="https://github.com/user-attachments/assets/0349dfdc-d1d5-469a-8fb7-8e96a4dadbdc" />

A modified counter design is synthesized after making changes to the original RTL.

Comparing the two synthesized structures demonstrates that even small changes in RTL can influence the resulting hardware implementation.

---

# **6. Importance of Optimization**

RTL optimization is important because the synthesized hardware directly influences the physical characteristics of a digital design.

## **Area**

Removing redundant or unnecessary logic reduces the number of standard cells required.

A smaller hardware implementation can reduce the silicon area occupied by the design.

## **Power**

Logic transitions contribute to dynamic power consumption.

Reducing unnecessary logic and switching activity can therefore improve power efficiency.

## **Timing**

The type and number of logic cells present along a signal path influence propagation delay.

Optimization can help reduce unnecessary logic levels and improve critical-path performance.

## **Hardware Efficiency**

Optimization enables the required functionality to be implemented using suitable and efficient hardware resources.

Therefore, effective RTL optimization is essential for developing practical and efficient VLSI designs.

---

# **Key Observations**

The Day 3 experiments demonstrate the following key points:

1. RTL Boolean expressions can be converted into corresponding hardware structures during synthesis.
2. Different Boolean functions result in different synthesized implementations.
3. Boolean identities can simplify combinational logic.
4. Constant values can be propagated through a design to eliminate unnecessary logic.
5. Sequential circuits can also benefit from constant propagation and optimization.
6. The synthesized circuit may have a different structure from the original RTL while maintaining the same functionality.
7. Sequential optimization requires consideration of clocking and stored state.
8. Counters contain both storage elements and combinational next-state logic.
9. Simulation waveforms are useful for verifying the functionality of optimized sequential designs.
10. RTL optimization can affect area, power, timing, and overall hardware efficiency.

---

# **Conclusion**

Day 3 provided practical experience in **RTL optimization and synthesis** using combinational and sequential digital circuits.

The logic optimization experiments demonstrated how basic Boolean operations are converted into hardware structures during synthesis. The constant propagation experiments showed how known values can be used to simplify logic and eliminate unnecessary hardware.

The D flip-flop experiments extended these concepts to sequential circuits and demonstrated how constant information can influence synthesized storage elements. The counter experiment further illustrated optimization in a larger sequential design containing flip-flops and next-state logic.

Overall, the experiments demonstrated that synthesis is more than a direct conversion of RTL into gates. The synthesis tool analyzes the RTL, applies optimization techniques, and generates an efficient hardware representation while preserving the intended functionality of the design.

