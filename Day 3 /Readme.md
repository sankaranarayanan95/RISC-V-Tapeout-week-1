# 🌟 Day 3: Combinational and Sequential Optimization

Welcome to **Day 3** of this workshop!  
Today, we focus on **optimization techniques** for **combinational and sequential circuits**, exploring methods to improve efficiency, performance, and resource usage.  

---

## 📑 Table of Contents

| Section | Topic |
|---------|-------|
| 1 | [Constant Propagation](#-1-constant-propagation) |
| 2 | [State Optimization](#-2-state-optimization) |
| 3 | [Cloning](#-3-cloning) |
| 4 | [Retiming](#-4-retiming) |
| 5 | [Labs on Optimization](#-5-labs-on-optimization) |
| 5.1 | [Lab 1](#lab-1) |
| 5.2 | [Lab 2](#lab-2) |
| 5.3 | [Lab 3](#lab-3) |
| 5.4 | [Lab 4](#lab-4) |
| 5.5 | [Lab 5](#lab-5) |
| 5.6 | [Lab 6](#lab-6) |

---

## 🔹 1. Constant Propagation

**Definition:**  
Constant propagation is a compiler/synthesis optimization technique that **replaces variables with constant values** wherever possible.  

**Process:**
- Identify variables that hold constant values.  
- Replace them directly in logic.  
- Simplify resulting Boolean expressions.  

**Benefits:**
- ✅ Reduced complexity (smaller circuits)  
- ✅ Improved performance (shorter critical paths)  
- ✅ Resource optimization (fewer gates/flip-flops)

![Constant Propagation Example](https://github.com/user-attachments/assets/d7f06056-66c1-44af-99a8-623fdf5879be)

---

## 🔹 2. State Optimization

**Definition:**  
State optimization improves FSMs (Finite State Machines) by reducing the number of states or encoding them efficiently.  

**Techniques:**
- 🔸 **State Reduction** → Merge equivalent states.  
- 🔸 **State Encoding** → Use optimal binary codes for states.  
- 🔸 **Logic Minimization** → Apply Boolean algebra/Karnaugh maps.  
- 🔸 **Power Optimization** → Clock gating, gray encoding, etc.  

---

## 🔹 3. Cloning

**Definition:**  
Cloning duplicates logic cells/modules to improve **timing, load balance, or power distribution**.  

**Steps:**
1. Identify timing-critical paths.  
2. Duplicate the bottleneck cell/module.  
3. Redistribute fan-out loads.  
4. Place & route cloned cells for better performance.  
5. Validate with timing & power analysis.  

![Cloning Example](https://github.com/user-attachments/assets/6bdd2c12-02a2-4ea5-895c-98e349b93bac)

---

## 🔹 4. Retiming

**Definition:**  
Retiming repositions registers/flip-flops in a circuit **without changing functionality**, to balance delays and reduce clock period.  

**Process:**
1. Represent circuit as a graph.  
2. Shift registers across logic gates.  
3. Preserve functional correctness.  
4. Optimize for minimum clock period or lower power.  

---

## 🧪 5. Labs on Optimization

### ⚡ Lab 1
```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
