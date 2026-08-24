
# **RTL DESIGN WORKSHOP**

> **A practical journey through RTL design, simulation, synthesis, timing libraries, optimization, and digital hardware implementation.**

This repository documents my hands-on learning journey in **RTL Design, Verilog Simulation, Waveform Analysis, Logic Synthesis, Technology Libraries, Timing Concepts, RTL Optimization, Gate-Level Simulation, Conditional RTL Coding, and Sequential Circuit Design**.

Each workshop day contains the concepts studied, practical experiments performed, simulation and synthesis procedures, results, observations, screenshots, and key learning outcomes.

---

## **WORKSHOP PROGRESS**

| **Day**   | **Topics Covered**                                                                | **Status**    |
| --------- | --------------------------------------------------------------------------------- | ------------- |
| **Day 1** | Verilog RTL Design, Icarus Verilog, GTKWave & Yosys Synthesis                     | **Completed** |
| **Day 2** | Timing Libraries, Synthesis Techniques & Flip-Flop RTL Coding                     | **Completed** |
| **Day 3** | RTL Optimization, Logic Optimization, Constant Propagation & Counter Optimization | **Completed** |
| **Day 4** | RTL-to-Gate-Level Flow, MUX, Sensitivity Lists, Blocking Assignments & GLS        | **Completed** |
| **Day 5** | IF-ELSE, CASE, Latch Inference, CASEZ, Loops, MUX, DEMUX & RCA                    | **Completed** |

---

## **REPOSITORY STRUCTURE**

```text
RTL_DESIGN_SYNTHESIS/
│
├── README.md
│
├── Day_1/
│   └── README.md
│
├── Day_2/
│   └── README.md
│
├── Day_3/
│   └── README.md
│
├── Day_4/
│   └── README.md
│
└── Day_5/
    └── README.md
```

---

# **DAY 1 — RTL DESIGN, SIMULATION & SYNTHESIS**

Day 1 introduced the fundamentals of the **RTL-to-Gate-Level design flow**.

The session covered Verilog RTL development, testbench creation, functional simulation, waveform analysis, and logic synthesis using open-source EDA tools.

### **TOPICS EXPLORED**

* Design, simulator and testbench concepts
* Verilog RTL coding
* Icarus Verilog simulation
* 2:1 Multiplexer design
* GTKWave waveform analysis
* RTL design methodology
* Fundamentals of logic synthesis
* Introduction to Yosys
* `.lib` technology files
* Standard-cell fundamentals
* Fast and slow standard-cell variants
* Cell selection based on design requirements
* Yosys synthesis workflow
* Synthesis statistics and reports
* Gate-level circuit representation
* Gate-level netlist generation

### **DAY 1 DESIGN FLOW**

```text
        ┌───────────────────┐
        │    Verilog RTL    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     Testbench     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Icarus Verilog   │
        │    Simulation     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │      GTKWave      │
        │ Waveform Analysis │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │   Gate-Level      │
        │     Netlist       │
        └───────────────────┘
```

### **DAY 1 DOCUMENTATION**

The Day 1 documentation includes:

* Verilog RTL source code
* Testbench implementation
* Simulation commands
* GTKWave waveform analysis
* Yosys synthesis commands
* Synthesis reports
* Experimental screenshots
* Gate-level netlist
* Key observations
* Conclusions

### **TOOLS USED — DAY 1**

| **Tool**           | **Purpose**                                 |
| ------------------ | ------------------------------------------- |
| **Verilog**        | Hardware description and RTL implementation |
| **Icarus Verilog** | RTL and functional simulation               |
| **GTKWave**        | Waveform visualization and analysis         |
| **Yosys**          | RTL logic synthesis                         |
| **Linux / Ubuntu** | Digital design environment                  |
| **Git & GitHub**   | Version control and documentation           |

---

# **DAY 2 — TIMING LIBRARIES, SYNTHESIS & FLIP-FLOP RTL**

