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

## 5. Labs on Optimization

### Lab 1

Below is the Verilog code for Lab 1:

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

**Explanation:**
- `assign y = a ? b : 0;` means:
  - If `a` is true, `y` is assigned the value of `b`.
  - If `a` is false, `y` is 0.

Follow the steps from [Day 1 Synthesis Lab](https://github.com/Ahtesham18112011/RTL_workshop/tree/main/Day_1#6-synthesis-lab-with-yosys) and add the following between `abc -liberty` and `synth -top`:
```shell
opt_clean -purge
```

![Lab 1 Output](https://github.com/user-attachments/assets/4d224d8d-f6f5-4a37-9732-ab570b64e31e)

---

### Lab 2

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Code Analysis:**
- Acts as a multiplexer:
  - `y = 1` if `a` is true.
  - `y = b` if `a` is false.

![Lab 2 Output](https://github.com/user-attachments/assets/59545745-8a8b-4afd-b4d5-0a3ad1d5b80e)

---

### Lab 3

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Functionality:**  
2-to-1 multiplexer; `y = a ? 1 : b` (outputs `1` when `a` is true, otherwise `b`).

![Lab 3 Output](https://github.com/user-attachments/assets/157b16d3-cecd-441a-aacf-bae296910886)

---

### Lab 4

Verilog code:

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```

**Functionality:**
- Three inputs (`a`, `b`, `c`), output `y`.
- Nested ternary logic:
  - If `a = 1`, `y = c`.
  - If `a = 0`, `y = !c`.
- Logic simplifies to:  
  `y = a ? c : !c`

![Lab 4 Output](https://github.com/user-attachments/assets/08d1e447-78c6-47c4-8c99-239645b38617)

---

### Lab 5

Verilog code:

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**
- D flip-flop with:
  - Asynchronous reset to 0
  - Loads constant `1` when not in reset

![Lab 5 Output](https://github.com/user-attachments/assets/a42fac06-a092-4efc-be39-33b263caaaa1)

---

### Lab 6

Verilog code:

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**
- D flip-flop always sets output `q` to `1` (regardless of reset or clock).

![Lab 6 Output](https://github.com/user-attachments/assets/ae45f7db-0a7f-4256-b43b-01cc4a1588f7)

---

## Summary
- **Focus:** Optimization techniques for combinational and sequential circuits in digital design, with practical Verilog labs.
  
- **Topics Covered:**
  1. **Constant Propagation:** Replacing variables with constant values to simplify logic and improve circuit efficiency.
  2. **State Optimization:** Reducing states and optimizing encoding in finite state machines to use less logic and power.
  3. **Cloning:** Duplicating logic cells/modules to improve timing and balance load.
  4. **Retiming:** Repositioning registers in a circuit to enhance performance without altering its function.

- **Labs:** Six practical Verilog labs illustrate these concepts, including examples of combinational logic optimizations and D flip-flop behaviors, each with code snippets and output images.
