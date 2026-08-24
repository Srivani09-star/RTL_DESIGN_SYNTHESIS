# **Day 05 — Conditional Statements, Case Constructs & Looping in RTL**

## **Overview**

Day 5 focuses on different Verilog RTL coding techniques used to represent **conditional decisions, selection logic, and repeated hardware structures**.

The experiments demonstrate how `if-else` and `case` statements are interpreted during synthesis, how incomplete assignments can result in unintended latch inference, and how overlapping `casez` patterns can create ambiguous selection conditions.

The session also introduces procedural `for` loops and generate `for` loops. These constructs are applied to practical designs such as MUX, DEMUX, and Ripple Carry Adder implementations.

Simulation waveforms and synthesized netlists are examined throughout the experiments to understand the relationship between RTL code and the resulting hardware.

---


## **Table of Contents**
- [1. RTL Conditional Coding Styles](#1-rtl-conditional-coding-styles)
- [2. Latch Inference](#2-latch-inference)
- [3. Labs 1-2: Incomplete IF Statements](#3-labs-1-2-incomplete-if-statements)
- [4. Labs 3-5: CASE Statement Experiments](#4-labs-3-5-case-statement-experiments)
- [5. Lab 6: Overlapping CASEZ Conditions](#5-lab-6-overlapping-casez-conditions)
- [6. Logic Redundancy Optimization](#6-logic-redundancy-optimization)
- [7. Looping Constructs in Verilog](#7-looping-constructs-in-verilog)
- [8. Labs 7-10: MUX, DEMUX and RCA](#8-labs-7-10-mux-demux-and-rca)
- [9. Overall Summary](#9-overall-summary)
- [10. Conclusion](#10-conclusion)

# **1. RTL Conditional Coding Styles**

RTL describes the intended behaviour of a digital circuit before the design is transformed into gates through synthesis.

The coding style used in RTL has a direct influence on how synthesis tools interpret the required hardware.

Conditional statements are commonly used for:

* Data selection
* Control operations
* Decision-making logic
* Priority logic
* Multiplexer implementation
* Decoder and FSM design

---

## **Priority Logic Using IF-ELSE**

An `if-else` structure evaluates conditions in sequence.

When more than one condition is true, the first matching condition is selected. Therefore, the order of the conditions establishes the priority.

Example:

```verilog
always @(*) begin
    if (condition_1)
        y = value_1;
    else if (condition_2)
        y = value_2;
    else
        y = value_3;
end
```

The priority order is:

| **Condition** | **Priority** |
| ------------- | ------------ |
| `if`          | Highest      |
| `else if`     | Intermediate |
| `else`        | Lowest       |

This makes `if-else` particularly suitable for circuits where one condition must take precedence over another, such as priority encoders and control circuits.

---

## **Selection Logic Using CASE**

A `case` statement compares a selector against multiple possible values.

Example:

```verilog
always @(*) begin
    case (sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        default: y = d;
    endcase
end
```

This coding style is useful when each selector value corresponds to a particular input or operation.

Typical applications include:

* Multiplexers
* Decoders
* Finite State Machines
* Control logic

### **IF-ELSE vs CASE**

| **Aspect**          | **IF-ELSE**               | **CASE**                     |
| ------------------- | ------------------------- | ---------------------------- |
| Main use            | Priority decisions        | Multiple selections          |
| Evaluation          | Sequential conditions     | Selector comparison          |
| Priority            | Naturally established     | Depends on case structure    |
| Common applications | Priority logic, control   | MUX, decoder, FSM            |
| Main concern        | Ordering and completeness | Coverage and pattern overlap |

---

# **2. Latch Inference**

A latch is a level-sensitive storage element capable of retaining its previous value.

In combinational RTL, an unintended latch can be inferred when an output is not assigned under every possible condition.

Consider:

```verilog
always @(*) begin
    if (enable)
        y = data;
end
```

When `enable` is `1`, `y` receives `data`.

When `enable` becomes `0`, there is no assignment to `y`. The circuit therefore needs to preserve the previous value, resulting in latch inference during synthesis.

A complete implementation can be written as:

```verilog
always @(*) begin
    if (enable)
        y = data;
    else
        y = 1'b0;
end
```

Another option is to provide a default assignment before evaluating the condition:

```verilog
always @(*) begin
    y = 1'b0;

    if (enable)
        y = data;
end
```

Both approaches ensure that the output receives a defined value for every possible execution path.

---

## **Latch Inference vs Sequential Storage**

Unintended latch inference in combinational logic should not be confused with intentional storage in sequential circuits.

For example:

```verilog
always @(posedge clk or posedge reset) begin
    if (reset)
        count <= 0;
    else if (enable)
        count <= count + 1;
end
```

Here, `count` is implemented using flip-flops because the process is triggered by a clock edge.

When `enable` is low, the flip-flop retains its previous value. This is expected sequential behaviour rather than unintended latch inference.

---

# **3. Labs 1-2: Incomplete IF Statements**

## **Lab 1 — Incomplete IF Statement**

**File:** `incomp_if.v`

The first experiment demonstrates how an incomplete `if` statement can cause latch inference.

```verilog
always @(*) begin
    if (i0)
        y = i1;
end
```

Only one condition is covered.

| **`i0`** | **Behaviour of `y`** |
| -------- | -------------------- |
| `1`      | `y = i1`             |
| `0`      | No new assignment    |

When `i0 = 0`, the output is not updated. The synthesized circuit must therefore preserve the previous value of `y`.

### **Waveform**

The waveform demonstrates the output behaviour caused by the incomplete assignment.

### **Synthesized Netlist**

The synthesized structure illustrates the storage inferred from the incomplete combinational description.

### **Learning Outcome**

A combinational output should be assigned for all required input conditions. Missing assignments can unintentionally introduce latches.

---

## **Lab 2 — Incomplete IF-ELSE Statement**

**File:** `incomp_if2.v`

The second experiment extends the previous example by adding another condition.

```verilog
always @(*) begin
    if (i0)
        y = i1;
    else if (i2)
        y = i3;
end
```

The behaviour can be summarized as:

| **`i0`** | **`i2`** | **Output**    |
| -------- | -------- | ------------- |
| `1`      | X        | `y = i1`      |
| `0`      | `1`      | `y = i3`      |
| `0`      | `0`      | No assignment |

The final condition remains uncovered, so the output can retain its previous value and latch inference may occur.

### **Complete Version**

```verilog
always @(*) begin
    if (i0)
        y = i1;
    else if (i2)
        y = i3;
    else
        y = 1'b0;
end
```

The final `else` ensures that the remaining condition also produces a defined output.

### **Learning Outcome**

Adding an `else if` does not automatically make a combinational block complete. Every possible execution path must assign a valid output.

---

# **4. Labs 3–5: CASE Statement Experiments**

## **Lab 3 — Incomplete CASE Statement**

**File:** `incomp_case.v`

This experiment demonstrates incomplete selector coverage.

```verilog
always @(*) begin
    case (sel)
        2'b00: y = i0;
        2'b01: y = i1;
    endcase
end
```

A 2-bit selector has four possible combinations:

| **`sel`** | **Output**    |
| --------- | ------------- |
| `2'b00`   | `y = i0`      |
| `2'b01`   | `y = i1`      |
| `2'b10`   | No assignment |
| `2'b11`   | No assignment |

Since two selector combinations are not covered, the synthesis tool may infer storage.

### **Waveform**

The waveform shows the output behaviour for the covered and uncovered selector conditions.

### **Synthesized Netlist**

The synthesized result demonstrates the hardware consequences of incomplete case coverage.

### **Learning Outcome**

When using `case` for combinational logic, all required selector values should be covered or a suitable `default` condition should be provided.

---
# **4. Labs 3-5: CASE Statement Experiments**

**File:** `comp_case.v`

The incomplete case statement can be completed by introducing a `default` branch.

```verilog
always @(*) begin
    case (sel)
        2'b00: y = i0;
        2'b01: y = i1;
        default: y = i2;
    endcase
end
```

The resulting behaviour is:

| **`sel`** | **Output** |
| --------- | ---------- |
| `2'b00`   | `y = i0`   |
| `2'b01`   | `y = i1`   |
| `2'b10`   | `y = i2`   |
| `2'b11`   | `y = i2`   |

The `default` branch ensures that previously uncovered selector values receive a defined output.

### **Waveform**

The waveform verifies the output response for each selector combination.

### **Synthesized Netlist**

The synthesized circuit demonstrates the complete combinational implementation.

### **Learning Outcome**

A `default` branch is useful for providing defined behaviour when selector values are not explicitly listed.

---

## **Lab 5 — Partial Output Assignment**

**File:** `partial_case_assign.v`

This experiment demonstrates that different outputs within the same `case` statement can have different assignment coverage.

```verilog
always @(*) begin
    case (sel)

        2'b00: begin
            y = i0;
            x = i2;
        end

        2'b01: begin
            y = i1;
        end

        default: begin
            y = i3;
            x = i4;
        end

    endcase
end
```

The output `y` is assigned in every branch, but `x` is not assigned when `sel = 2'b01`.

| **`sel`** | **`y`** | **`x`**        |
| --------- | ------- | -------------- |
| `2'b00`   | `i0`    | `i2`           |
| `2'b01`   | `i1`    | Previous value |
| Default   | `i3`    | `i4`           |

As a result, storage may be inferred for `x`.

### **Synthesized Netlist**

The synthesized structure highlights the effect of incomplete assignment for one output.

### **Learning Outcome**

Every combinational output must be appropriately assigned across all execution paths.

---

# **5. Lab 6: Overlapping CASEZ Conditions**

**File:** `bad_case.v`

This experiment examines overlapping wildcard patterns using `casez`.

```verilog
always @(*) begin
    casez (sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3;
    endcase
end
```

The `?` symbol represents a don't-care position.

Therefore:

```text
2'b1?
```

can match both:

```text
2'b10
2'b11
```

Consequently, the selector `2'b10` matches more than one case item.

| **`sel`** | **Matching Pattern(s)** |
| --------- | ----------------------- |
| `2'b00`   | `2'b00`                 |
| `2'b01`   | `2'b01`                 |
| `2'b10`   | `2'b10` and `2'b1?`     |
| `2'b11`   | `2'b1?`                 |

This is an **overlap condition**, rather than a latch condition.

### **Waveform**

The simulation waveform demonstrates the behaviour produced by the overlapping patterns.

### **Learning Outcome**

Wildcard patterns should be designed carefully because overlapping conditions can create multiple possible matches and lead to unintended selection behaviour.

---

# **6. Logic Redundancy Optimization**

Synthesis tools analyze the logic extracted from RTL and optimize it before mapping the design to the target technology.

Redundant Boolean expressions can often be simplified without changing the intended functionality.

For example:

```text
F = A + A'B
```

can be simplified to:

```text
F = A + B
```

The simplified expression contains less redundant logic.

Synthesis optimization can contribute to:

* Reduced gate count
* Lower area
* Lower logic complexity
* Improved timing
* Reduced switching activity
* Potential power reduction

The general optimization flow is:

```text
RTL Description
      ↓
Logic Analysis
      ↓
Boolean Simplification
      ↓
Technology Mapping
      ↓
Gate-Level Netlist
```

Therefore, the final synthesized structure may look very different from the original RTL while still implementing the same functionality.

---

# **7. Looping Constructs in Verilog**

Loops provide an efficient way to describe repeated operations without manually writing similar RTL statements multiple times.

Two important loop constructs used in RTL design are:

* **Procedural `for` loop**
* **Generate `for` loop**

Although their syntax is similar, they are used for different purposes.

---

## **Procedural `for` Loop**

A procedural `for` loop is written inside an `always` block.

Example:

```verilog
integer i;

always @(*) begin
    for (i = 0; i < 4; i = i + 1) begin
        y[i] = a[i];
    end
end
```

Procedural loops are useful for describing:

* MUX logic
* DEMUX logic
* Bit-level operations
* Array processing
* Repeated combinational operations

---

## **Generate `for` Loop**

A generate loop is used outside procedural blocks to replicate structural hardware during elaboration.

Example:

```verilog
genvar i;

generate
    for (i = 0; i < WIDTH; i = i + 1) begin
        // Repeated hardware instance
    end
endgenerate
```

Generate loops are useful for:

* Ripple Carry Adders
* Full Adder arrays
* Register arrays
* Repeated module instances
* Parameterized hardware

### **Procedural `for` vs Generate `for`**

| **Feature**       | **Procedural `for`**       | **Generate `for`**             |
| ----------------- | -------------------------- | ------------------------------ |
| Location          | Inside `always` block      | Outside procedural blocks      |
| Purpose           | Repeats RTL operations     | Replicates hardware structures |
| Typical use       | MUX, DEMUX, bit operations | RCA, module arrays             |
| Processing stage  | Procedural evaluation      | Elaboration                    |
| Description style | Behavioral                 | Structural                     |

---

# **8. Labs 7–10: MUX, DEMUX & RCA**

## **Lab 7 — MUX Using Procedural `for` Loop**

**File:** `mux_generate.v`

A multiplexer selects one input from multiple available inputs according to a select signal.

Using a loop avoids repeatedly writing individual selection conditions and makes the RTL easier to scale.

The functional concept is:

```text
Multiple Inputs
      ↓
Select Signal
      ↓
     MUX
      ↓
Single Output
```

### **Waveform**

The waveform verifies that the output corresponds to the input selected by the control signal.

### **Observation**

The procedural loop provides a compact way of describing repetitive MUX selection logic.

### **Learning Outcome**

A procedural `for` loop can reduce repetitive RTL statements while still allowing the synthesis tool to generate the required combinational hardware.

---

# **8. Labs 7-10: MUX, DEMUX and RCA**
**File:** `demux_case.v`

A demultiplexer takes a single input and directs it to one of several output lines based on the select signal.

For a four-output DEMUX:

```text
sel = 2'b00 → Output 0
sel = 2'b01 → Output 1
sel = 2'b10 → Output 2
sel = 2'b11 → Output 3
```

A `case` statement provides a direct way of representing these selection conditions.

The selected output receives the input while the remaining outputs remain inactive.

### **Waveform**

The waveform verifies that the input is routed to the correct output for each selector value.

### **Learning Outcome**

For relatively small DEMUX designs, a `case` statement provides a straightforward and readable RTL implementation.

---

## **Lab 9 — DEMUX Using Procedural `for` Loop**

**File:** `demux_generate.v`

The same DEMUX functionality can also be implemented using a procedural loop.

The basic sequence is:

```text
Initialize Outputs
      ↓
Read Select Signal
      ↓
Iterate Through Outputs
      ↓
Find Selected Index
      ↓
Activate Selected Output
```

This approach avoids writing a separate branch for every output line.

### **CASE vs Procedural LOOP**

| **Feature**   | **CASE**          | **Procedural `for`** |
| ------------- | ----------------- | -------------------- |
| Coding method | Explicit branches | Repeated operation   |
| Small designs | Simple            | Simple               |
| Scalability   | More manual       | More convenient      |
| Repetition    | Higher            | Lower                |

### **Waveform**

The waveform verifies that the correct output is activated based on the select signal.

### **Learning Outcome**

Procedural loops provide a compact and scalable method for implementing repetitive DEMUX logic.

---

## **Lab 10 — Ripple Carry Adder Using Generate `for` Loop**

**File:** `rca.v`

A Ripple Carry Adder (RCA) is constructed by connecting multiple Full Adder stages in sequence.

Each Full Adder receives:

* One bit from operand `A`
* One bit from operand `B`
* Carry from the previous stage

and generates:

* One sum bit
* Carry for the next stage

The carry propagates from the least significant stage toward the most significant stage.

### **RCA Structure**

```text
A0 ──┐
B0 ──┤
Cin ─┤
     ↓
 Full Adder
     │
     ├── Sum0
     │
     └── Carry1
             ↓
          Full Adder
             │
             ├── Sum1
             │
             └── Carry2
                    ↓
                   ...
```

A generate loop can be used to automatically instantiate a Full Adder for each bit:

```verilog
genvar i;

generate
    for (i = 0; i < WIDTH; i = i + 1) begin

        full_adder FA (
            .a(a[i]),
            .b(b[i]),
            .cin(carry[i]),
            .sum(sum[i]),
            .cout(carry[i+1])
        );

    end
endgenerate
```

The use of a generate loop makes the RCA scalable because the number of Full Adder stages can be controlled through the width parameter.

### **RTL Simulation Waveform**

The RTL waveform verifies the addition operation, sum output, and carry propagation.

### **Synthesized Netlist**

The synthesized netlist represents the hardware generated from the structural RTL description.

### **Gate-Level Verification**

The synthesized design can be verified through Gate-Level Simulation to confirm that the hardware implementation maintains the intended functionality.

### **Learning Outcome**

The RCA experiment demonstrates how generate loops can be used to construct repeated hardware structures. The same approach can be extended to larger arithmetic circuits, register arrays, and other parameterized designs.

---

# **9. Overall Summary**

The Day 5 experiments demonstrate how different Verilog RTL constructs influence the description, synthesis, and verification of digital hardware.

| **Topic**              | **Main Learning**                                      |
| ---------------------- | ------------------------------------------------------ |
| IF-ELSE                | Represents ordered priority conditions                 |
| CASE                   | Describes multi-way selection                          |
| Incomplete IF          | May result in unintended latch inference               |
| Incomplete CASE        | May result in unintended storage                       |
| Partial Assignment     | Can create storage for an incompletely assigned output |
| Overlapping CASEZ      | Can result in multiple matching conditions             |
| Synthesis Optimization | Removes redundant logic while preserving functionality |
| Procedural `for`       | Describes repeated RTL operations                      |
| Generate `for`         | Replicates structural hardware                         |
| MUX                    | Selects one input using a control signal               |
| DEMUX                  | Routes one input to a selected output                  |
| RCA                    | Performs binary addition using cascaded Full Adders    |

---

# **10. Conclusion**

Day 5 extends the concepts of RTL design and synthesis by introducing conditional statements and looping constructs used to describe practical digital hardware.

The incomplete `if-else` and `case` experiments demonstrated how missing assignments can result in unintended latch inference. The complete implementations showed how proper output coverage can be used to describe predictable combinational logic.

The `casez` experiment highlighted the importance of carefully designing wildcard conditions because overlapping patterns can produce multiple matches.

The synthesis optimization experiment demonstrated that redundant logic can be simplified by the synthesis tool while maintaining the required functionality.

The final experiments introduced procedural and generate loops. Procedural loops provide an efficient way to describe repetitive operations such as MUX and DEMUX logic, while generate loops allow repeated structural components such as Full Adders to be instantiated efficiently.

Overall, Day 5 provided practical experience in developing **complete, scalable, and synthesis-friendly RTL**, while reinforcing the importance of simulation and synthesized-netlist analysis for verifying digital hardware designs.