Day 2 moved deeper into the digital implementation flow by focusing on **technology libraries, timing information, synthesis approaches, and sequential RTL design**.

Different flip-flop coding styles were also implemented and studied to understand how RTL descriptions are mapped to technology-specific cells.

### **TOPICS EXPLORED**

* SKY130 technology library
* `.lib` timing library structure
* Standard-cell timing characteristics
* Process, Voltage and Temperature (PVT) conditions
* Hierarchical synthesis
* Flattened synthesis
* Hierarchical versus flattened netlists
* Asynchronous-reset D flip-flop
* Asynchronous-set D flip-flop
* Synchronous-reset D flip-flop
* Icarus Verilog simulation
* GTKWave waveform analysis
* Yosys synthesis
* `dfflibmap` flip-flop technology mapping
* `abc` technology mapping
* Technology-specific gate-level representation

### **DAY 2 DESIGN FLOW**

```text
        ┌───────────────────┐
        │     RTL Design    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Technology /      │
        │ Timing Library    │
        │      (.lib)       │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     Synthesis     │
        │                   │
        │ Hierarchical /    │
        │    Flattened      │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ dfflibmap + ABC   │
        │ Technology Mapping │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Technology-Mapped │
        │     Netlist       │
        └───────────────────┘
```

### **DAY 2 DOCUMENTATION**

The Day 2 documentation covers:

* Timing library exploration
* PVT condition analysis
* Hierarchical and flattened synthesis
* Flip-flop RTL implementations
* Functional simulation
* Waveform analysis
* Yosys synthesis procedures
* Flip-flop library mapping
* Technology mapping using ABC
* Gate-level synthesis results
* Experimental observations

### **TOOLS & TECHNOLOGIES — DAY 2**

| **Tool / Technology** | **Purpose**                       |
| --------------------- | --------------------------------- |
| **Verilog**           | RTL hardware design               |
| **Icarus Verilog**    | Functional and RTL simulation     |
| **GTKWave**           | Waveform visualization            |
| **Yosys**             | RTL synthesis and optimization    |
| **SKY130**            | Standard-cell technology library  |
| **Linux / Ubuntu**    | Design and simulation environment |
| **Git & GitHub**      | Version control and documentation |

---

# **DAY 3 — RTL OPTIMIZATION & SYNTHESIS**

Day 3 focused on understanding how synthesis tools **analyze RTL, simplify logic, remove unnecessary hardware, and generate optimized implementations**.

The experiments covered both combinational and sequential optimization techniques.

### **TOPICS EXPLORED**

* RTL optimization
* Boolean logic simplification
* AND logic optimization
* OR logic optimization
* Three-input AND logic
* Constant propagation
* Constant-value optimization
* D flip-flop optimization
* Sequential logic optimization
* Counter optimization
* Redundant logic removal
* Hardware efficiency
* Area and timing considerations

### **DAY 3 OPTIMIZATION FLOW**

```text
        ┌───────────────────┐
        │     RTL Design    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Logic Analysis   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     RTL Logic     │
        │    Optimization   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │      Yosys        │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Optimized Hardware │
        │   Representation  │
        └───────────────────┘
```

### **EXPERIMENTS**

The Day 3 experiments included:

* Basic AND logic
* Basic OR logic
* Three-input AND logic
* Constant propagation
* DFF constant optimization
* Sequential logic optimization
* Counter optimization

The experiments demonstrated how synthesis tools can simplify RTL while preserving the intended functionality.

### **KEY LEARNINGS**

* Boolean expressions can be simplified during synthesis.
* Constant values can eliminate unnecessary logic.
* Sequential elements can also be optimized.
* Counter structures contain both storage and next-state logic.
* Optimization can influence area, power, timing, and hardware complexity.
* Synthesized hardware may have a different structure from the original RTL while maintaining the same functionality.

---

# **DAY 4 — RTL DESIGN, SYNTHESIS & GATE-LEVEL SIMULATION**

