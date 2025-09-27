# Day 3 - Combinational and Sequential Optimizations

## 📌 Overview
This repository contains notes, labs, and resources for **Day 3** of the SKY130RTL course.  
Focus areas include **Combinational Logic Optimizations** and **Sequential Logic Optimizations**.

---

## 📚 Contents

### 🔹 Introduction to Optimizations
- Combinational logic – why and how it helps in design efficiency  
- Squeezing designs for maximum optimization  
- Benefits: **area and power savings**  

#### Techniques:
- Constant propagation  
- Direct optimization  
- Boolean logic optimization  

#### Sequential Logic Optimizations:
- Basics  
- Sequential constant propagation  
- Advanced state optimization  
- Retiming  
- Sequential logic cloning (used in **physical-aware synthesis**)  

---

### 🔹 Lab on Combinational Logic Optimizations

#### 🔸 Commands
Command for constant propogation all optimization

```
opt_clean -purge
```
### Example 1: Constant Propagation

### Verilog code

module opt_check (input a , input b , output y);
	assign y = a ? b : 0;
endmodule


📷 Synthesis Result

<img width="940" height="587" alt="image" src="https://github.com/user-attachments/assets/2f141c8d-2b56-414d-868b-76eadac6ca17" />


### Example 2: Direct Optimization

Verilog code

module opt_check2 (input a , input b , output y);
	assign y = a ? 1 : b;
endmodule


📷 Synthesis Result

<img width="940" height="582" alt="image" src="https://github.com/user-attachments/assets/d7ca684c-8677-40ce-a103-3a6483def05f" />

### Example 3: opt_check3

📷 Synthesis Result

<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/afefad02-00ac-49c0-891a-682e1668cde2" />

### Example 4: Multiple Modules

📷 Synthesis Result

<img width="940" height="600" alt="image" src="https://github.com/user-attachments/assets/7c85de7c-bf9c-4bed-b676-ecd2d9652cfc" />


---

### 🔹 Sequential Logic Optimizations

🔸 Verilog Files
```
gvim dff_const.v -o dff_const2.v
```
<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/a400ffaa-acce-4982-ac6f-b4a5804334bc" />

### Waveform for dff_const1.v


<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/9f827d8d-5d7b-484c-a7c7-bec54cde5f71" />

### Waveform for dff_const2.v

<img width="940" height="597" alt="image" src="https://github.com/user-attachments/assets/b012373b-7894-4efb-8393-855d65fa2f80" />

---
### 🔸 Synthesization Results of dff_const1

<img width="940" height="586" alt="image" src="https://github.com/user-attachments/assets/f92f7b52-c5b6-4960-a336-f7e828cba6a6" />

### 🔸 Synthesization Results of dff_const2

<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/79b4bd64-2cd7-459b-a6ee-7774456d605a" />

### 🔸 Synthesization Results of dff_const3

<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/7ef404fd-5a3b-41be-93e7-4eec6b7ffe2b" />

### 🔸 Synthesization Results of dff_const4

<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/f2782e4c-7425-487a-b804-949e51a77196" />

### 🔑 Key Insight

Not every flop with a constant gets optimized.

We must check:
 - Reset and set conditions
 - Whether Q value changes during operation


### 🔹 Sequential Optimizations for Unused Outputs

### 🔸 Example: counter_opt.v
```
gvim counter_opt.v
```
📌 It is an up-counter.

###🔸 Synthesis Flow
```
yosys
read_liberty -lib ../lib/sky130
read_verilog counter_opt.v
synth -top counter_opt
```

📷 Initial Synthesis

<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/291d5928-8dfb-40c1-93a8-6b65e1d0bceb" />

⚠️ Issue:

Expected 3-bit counter
Flops inferred incorrectly due to RTL code

🔸 Modified RTL

count[0] to count[2:0] == 3'b100


<img width="940" height="600" alt="image" src="https://github.com/user-attachments/assets/244f1e29-1540-4db1-b282-f66439d08eb2" />
 
 📷 Synthesized Design

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/73386dec-49fd-439c-b0ca-6d0d79d15ad3" />

### 🔑 Observation

- All inputs are not directly required
- Yosys optimizes away unused logic automatically

  ## ✅ Key Learnings

- **Constant propagation** eliminates redundant logic  
- **Direct / Boolean optimizations** reduce gate count  
- **Sequential optimizations** depend on reset/set conditions  
- **Unused outputs** are removed during synthesis  
- **Physical-aware synthesis** leverages cloning & retiming  

