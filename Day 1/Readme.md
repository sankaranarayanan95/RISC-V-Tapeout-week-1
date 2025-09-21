# 🌟 Day 1 – Introduction to Verilog RTL Design & Synthesis  

<div align="center">

![RTL](https://img.shields.io/badge/Verilog-RTL-blue?style=for-the-badge&logo=verilog)
![Synthesis](https://img.shields.io/badge/Yosys-Synthesis-green?style=for-the-badge&logo=opensourceinitiative)
![Icarus](https://img.shields.io/badge/Icarus-Verilog-orange?style=for-the-badge)
![GTKWave](https://img.shields.io/badge/GTKWave-Simulation-lightgrey?style=for-the-badge)

</div>

Welcome to **Day 1** of the **RISC-V SoC Tapeout RTL Workshop** 🚀  
Today marks the beginning of our design journey with **Verilog, Icarus Verilog (iverilog)**, and **Yosys synthesis**.  
We’ll explore essential RTL concepts, run hands-on labs, and build the foundation for complete digital design flow.  

---

## 📚 Table of Contents
1. [Simulator, Design & Testbench](#1-simulator-design--testbench)  
2. [Getting Started with iverilog](#2-getting-started-with-iverilog)  
3. [Lab: 2-to-1 Multiplexer Simulation](#3-lab-2-to-1-multiplexer-simulation)  
4. [Verilog Code Walkthrough](#4-verilog-code-walkthrough)  
5. [Yosys & Gate Libraries](#5-yosys--gate-libraries)  
6. [Lab: Synthesis with Yosys](#6-lab-synthesis-with-yosys)  
7. [Day 1 Summary](#7-day-1-summary)  

---

## 1️⃣ Simulator, Design & Testbench  

### 🔹 Simulator  
A **simulator** verifies the functionality of your circuit by applying test inputs and observing outputs—saving you from hardware errors.  

### 🔹 Design  
The **design** is your Verilog description of the intended logic.  

### 🔹 Testbench  
The **testbench** is a virtual lab environment that feeds inputs to your design and validates outputs.  

<div align="center">
  <img src="https://github.com/user-attachments/assets/93927b96-df80-4da5-b801-284fc2cc6757" alt="Design & Testbench Overview" width="70%">
</div>

---

## 2️⃣ Getting Started with iverilog  

**iverilog** is our go-to open-source Verilog simulator.  
Here’s the simulation flow:  

<div align="center">
  <img src="https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba" alt="iverilog Simulation Flow" width="70%">
</div>

➡️ Feed **design + testbench** → Run `iverilog` → Generate `.vcd` → View in **GTKWave**.  

---

## 3️⃣ Lab: 2-to-1 Multiplexer Simulation  

We’ll simulate a simple **2:1 Multiplexer** using `iverilog`.  

### 🔧 Step 1: Clone Repository  
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