Day 4 focused on the complete transition from **RTL code to synthesized hardware and Gate-Level Simulation (GLS)**.

The experiments also examined RTL coding practices that can lead to simulation-synthesis mismatches.

### **TOPICS EXPLORED**

* RTL-to-Gate-Level design flow
* Ternary-operator MUX
* RTL simulation
* Yosys synthesis
* Technology mapping
* Standard-cell netlist generation
* Gate-Level Simulation
* Incomplete sensitivity lists
* `always @(*)`
* Blocking assignments
* Non-blocking assignments
* Simulation-synthesis mismatch
* RTL and GLS waveform comparison

### **DAY 4 DESIGN FLOW**

```text
        ┌───────────────────┐
        │    Verilog RTL    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │   RTL Simulation  │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Technology Mapping │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Gate-Level Netlist│
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Gate-Level Sim.   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │    GTKWave GLS    │
        │ Waveform Analysis │
        └───────────────────┘
```

### **EXPERIMENTS**

The Day 4 experiments included:

* 2:1 MUX using the ternary operator
* RTL simulation of the MUX
* Synthesis and standard-cell mapping
* Gate-Level Simulation
* MUX with incomplete sensitivity list
* Correct use of `always @(*)`
* Blocking assignment experiment
* Blocking versus non-blocking assignments
* RTL and GLS waveform comparison

### **KEY LEARNINGS**

* The ternary operator provides a compact way to describe a MUX.
* All relevant signals should be included in combinational sensitivity.
* `always @(*)` helps avoid incomplete sensitivity-list problems.
* Blocking assignments execute sequentially within a procedural block.
* Non-blocking assignments are commonly used for sequential logic.
* RTL simulation and Gate-Level Simulation serve different verification purposes.
* Improper RTL coding can result in simulation-synthesis mismatches.

---

# **DAY 5 — CONDITIONAL STATEMENTS, CASE & LOOPING IN RTL**

Day 5 concentrated on **conditional RTL coding, latch inference, case statements, wildcard matching, synthesis optimization, and looping constructs**.

The concepts were applied to practical digital circuits such as MUX, DEMUX, and Ripple Carry Adder designs.

### **TOPICS EXPLORED**

* IF-ELSE conditional coding
* Priority logic
* CASE statements
* Complete and incomplete CASE
* Latch inference
* Incomplete output assignments
* `casez` wildcard matching
* Overlapping CASEZ conditions
* Logic redundancy optimization
* Procedural `for` loops
* Generate `for` loops
* MUX implementation
* DEMUX implementation
* Ripple Carry Adder
* Parameterized hardware structures

### **DAY 5 RTL CODING FLOW**

```text
        ┌───────────────────┐
        │   RTL Description │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ IF-ELSE / CASE    │
        │ Conditional Logic │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │   RTL Simulation  │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Logic Optimization│
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Hardware Netlist │
        └───────────────────┘
```

### **EXPERIMENTS**

The Day 5 experiments covered:

* Incomplete IF statement
* Incomplete IF-ELSE statement
* Incomplete CASE statement
* Complete CASE statement
* Partial output assignment
* Overlapping `casez` conditions
* Boolean logic optimization
* MUX using procedural `for`
* DEMUX using CASE
* DEMUX using procedural `for`
* Ripple Carry Adder using generate `for`

### **LATCH INFERENCE**

An incomplete combinational assignment can cause the synthesis tool to infer a latch.

For example:

```verilog
always @(*) begin
    if (enable)
        y = data;
end
```

When `enable` is low, `y` is not assigned.

A complete description should provide an output value for every required condition:

```verilog
always @(*) begin
    if (enable)
        y = data;
    else
        y = 1'b0;
end
```

### **CASE AND CASEZ**

The experiments demonstrated the importance of complete `case` coverage and careful use of wildcard patterns.

An overlapping `casez` pattern can match more than one selector condition, which may lead to unexpected selection behaviour.

