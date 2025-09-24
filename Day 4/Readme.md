# 🚀 Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

Welcome to **Day 4** of the RTL Workshop! 🎉 This session dives into three pivotal VLSI design concepts:

- 🔍 **Gate-Level Simulation (GLS)**: Validate your synthesized netlist.
- ⚠️ **Synthesis-Simulation Mismatch**: Avoid common pitfalls.
- 🔄 **Blocking vs. Non-Blocking Assignments**: Master Verilog coding practices.

Get ready for a blend of theory, practical insights, and hands-on labs to level up your digital design skills! 💻

---

## 📑 Table of Contents

| **Section** | **Description** |
|-------------|-----------------|
| [1. Gate-Level Simulation (GLS)](#1-gate-level-simulation-gls) | Learn about GLS for netlist validation. |
| [2. Synthesis-Simulation Mismatch](#2-synthesis-simulation-mismatch) | Understand causes and prevention of mismatches. |
| [3. Blocking vs. Non-Blocking Assignments](#3-blocking-vs-non-blocking-assignments) | Explore Verilog assignment types. |
| [3.1 Blocking Statements](#31-blocking-statements) | Details on blocking assignments. |
| [3.2 Non-Blocking Statements](#32-non-blocking-statements) | Details on non-blocking assignments. |
| [3.3 Comparison Table](#33-comparison-table) | Compare blocking vs. non-blocking. |
| [4. Labs](#4-labs) | Hands-on labs for practical learning. |
| [4.1 Lab 1: Ternary Operator MUX](#lab-1-ternary-operator-mux) | Build a 2:1 MUX with ternary operator. |
| [4.2 Lab 2: Synthesis Using Yosys](#lab-2-synthesis-using-yosys) | Synthesize the MUX with Yosys. |
| [4.3 Lab 3: GLS of MUX](#lab-3-gate-level-simulation-gls-of-mux) | Run GLS on the MUX netlist. |
| [4.4 Lab 4: Bad MUX Example](#lab-4-bad-mux-example) | Identify and fix Verilog pitfalls. |
| [4.5 Lab 5: GLS of Bad MUX](#lab-5-gls-of-bad-mux) | Observe mismatches in GLS. |
| [4.6 Lab 6: Blocking Assignment Caveat](#lab-6-blocking-assignment-caveat) | Fix blocking assignment issues. |
| [4.7 Lab 7: Synthesis of Blocking Caveat Module](#lab-7-synthesis-of-blocking-caveat-module) | Synthesize the corrected module. |
| [5. Summary](#5-summary) | Recap of key concepts. |
| [6. Highlights](#6-highlights) | Key achievements from Day 4. |
| [7. Key Takeaways](#7-key-takeaways) | Critical learnings for VLSI design. |

---

## 1. Gate-Level Simulation (GLS) 🛠️

**Gate-Level Simulation (GLS)** verifies the synthesized netlist, ensuring:

- ✅ Functional correctness
- ⏱️ Timing behavior
- ⚡ Power estimates
- 🔧 Test structures (e.g., scan chains for DFT)

### Why GLS?
| **Purpose**            | **Description**                                      |
|------------------------|-----------------------------------------------------|
| **Synthesis Validation** | Confirms RTL-to-gate translation accuracy.          |
| **Timing Verification** | Checks for setup/hold violations using SDF files.   |
| **Testability**         | Validates scan chains and DFT features.             |

### When to Perform GLS?
- **Post-Synthesis**: After RTL is converted to a gate-level netlist.
- **Pre-Physical Design**: To catch issues before layout.

### Types of GLS
| **Type**          | **Description**                          |
|-------------------|------------------------------------------|
| **Functional GLS** | Logic-only simulation (zero/unit delays).|
| **Timing GLS**     | Uses annotated timing data for real-world behavior. |

> [!NOTE]  
> GLS is a cornerstone of the **RISC-V SoC Tapeout Program**, ensuring reliable netlists before tapeout with Synopsys tools and SCL180 PDK.

---

## 2. Synthesis-Simulation Mismatch 🚨

A **synthesis-simulation mismatch** occurs when RTL simulation (pre-synthesis) differs from gate-level simulation (post-synthesis) or hardware behavior.

### Common Causes
| **Cause**                     | **Example**                              |
|-------------------------------|------------------------------------------|
| **Non-Synthesizable Constructs** | Delays, initial blocks, or unsupported code. |
| **Ambiguous Coding**           | Missing `else` clauses, incomplete sensitivity lists. |
| **Tool Differences**           | Simulation vs. synthesis tool interpretation. |

**Solution**: Write **synthesizable**, unambiguous RTL and adhere to best practices.

> [!TIP]  
> Simulate both RTL and gate-level netlists to catch mismatches early.

---

## 3. Blocking vs. Non-Blocking Assignments 🔄

Verilog uses two procedural assignment types: **Blocking** and **Non-Blocking**.

### 3.1 Blocking Statements (`=`) 📝
- **Syntax**: `=`
- **Execution**: Immediate, sequential.
- **Use Case**: Combinational logic (e.g., `always @(*)`).
- **Example**:
  ```verilog
  always @(*) begin
      y = a & b;
  end
  ```

### 3.2 Non-Blocking Statements (`<=`) ⏳
- **Syntax**: `<=`
- **Execution**: Concurrent, scheduled at end of time step.
- **Use Case**: Sequential logic (e.g., `always @(posedge clk)`).
- **Example**:
  ```verilog
  always @(posedge clk) begin
      q <= d;
  end
  ```

### 3.3 Comparison Table
| **Feature**                | **Blocking (`=`)**                       | **Non-Blocking (`<=`)**                  |
|----------------------------|------------------------------------------|------------------------------------------|
| **Operator**               | `=`                                      | `<=`                                     |
| **Execution**              | Sequential, immediate                    | Concurrent, end of time step             |
| **Update Timing**          | Instant, in code order                  | After time step                          |
| **Typical Use**            | Combinational logic, temp variables      | Sequential logic, registers/flip-flops   |
| **Inferred Hardware**      | Gates (combinational)                    | Flip-flops (sequential)                  |

> [!WARNING]  
> Mixing blocking and non-blocking assignments in the same block can cause simulation mismatches, especially in sequential logic.

---

## 4. Labs 🧪

| **Lab** | **Description** | **Key Focus** |
|---------|-----------------|---------------|
| [Lab 1](#lab-1-ternary-operator-mux) | 2:1 MUX with ternary operator | RTL design |
| [Lab 2](#lab-2-synthesis-using-yosys) | Synthesize MUX with Yosys | Synthesis flow |
| [Lab 3](#lab-3-gate-level-simulation-gls-of-mux) | GLS of MUX | Netlist validation |
| [Lab 4](#lab-4-bad-mux-example) | Bad MUX with pitfalls | Coding errors |
| [Lab 5](#lab-5-gls-of-bad-mux) | GLS of bad MUX | Mismatch detection |
| [Lab 6](#lab-6-blocking-assignment-caveat) | Blocking assignment issues | Assignment order |
| [Lab 7](#lab-7-synthesis-of-blocking-caveat-module) | Synthesize corrected module | Synthesis validation |

### Lab 1: Ternary Operator MUX
**Description**: Implements a 2:1 multiplexer using a ternary operator.  
**Functionality**: `y = i1` if `sel = 1`; else `y = i0`.

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
    assign y = sel ? i1 : i0;
endmodule
```

![Lab 1 Output](https://github.com/user-attachments/assets/3f5eb05a-1861-4bb8-940c-6ff9f2af87fb)

---

### Lab 2: Synthesis Using Yosys
**Description**: Synthesize the MUX from Lab 1 using Yosys.  
**Instructions**: Follow the standard Yosys synthesis flow (e.g., `read_verilog`, `synth`, `write_verilog`).

![Lab 2 Output](https://github.com/user-attachments/assets/7a0cdc7c-cbbd-4943-bd3d-130a0d66b9b1)

---

### Lab 3: Gate-Level Simulation (GLS) of MUX
**Description**: Run GLS on the synthesized MUX netlist.  
**Command**:
```shell
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```

![Lab 3 Output](https://github.com/user-attachments/assets/9acf45b3-2e42-4ac1-88ae-b4a494cc8d87)

---

### Lab 4: Bad MUX Example
**Description**: Demonstrates common Verilog pitfalls causing synthesis-simulation mismatches.  
**Original Code** (with issues):
```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
    always @(sel) begin
        if (sel)
            y <= i1;
        else
            y <= i0;
    end
endmodule
```

**Issues**:
- Incomplete sensitivity list (missing `i0`, `i1`).
- Non-blocking assignments (`<=`) in combinational logic.

**Corrected Code**:
```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
    always @(*) begin
        if (sel)
            y = i1;
        else
            y = i0;
    end
endmodule
```

![Lab 4 Output](https://github.com/user-attachments/assets/4c2ede06-0605-4ff0-99cb-fc89844b89e4)

---

### Lab 5: GLS of Bad MUX
**Description**: Perform GLS on the `bad_mux` to observe mismatches or warnings.  
**Expected**: Issues due to improper sensitivity list and assignments.

![Lab 5 Output](https://github.com/user-attachments/assets/2e698404-27b5-4c4a-a811-41b5fc13db77)

---

### Lab 6: Blocking Assignment Caveat
**Description**: Highlights issues with blocking assignment order.  
**Original Code** (with issue):
```verilog
module blocking_caveat (input a, input b, input c, output reg d);
    reg x;
    always @(*) begin
        d = x & c;
        x = a | b;
    end
endmodule
```

**Issue**: `d` uses old value of `x` due to assignment order.  
**Corrected Code**:
```verilog
module blocking_caveat (input a, input b, input c, output reg d);
    reg x;
    always @(*) begin
        x = a | b;
        d = x & c;
    end
endmodule
```

![Lab 6 Output](https://github.com/user-attachments/assets/42cac594-0008-4c7b-b415-43e6565b6081)

---

### Lab 7: Synthesis of Blocking Caveat Module
**Description**: Synthesize the corrected `blocking_caveat` module and verify results.  
**Instructions**: Use Yosys to synthesize and analyze the netlist.

![Lab 7 Output](https://github.com/user-attachments/assets/833bfacc-3b76-40fa-814c-47f0d783a6e0)

---

## 5. Summary 📚

| **Topic**                      | **Key Takeaway**                                                                 |
|--------------------------------|----------------------------------------------------------------------------------|
| **Gate-Level Simulation**      | Validates netlist functionality, timing, and testability post-synthesis.          |
| **Synthesis-Simulation Mismatch** | Avoid with synthesizable RTL and proper coding practices.                        |
| **Blocking vs. Non-Blocking**  | Use `=` for combinational, `<=` for sequential logic to ensure correct behavior.  |
| **Labs**                       | Practical exercises highlight GLS, coding pitfalls, and proper Verilog practices. |

---

## 6. Highlights 🌟

| **Achievement** | **Details** |
|-----------------|-------------|
| 🎯 **Mastered GLS** | Successfully ran gate-level simulations to validate netlists, aligning with RISC-V SoC program goals. |
| 🛠️ **Fixed Pitfalls** | Identified and corrected Verilog coding errors (e.g., bad sensitivity lists, assignment order). |
| ⚡ **Optimized Designs** | Applied Yosys synthesis and GLS to ensure robust, mismatch-free designs. |
| 📝 **Verilog Expertise** | Gained hands-on experience with blocking and non-blocking assignments for combinational and sequential logic. |

> [!NOTE]  
> These skills are directly applicable to the **RISC-V Reference SoC Tapeout Program**, preparing you for functional validation and GLS with Synopsys tools.

---

## 7. Key Takeaways 🔑

| **Concept** | **Takeaway** |
|-------------|--------------|
| **GLS Importance** | GLS ensures your netlist is functionally correct and timing-compliant before tapeout. |
| **Avoiding Mismatches** | Write clear, synthesizable RTL with complete sensitivity lists and proper assignments. |
| **Assignment Rules** | Use blocking (`=`) for combinational logic and non-blocking (`<=`) for sequential logic to prevent simulation issues. |
| **Practical Skills** | Labs reinforce real-world VLSI skills, from RTL coding to synthesis and GLS, critical for the RISC-V SoC flow. |

> [!TIP]  
> Always simulate both RTL and gate-level netlists, and review synthesis/simulation warnings to catch issues early! 🚀

---

## 🌟 Ready to Dive Deeper?
- 💻 Write testbenches for the labs using iverilog/GTKWave (tools in the RISC-V SoC program).
- 🔧 Experiment with Synopsys tools (Design Compiler, PrimeTime) for GLS and timing analysis.
- 🚀 Apply these concepts to the **RISC-V Reference SoC Tapeout Program** for functional validation and tapeout preparation.