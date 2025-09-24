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

# 🚀 Blocking vs. Non-Blocking Assignments in Verilog 🔄

In Verilog, not all assignments are created equal! There are **two flavors** of procedural assignments, and choosing the right one can save you from subtle hardware bugs. Let’s dive into **Blocking** and **Non-Blocking Assignments** to master their use in VLSI design! 🎉

---

## 📑 Table of Contents

| **Section** | **Description** |
|-------------|-----------------|
| [1. Blocking Assignments (`=`)] | Understand blocking assignments for combinational logic. |
| [2. Non-Blocking Assignments (`<=`)] | Explore non-blocking assignments for sequential logic. |
| [3. Comparison Table] | Compare blocking vs. non-blocking assignments. |

---

## 1. Blocking Assignments (`=`) 📝

Think of **blocking assignments** as **strict line-followers**. Each statement **finishes completely** before the next one begins, like following a recipe step-by-step—chop the onions, *then* fry them!

- **When to Use**: Combinational logic (e.g., `always @(*)`).
- **Behavior**: Updates are **immediate**, so later lines see the updated values right away.
- **Analogy**: Cooking sequentially—you can’t fry onions before chopping them!

**Example**:
```verilog
always @(*) begin
    y = a & b;   // AND first, y updated immediately
    z = y | c;   // Uses updated y value
end
```

> [!NOTE]  
> Blocking assignments (`=`) infer combinational logic (gates) and are ideal for logic that doesn’t depend on a clock.

---

## 2. Non-Blocking Assignments (`<=`) ⏳

**Non-blocking assignments** are patient and fair. They **read all right-hand side (RHS) values first**, then update all left-hand side (LHS) values **together at the end** of the time step, preventing order-dependent issues.

- **When to Use**: Sequential logic (e.g., `always @(posedge clk)`).
- **Behavior**: Updates are **scheduled**, ensuring all assignments occur concurrently.
- **Analogy**: A team working in parallel—everyone gathers their materials, then updates together.

**Example**:
```verilog
always @(posedge clk) begin
    q0 <= d;     // Schedule update for q0
    q  <= q0;    // Reads old q0, schedules update for q
end
```

> [!NOTE]  
> Non-blocking assignments (`<=`) infer sequential logic (flip-flops), critical for clocked designs like those in the RISC-V SoC.

<p align="center">
   <img src="block.jpeg" alt="GTKWave Counter Output" width="100%">
</p>


| **Aspect**               |                    **Left Side Image**                                                                 |             **Right Side Image**                                                                |
|--------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| **Code Example**         | ```verilog<br>always @(posedge clk) begin<br>    q <= q0; // (1)<br>    q0 <= d; // (2)<br>end``` | ```verilog<br>always @(posedge clk) begin<br>    q0 = d; // (1)<br>    q = q0; // (2)<br>end``` |
| **Line (1) Behavior**    | `q` gets the **old value** of `q0` (previous clock cycle).                                   | `q0` is **immediately updated** to `d`.                                                 |
| **Line (2) Behavior**    | `q0` is updated with `d` (scheduled for end of time step).                                   | `q` gets the **new value** of `q0` (which is `d`).                                      |
| **Effective Outcome**    | `q` and `q0` act as **two separate registers** (shift register behavior).                    | `q0` and `q` follow the **same signal**; only one register is needed.                   |
| **Synthesis Result**     | Creates **two flip-flops**: one for `q0`, one for `q`.                                       | Collapses to a **single flip-flop**: `q` directly stores `d`.                           |
| **Analogy**             | Like a relay race: `q` passes the old baton (`q0`) before `q0` gets a new one (`d`).         | Like a direct handoff: `q0` gets `d`, and `q` instantly takes it.                       |


---

## 3. Comparison Table

| **Feature**                | **Blocking (`=`)**                       | **Non-Blocking (`<=`)**                  |
|----------------------------|------------------------------------------|------------------------------------------|
| **Operator**               | `=`                                      | `<=`                                     |
| **Execution**              | Sequential, immediate                    | Concurrent, end of time step             |
| **Update Timing**          | Instant, in code order                  | Scheduled, after time step               |
| **Typical Use**            | Combinational logic, temp variables      | Sequential logic, registers/flip-flops   |
| **Inferred Hardware**      | Gates (combinational)                    | Flip-flops (sequential)                  |
| **Analogy**                | Cooking step-by-step                    | Team updating in parallel                |

> [!WARNING]  
> Mixing blocking and non-blocking assignments in the same block can cause synthesis-simulation mismatches, especially in sequential logic. Stick to `=` for combinational and `<=` for sequential!

---


## 🚀 Ready to Apply?
- 💻 Simulate these examples using iverilog/GTKWave to see blocking vs. non-blocking behavior.
- 🔧 Apply these rules in your RISC-V SoC RTL design to prevent synthesis-simulation mismatches.
- 🌟 Experiment with Synopsys tools (Design Compiler, PrimeTime) for synthesis and GLS.

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

<p align="center">
   <img src="tmux_wave.png" alt="GTKWave Counter Output" width="100%">
</p>

---

### Lab 2: Synthesis Using Yosys
**Description**: Synthesize the MUX from Lab 1 using Yosys.  
**Instructions**: Follow the standard Yosys synthesis flow (e.g., `read_verilog`, `synth`, `write_verilog`).

<p align="center">
   <img src="tmux_synth.png" alt="GTKWave Counter Output" width="100%">