### **LOOPING CONSTRUCTS**

Two types of loops were studied:

| **Construct**             | **Purpose**                                           |
| ------------------------- | ----------------------------------------------------- |
| **Procedural `for` loop** | Repeated RTL operations inside procedural blocks      |
| **Generate `for` loop**   | Replication of structural hardware during elaboration |

### **PRACTICAL DESIGNS**

The concepts were applied to:

* **MUX** — selection of one input from multiple inputs
* **DEMUX** — routing one input toward a selected output
* **Ripple Carry Adder** — cascading Full Adders to perform binary addition

### **KEY LEARNINGS**

* IF-ELSE naturally represents priority-based decisions.
* CASE statements are useful for multi-way selection logic.
* Incomplete assignments can infer unintended latches.
* `default` branches can provide complete combinational coverage.
* Wildcard CASE conditions must be designed carefully.
* Procedural loops simplify repetitive RTL operations.
* Generate loops are useful for repeated hardware structures.
* RTL coding style directly affects the synthesized hardware.

---

# **TOOLS & TECHNOLOGIES**

The workshop progressively uses the following tools and technologies:

| **Tool / Technology** | **Purpose**                               |
| --------------------- | ----------------------------------------- |
| **Verilog**           | RTL hardware description                  |
| **Icarus Verilog**    | RTL and functional simulation             |
| **GTKWave**           | Waveform visualization and analysis       |
| **Yosys**             | RTL synthesis and optimization            |
| **SKY130**            | Standard-cell technology library          |
| **ABC**               | Logic optimization and technology mapping |
| **dfflibmap**         | Flip-flop technology mapping              |
| **Linux / Ubuntu**    | Digital design environment                |
| **Git**               | Version control                           |
| **GitHub**            | Project hosting and documentation         |

---

# **RTL DESIGN JOURNEY**

```text
                    RTL DESIGN
                        │
                        ▼
                    SIMULATION
                        │
                        ▼
                 WAVEFORM ANALYSIS
                        │
                        ▼
                     SYNTHESIS
                        │
                        ▼
                 RTL OPTIMIZATION
                        │
                        ▼
                TECHNOLOGY MAPPING
                        │
                        ▼
                GATE-LEVEL NETLIST
                        │
                        ▼
              GATE-LEVEL SIMULATION
                        │
                        ▼
                  VERIFICATION
                        │
                        ▼
                DIGITAL HARDWARE
```

---

# **WORKSHOP LEARNING PROGRESSION**

```text
DAY 1
RTL Design
   ↓
Simulation
   ↓
Waveform Analysis
   ↓
Basic Synthesis
        │
        ▼
DAY 2
Timing Libraries
   ↓
PVT Information
   ↓
Sequential RTL
   ↓
Technology Mapping
        │
        ▼
DAY 3
RTL Optimization
   ↓
Constant Propagation
   ↓
Sequential Optimization
   ↓
Counter Optimization
        │
        ▼
DAY 4
RTL-to-Gate Flow
   ↓
Gate-Level Simulation
   ↓
Coding Practices
   ↓
Simulation-Synthesis Analysis
        │
        ▼
DAY 5
Conditional RTL
   ↓
Latch Analysis
   ↓
CASE / CASEZ
   ↓
Looping Constructs
   ↓
MUX / DEMUX / RCA
```

---

# **CONCLUSION**

This workshop provides a progressive hands-on understanding of the **RTL-to-Gate-Level digital design flow**.

The journey begins with basic Verilog RTL design and simulation, then moves into timing libraries, sequential circuit design, synthesis techniques, RTL optimization, Gate-Level Simulation, and advanced RTL coding constructs.

Across five modules, the experiments demonstrate how RTL descriptions are analyzed, simulated, optimized, synthesized, technology-mapped, and verified.

The workshop therefore provides a practical foundation for understanding **digital VLSI design, RTL development, synthesis, verification, and the early stages of the RTL-to-GDS implementation flow**.
