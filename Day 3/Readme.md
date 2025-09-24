# 🌟 Day 3: Advanced Combinational & Sequential Optimization Superpowers  

🚀 **Welcome to Day 3** of the RTL Optimization Workshop! Today, we dive deeper into **combinational** and **sequential circuit wizardry**, unleashing techniques to craft ultra-lean, lightning-fast, and power-sipping designs. Prepare for cosmic-level enhancements! 🌌  

---

## 📑 Table of Contents  

| 🏷️ Section | 📘 Topic |
|------------|----------|
| 1️⃣ | [Constant Propagation](#-1️⃣-constant-propagation) |
| 2️⃣ | [State Optimization](#-2️⃣-state-optimization) |
| 3️⃣ | [Cloning](#-3️⃣-cloning) |
| 4️⃣ | [Retiming](#-4️⃣-retiming) |
| 5️⃣ | [Labs on Optimization](#-5️⃣-labs-on-optimization) |
| 🔬 | [Lab 1 → Lab 8](#labs-on-optimization) – Now with extras! |

---

## 🔹 1️⃣ Constant Propagation  

💡 **Core Idea:** Hunt down signals locked to constants and swap 'em in – zap redundant logic like a black hole sucking in debris!  

✅ **Steps (Enhanced):**  
- Scan for constant drivers (e.g., tied inputs, parameters)  
- Propagate through muxes, gates, and flops  
- Fold constants (e.g., 2'b10 & 2'b11 = 2'b10)  
- Purge dead code post-propagation  

🎯 **Super Benefits:**  
- 🚀 Turbo timing: Shave ns off paths  
- 📉 Gate apocalypse: Fewer cells = smaller die  
- 🔋 Eco-mode: Less switching = greener chips  
- 💰 Cost cutter: Optimizes for ASIC/FPGA fab  

 <p align="center">
   <img src="const.png" alt="GTKWave Counter Output" width="60%">
</p>

**Pro Tip:** In Yosys, `opt` pass auto-handles this; watch for warnings on unpropagated seq constants.

---

## 🔹 2️⃣ State Optimization  

💡 **Epic Quest:** Shrink FSMs into compact powerhouses – merge states, encode like ninjas!  

🔧 **Advanced Techniques:**  
- ✂️ *State Reduction:* Implode equivalent states using partitioning algorithms  
- 🧮 *Encoding Mastery:* Binary (compact), Gray (glitch-free), One-Hot (FPGA-friendly), or custom for power  
- 🔗 *Logic Minimization:* Espresso/K-map on next-state logic + output decoding  
- 🔋 *Power sorcery:* Clock gating unused states, Gray for minimal toggles, async resets where safe  
- 🌟 *Bonus:* FSM extraction in tools like Yosys (`fsm` pass) detects and optimizes hidden machines  

**Fancy Example:** A traffic light FSM with Gray encoding to slash dynamic power by 30%!  

```verilog
module traffic_fsm (
    input clk, rst_n, sensor,
    output reg [1:0] light  // 00=Red, 01=Yellow, 11=Green
);
    reg [1:0] state, next_state;  // Gray: minimizes transitions
    
    always @(*) begin
        case (state)
            2'b00: next_state = sensor ? 2'b01 : 2'b00;  // Red to Yellow
            2'b01: next_state = 2'b11;                   // Yellow to Green
            2'b11: next_state = sensor ? 2'b00 : 2'b11;  // Green to Red
            default: next_state = 2'b00;
        endcase
        light = state;  // Moore output
    end
    
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n) state <= 2'b00;
        else state <= next_state;
    end
endmodule
```

Yosys `fsm` optimizes to fewer FFs; simulate toggles to verify power win!

---

## 🔹 3️⃣ Cloning  

💡 **Duplicate Dynamo:** Clone bottleneck logic to fan-out like a hydra – balance loads, conquer timing!  

⚙️ **Workflow (Powered Up):**  
1. 🕵️‍♂️ Identify critical paths via timing reports (e.g., high fan-out nets)  
2. 🔄 Clone cells/modules with `techmap` or manual dupes  
3. 📉 Demux heavy fan-outs into parallel paths  
4. 📍 Phys-aware placement: Clone near consumers  
5. ✅ Re-validate: Check power bump vs. timing gain; use retiming post-clone  
6. 🌟 *Fancy Twist:* Sequential cloning for datapaths – duplicate FFs for pipeline balance  

  <p align="center">
   <img src="cloning.png" alt="GTKWave Counter Output" width="60%">
</p>

**Example Snippet:** Clone a mux for high-fan-out adder result.  

```verilog
wire [7:0] sum;  // High fan-out
assign sum = a + b;

// Clone for timing-critical uses
wire [7:0] sum_clone1 = sum;  // For path1
wire [7:0] sum_clone2 = sum;  // For path2
```

In Yosys/ABC, `-D` flag aids delay-driven cloning.

---

## 🔹 4️⃣ Retiming  

💡 **Register Shuffle:** Relocate FFs across logic clouds to even delays – warp clock cycles like spacetime! ⏱️  

📌 **Steps (Cosmic Edition):**  
1. 🧩 Model as retiming graph (nodes=logic, edges=delays)  
2. 🔄 Push/pull FFs: Forward for setup relief, backward for hold fixes  
3. ✅ Preserve I/O behavior (no functional shift!)  
4. 🎯 Tune: Faster clk, lower power via balanced paths  
5. 🌟 *Advanced:* Combine with pipelining; Yosys lacks native, but ABC extensions or manual Verilog tweaks simulate  

**Fancy Example:** Retime a pipelined adder – move FF mid-logic for 20% freq boost.  

```verilog
module retimed_adder (input clk, [7:0] a, b, output reg [8:0] sum);
    wire [8:0] partial = a + b;  // Long path
    reg [7:0] a_reg, b_reg;      // Pre-FFs
    
    always @(posedge clk) begin
        a_reg <= a; b_reg <= b;
        sum <= a_reg + b_reg;    // Retimed: FF before add
    end
endmodule
```
 <p align="center">
   <img src="retimed.png" alt="GTKWave Counter Output" width="60%">
</p>

Tools like Vivado auto-retime; manual for control.

---

## 5. Labs on Optimization  

🔬 **Expanded Labs:** Original 6 + 2 fancy extras! Use Yosys: Add `opt -purge; fsm; opt` between `abc` and `synth -top`. Simulate with Icarus, view waves in GTKWave.  

### Lab 1: Basic Const Prop Mux  
```verilog
module opt_mux1 (input a, b, output y);
    assign y = a ? b : 1'b0;  // Const 0 propagates
endmodule
```
**Explanation:**
- `assign y = a ? b : 0;` means:
  - If `a` is true, `y` is assigned the value of `b`.
  - If `a` is false, `y` is 0.
  - 
**Expected:** Simplifies to AND gate. Run `opt_clean -purge` – y = a & b!

```shell
read_verilog opt_check.v
synth -top opt_check
opt_clean purge
abc ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

 <p align="center">
   <img src="opt_check.png" alt="GTKWave Counter Output" width="60%">
</p>

---

### Lab 2: Const 1 Mux  
```verilog
module opt_mux2 (input a, b, output y);
    assign y = a ? 1'b1 : b;  // Const 1
endmodule
```  
**Expected:** OR gate: y = a | b.  

 <p align="center">
   <img src="opt_check2.png" alt="GTKWave Counter Output" width="60%">
</p>

---

### Lab 3: Duplicate Mux (State Opt Tease)  
```verilog
module opt_mux3 (input a, b, output y);
    assign y = a ? 1'b1 : b;  // Same as Lab 2, but add param
    parameter CONST_VAL = 1'b1;
endmodule
```  
**Expected:** Same OR; param propagates for reconfigurability.  

 <p align="center">
   <img src="opt_check3.png" alt="GTKWave Counter Output" width="60%">
</p>

---

### Lab 4: Multiple Modules Flatten Optimixation   
```verilog
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 


endmodule
```  
**Expected:** XOR-like: y = a ^ ~c. Fancy fold!  

 <p align="center">
   <img src="multiple_module_opt.png" alt="GTKWave Counter Output" width="60%">
</p>


---

### Lab 5: DFF Const Load (Seq Prop Fail)  
```verilog
module dff_const_load (input clk, reset, output reg q);
    always @(posedge clk, posedge reset) begin
        if (reset) q <= 1'b0;
        else q <= 1'b1;  // Const, but reset blocks full prop
    end
endmodule
```  
**Expected:** Full FF retained – can't optimize Q to const due to reset.  

<p align="center">
   <img src="dff_const1.png" alt="GTKWave Counter Output" width="60%">
</p>

---

### Lab 6: Full Const DFF  
```verilog
module dff_full_const (input clk, reset, output reg q);
    always @(posedge clk, posedge reset) begin
        if (reset) q <= 1'b1;
        else q <= 1'b1;  // Always 1!
    end
endmodule
```  
**Expected:** Optimized to tie-high; no FF needed.  

<p align="center">
   <img src="dff_const2.png" alt="GTKWave Counter Output" width="60%">
</p>

---

### Lab 7: Two D Flip Flops 
```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```  
**Expected:** One DFF acts as SET and the other DFF acts as RESET..!

  <p align="center">
   <img src="dff_const3.png" alt="GTKWave Counter Output" width="300%">
</p>

**OUTPUT WAVEFORM**
 <p align="center">
   <img src="dff_const3_wave.png" alt="GTKWave Counter Output" width="300%">
</p>

- The graph shows the simulation of a D Flip-Flop with reset.
- When reset = 1, output Q is cleared to 0 (irrespective of input).
- When reset = 0, Q updates on every rising clock edge.
- The sudden drop of Q happens because reset overrides the clocked input.
- Once reset is released, Q resumes normal operation, following the input.

---

### Lab 8: Constant output = No need of Flops  
```verilog
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```
<p align="center">
   <img src="dff_const4.png" alt="GTKWave Counter Output" width="300%">
</p>

<p align="center">
   <img src="flops.png" alt="GTKWave Counter Output" width="300%">
</p>

<p align="center">
   <img src="dff_const4_wave.png" alt="GTKWave Counter Output" width="300%">
</p>

- The graph shows the simulation of a D Flip-Flop with reset
- When reset = 1, output Q is cleared to 0 (irrespective of input).
- When reset = 0, Q updates on every rising clock edge.
- The sudden drop of Q happens because reset overrides the clocked input.
- Once reset is released, Q resumes normal operation, following the input.

---


### Lab 9: Two D Flip Flops with No optimzation
```verilog

module dff_const5(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b0;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```  
- Two flip-flops are used: q and q1.
- On reset = 1, both outputs (q and q1) are cleared to 0.
- After reset, q1 is forced to constant 1 at every clock cycle.
- q copies the value of q1, so it becomes 1 one clock later.
- The output stabilizes at q = 1 regardless of input conditions.
- The extra flip-flop (q1) is redundant → synthesis tool will optimize it away.

  <p align="center">
   <img src="dff_const5.png" alt="GTKWave Counter Output" width="300%">
</p>

---

## Summary  
- **Ultimate Focus:** Master optimizations for combinational/sequential circuits – from const zaps to FSM shrinks, cloning hydras, and retiming warps.  
- **Topics Amplified:**  
  1. **Constant Propagation:** Const folding for instant wins.  
  2. **State Optimization:** FSM encoding/reduction for power/area magic.  
  3. **Cloning:** Duplicate for timing triumphs.  
  4. **Retiming:** FF dances for freq boosts.  
- **Labs Galaxy:** 8 Verilog gems showcase Yosys powers – synthesize, optimize, conquer! 🚀
