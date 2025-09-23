# 🌟 Day 3: Combinational & Sequential Optimization  

Welcome to **Day 3** of the workshop!  
Today, we unlock **optimization superpowers** ⚡ for **combinational** and **sequential circuits**, making designs leaner, faster, and smarter.  

---

## 📑 Table of Contents  

| 🏷️ Section | 📘 Topic |
|------------|----------|
| 1️⃣ | [Constant Propagation](#-1️⃣-constant-propagation) |
| 2️⃣ | [State Optimization](#-2️⃣-state-optimization) |
| 3️⃣ | [Cloning](#-3️⃣-cloning) |
| 4️⃣ | [Retiming](#-4️⃣-retiming) |
| 5️⃣ | [Labs on Optimization](#-5️⃣-labs-on-optimization) |
| 🔬 | [Lab 1 → Lab 6](#labs-on-optimization) |

---

## 🔹 1️⃣ Constant Propagation  

💡 **Idea:** Replace variables that always hold a constant value → simplify logic instantly.  

✅ **Steps:**  
- Detect constant signals  
- Replace them in Boolean logic  
- Simplify expressions  

🎯 **Benefits:**  
- 🚀 Faster timing (shorter paths)  
- 📉 Fewer gates  
- 🔋 Energy-efficient  

![Constant Propagation](https://github.com/user-attachments/assets/d7f06056-66c1-44af-99a8-623fdf5879be)

---

## 🔹 2️⃣ State Optimization  

💡 **Idea:** Make **FSMs (Finite State Machines)** smaller, faster, and power-aware.  

🔧 **Techniques:**  
- ✂️ *State Reduction* → Merge duplicates  
- 🧮 *State Encoding* → Choose binary/Gray codes  
- 🔗 *Logic Minimization* → Boolean + K-map  
- 🔋 *Power Tricks* → Clock gating, Gray encoding  

---

## 🔹 3️⃣ Cloning  

💡 **Idea:** Duplicate critical logic to balance loads & improve timing.  

⚙️ **Workflow:**  
1. Spot critical path 🕵️  
2. Duplicate bottleneck cells 🔀  
3. Split heavy fan-outs 📉  
4. Place cloned cells closer 📍  
5. Validate timing & power ✅  

![Cloning Example](https://github.com/user-attachments/assets/6bdd2c12-02a2-4ea5-895c-98e349b93bac)

---

## 🔹 4️⃣ Retiming  

💡 **Idea:** Move registers around → balance delays, shrink clock cycle ⏱️  

📌 **Steps:**  
1. Convert circuit → graph 🧩  
2. Shift registers across logic 🔄  
3. Maintain functionality ✅  
4. Target faster frequency or lower power 🎯  

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
