# **RTL DESIGN WORKSHOP**

> **A practical journey through RTL design, simulation, synthesis, timing libraries, and digital hardware implementation.**

This repository showcases my hands-on learning journey in **RTL Design, Verilog Simulation, Waveform Analysis, Logic Synthesis, Technology Libraries, Timing Concepts, and Sequential Circuit Design**.

Each workshop day includes the concepts explored, practical experiments, commands executed, results obtained, screenshots, and important observations.

---

## **WORKSHOP PROGRESS**

| **Day**   | **Topics Covered**                                            | **Status**    |
| --------- | ------------------------------------------------------------- | ------------- |
| **Day 1** | Verilog RTL Design, Icarus Verilog, GTKWave & Yosys Synthesis | **Completed** |
| **Day 2** | Timing Libraries, Synthesis Techniques & Flip-Flop RTL Coding | **Completed** |

---

## **REPOSITORY STRUCTURE**

```text
RTL_Design_Workshop/
│
├── README.md
│
├── Day_1/
│   └── README.md
│
└── Day_2/
    └── README.md
```

---

# **DAY 1 — RTL DESIGN, SIMULATION & SYNTHESIS**

Day 1 focused on understanding the basics of the **RTL-to-Gate-Level design flow**.

The session began with creating Verilog RTL and a testbench, followed by functional simulation, waveform inspection, and logic synthesis using open-source EDA tools.

### **TOPICS EXPLORED**

* Design, Simulator and Testbench concepts
* Verilog RTL coding
* Icarus Verilog simulation
* 2:1 Multiplexer design
* GTKWave waveform inspection
* RTL design methodology
* Fundamentals of logic synthesis
* Introduction to Yosys
* Understanding `.lib` technology files
* Standard-cell fundamentals
* Fast and slow standard-cell variants
* Selecting cells according to design requirements
* Yosys synthesis workflow
* Synthesis statistics and reports
* Gate-level circuit representation
* Generation of gate-level netlists

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
* Simulation commands and procedures
* GTKWave waveform analysis
* Yosys synthesis commands
* Synthesis reports and results
* Experimental screenshots
* Generated gate-level netlist
* Key observations and conclusions

---

## **TOOLS USED — DAY 1**

| **Tool**           | **Purpose**                                 |
| ------------------ | ------------------------------------------- |
| **Verilog**        | Hardware description and RTL implementation |
| **Icarus Verilog** | RTL and functional simulation               |
| **GTKWave**        | Waveform visualization and analysis         |
| **Yosys**          | RTL logic synthesis                         |
| **Linux / Ubuntu** | Digital design environment                  |
| **Git & GitHub**   | Version control and project documentation   |

---

# **DAY 2 — TIMING LIBRARIES, SYNTHESIS & FLIP-FLOP RTL**

Day 2 explored the next stages of the digital implementation flow, with emphasis on **technology libraries, timing information, synthesis techniques, and sequential RTL design**.

The session also examined different flip-flop coding styles and how RTL descriptions are mapped to technology-specific standard cells.

### **TOPICS EXPLORED**

* SKY130 technology library
* `.lib` timing library structure
* Standard-cell timing characteristics
* Process, Voltage and Temperature (PVT) conditions
* Hierarchical synthesis
* Flattened synthesis
* Comparison of hierarchical and flattened designs
* Asynchronous-reset D flip-flop
* Asynchronous-set D flip-flop
* Synchronous-reset D flip-flop
* Icarus Verilog functional simulation
* GTKWave waveform analysis
* Yosys synthesis flow
* `dfflibmap` for flip-flop technology mapping
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
        │  Technology /     │
        │  Timing Library   │
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
        │ Technology Mapping│
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
* Different synthesis approaches
* Flip-flop RTL implementations
* Functional simulation waveforms
* Yosys synthesis procedures
* Flip-flop library mapping
* Technology mapping using ABC
* Gate-level synthesis results
* Screenshots and experimental observations

---

## **TOOLS & TECHNOLOGIES — DAY 2**

| **Tool / Technology** | **Purpose**                               |
| --------------------- | ----------------------------------------- |
| **Verilog**           | RTL hardware design                       |
| **Icarus Verilog**    | Functional and RTL simulation             |
| **GTKWave**           | Waveform visualization                    |
| **Yosys**             | RTL synthesis and optimization            |
| **SKY130**            | Standard-cell technology library          |
| **Linux / Ubuntu**    | Design and simulation environment         |
| **Git & GitHub**      | Version control and project documentation |

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
             TECHNOLOGY MAPPING
                     │
                     ▼
             GATE-LEVEL NETLIST
                     │
                     ▼
              DIGITAL HARDWARE
```

> **This workshop provides a practical foundation for understanding the RTL-to-GDS implementation flow and developing hands-on skills in digital VLSI design.**