</p>

---

### Lab 3: Gate-Level Simulation (GLS) of MUX
**Description**: Run GLS on the synthesized MUX netlist.  
## 📂 Required Files  
1️⃣ **primitives.v** → Standard cell primitives  
2️⃣ **sky130_fd_sc_hd.v** → SKY130 standard cell library  
3️⃣ **your_file.v** → Your synthesized netlist (design under test)  
4️⃣ **tb_your_file.v** → Testbench for simulation  

**Command**:
```shell
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux.v tb_ternary_operator_mux.
```

<p align="center">
   <img src="tmux_gls.png" alt="GTKWave Counter Output" width="100%">
</p>

---

## 🧪 Lab 4: The Misadventure of the Bad MUX 😱

**📘 Overview**: This lab showcases a classic **multiplexer (MUX)** gone wrong, highlighting pitfalls that lead to synthesis-simulation mismatches. Let’s debug and perfect it!

### ❌ Problematic Code: The Troubled MUX
Here’s the original code, riddled with issues:

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

**🚨 Issues Uncovered**:
- 🔴 **Incomplete Sensitivity List**: The `always @(sel)` block only triggers on `sel` changes, ignoring `i0` and `i1`. This leads to a mismatch between simulation (where outputs may not update) and synthesis (where tools assume combinational logic).
- 🔴 **Non-Blocking Assignments in Combinational Logic**: Using `<=` in a combinational block can cause unexpected behavior, as it’s meant for sequential logic.

<p align="center">
   <img src="dmux_wave.png" alt="GTKWave Counter Output" width="100%">
</p>

### ✅ Fixed Code: A Polished MUX
Here’s the corrected version, ready to shine:

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

**🎯 Takeaway**: Always use `@(*)` for combinational logic and `=` for immediate updates to avoid sneaky mismatches!

---

## 🧪 Lab 5: Gate-Level Simulation (GLS) of the Bad MUX 🔍

**📘 Overview**: Let’s run **Gate-Level Simulation (GLS)** on the problematic `bad_mux` to observe the chaos caused by its flaws.

**⚠️ Expected Behavior**:
- **Simulation Mismatch**: Due to the incomplete sensitivity list, the output `y` may not update correctly in simulation when `i0` or `i1` changes, while synthesis assumes full combinational behavior.
- **Tool Warnings**: GLS tools will likely flag warnings about the sensitivity list or mismatched behavior.

**🛠️ How to Test**:
1. Synthesize the original `bad_mux` using a standard cell library (e.g., Sky130).
2. Run GLS with a testbench toggling `i0`, `i1`, and `sel`.
3. Compare simulation results with expected combinational behavior.

**Command**:
```shell
write_verilog bad_mux_net.v
```
```shell
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux
```

**📊 Fixes Applied**:
- ✅ **Complete Sensitivity List**: Switched to `@(*)` to automatically include all inputs (`i0`, `i1`, `sel`), ensuring proper triggering.
- ✅ **Blocking Assignments**: Replaced `<=` with `=` for combinational logic, aligning simulation and synthesis behavior.

<p align="center">
   <img src="dmux_gls.png" alt="GTKWave Counter Output" width="100%">
</p>

**🎯 Takeaway**: GLS exposes mismatches early, saving you from costly debug cycles in hardware!

---

## 🧪 Lab 6: The Blocking Assignment Trap 🕳️

**📘 Overview**: This lab reveals how the order of **blocking assignments** in combinational logic can lead to unexpected results. Let’s fix a sneaky bug in the `blocking_caveat` module!

### ❌ Problematic Code: Order Matters
Here’s the original code with a subtle but critical flaw:

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
    reg x;
    always @(*) begin
        d = x & c;
        x = a | b;
    end
endmodule
```

**🚨 Issue Uncovered**:
- 🔴 **Wrong Assignment Order**: The output `d` is computed using the *old* value of `x` because `x = a | b` comes *after* `d = x & c`. This leads to stale data in simulation and synthesis.

### ✅ Fixed Code: Order Restored
Here’s the corrected version, ensuring logical flow:

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
    reg x;
    always @(*) begin
        x = a | b;
        d = x & c;
    end
endmodule
```

**📊 Fixes Applied**:
- ✅ **Correct Assignment Order**: Moved `x = a | b` before `d = x & c`, ensuring `d` uses the updated value of `x`.

**🎯 Takeaway**: In combinational logic, order your blocking assignments carefully to reflect data dependencies!

---

## 🧪 Lab 7: Synthesizing the Blocking Caveat Module 🏭

**📘 Overview**: Let’s synthesize the corrected `blocking_caveat` module using **Yosys** and verify a clean, optimized netlist.

**🔧 Synthesis Instructions**:
Run the following Yosys commands to synthesize the module:

```tcl
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
write_verilog blocking_caveat_netlist.v
```

**✅ Expected Results**:
- **Clean Synthesis**: No warnings or errors, as the corrected module uses proper sensitivity lists and assignment ordering.
- **Optimized Netlist**: The output `blocking_caveat_netlist.v` reflects the intended logic: `d = (a | b) & c`.

**🛠️ Verification Steps**:
1. Inspect the netlist for correct gate-level implementation.
2. Run GLS on the netlist to confirm matching behavior with RTL simulation.

**🎯 Takeaway**: Proper Verilog coding ensures smooth synthesis and reliable hardware implementation!

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
