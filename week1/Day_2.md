# Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles 

## 📌 Topics Covered
1. **Timing Libraries (`.lib`)**
   - Introduction to timing `.lib` files  
   - What `.lib` Contains
   -  Why `.lib` is Important
2. **Hierarchical vs Flat Synthesis**
   - hierarchical synthesis  
   - flat synthesis  
   - synthesis at sub module level  

3. **Flop Coding Styles and Optimization**
   - Why Flops?
   - Types of Resets 
   - Interesting Optimization Case


---
## Lab work
1. **Timing Libraries (`.lib`)**
## 📘 Introduction to Timing `.lib` Files

A **timing library (`.lib`)** is a text-based file (Liberty format) used in **VLSI design flow**.  
It contains **characterization data** of standard cells for synthesis, timing, and power analysis.

---

## 🔹 What `.lib` Contains
1. **Cell Definitions**
     - Pin names
     - Pin directions (input/output)
     - Logic functions

2. **Timing Information**
   - Propagation delay  
   - Setup & hold times  
   - Recovery & removal times  
   - Timing arcs (input → output)

3. **Power Information**
   - Internal switching power  
   - Leakage power  
   - Dynamic vs. static consumption

4. **PVT Corners**
   - `.lib` files are created for multiple **process-voltage-temperature (PVT)** corners:
     - **P (Process):** fast, slow, typical fabrication variations  
     - **V (Voltage):** supply variations  
     - **T (Temperature):** operating environment  
   
   what is meaning of sky130_fd_sc_hd__tt_025C_1v80.lib
- **tt** → typical process  
- **025C** → 25 °C  
- **1v80** → 1.8 V supply
  
```
gvim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/229af8ac-36d0-4eb0-bece-118336af5c5f" />

## 🔹 Why `.lib` is Important
- Used in **logic synthesis** to map RTL → standard cells.  
- Ensures **timing closure** by accounting for delays at different corners.  
- Essential for **EDA tools** like Yosys, OpenSTA, Synopsys DC, Innovus, etc. 

2. **Hierarchical vs Flat Synthesis**

**Hierarchical Synthesis**
- In **hierarchical synthesis**, the design is kept **module-wise** (sub-modules remain intact).  
- Each module is synthesized independently, then combined at the top level.  
- This gives **better readability and reuse** when modules repeat multiple times.

#### Example: Multiple Module Design
```
gvim multiple_modules.v
```
<img width="940" height="621" alt="image" src="https://github.com/user-attachments/assets/33fde73e-00c0-48f3-8eb7-b8ecd429fdba" />

Synthesized view multiple_modules.v
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```
<img width="940" height="679" alt="image" src="https://github.com/user-attachments/assets/81cc1e05-fadf-4706-8be4-b9949aa59c84" />

Writing Hierarchical Netlist
```
write_verilog -noattr multiple_modules_hier.v
!gvim multiple_modules_hier.v
```
Hierarchical netlist:
<img width="940" height="684" alt="image" src="https://github.com/user-attachments/assets/24914a7e-5182-458e-9591-42b6a90acb0a" />

**Observation**
-The design shows NAND gate usage instead of stacked PMOS (since stacked PMOS is inefficient).
-Sub-modules are preserved → more structured netlist.

**Flat Synthesis**
 - In flat synthesis, all modules are flattened into one level.
 - The hierarchy is removed → one large netlist.
 - Tools optimize across module boundaries, but readability reduces.

For flatten synthesis
```
read_verilog multiple_modules.v
flatten
write_verilog -noattr multiple_modules_flat.v
!gvim multiple_modules_flat.v
```
Flat synthesis netlists:

<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/28ebb1fc-e293-42ba-b266-3ba2deb5a8a6" />
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/39951954-008f-4274-bdbb-39f3567ce9af" />

**Observation:**
Sub-modules are removed.
Final design = single-level netlist.
Optimization possible across module boundaries

**If we want syntehsis at sub module level**

```
synth -top sub_module
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
Sub-module synthesis:

<img width="940" height="593" alt="image" src="https://github.com/user-attachments/assets/b0a434b8-452f-43b6-a8c8-1982536eadc2" />

**Observation:**
-When multiple instances of the same sub-module exist, it is better to synthesize module-wise.
-Divide and conquer approach:
-Each module synthesized once → replicated at top level.
-Final top-level netlist is more optimized and manageable.


3. **Flop Coding Styles and Optimization**
### 🔹 Why Flops?
- A **flip-flop (flop)** is a sequential element used to **store values** across clock cycles.  
- Required to build **state elements** in digital designs.  
- Resets ensure flops start in a **known state**.  

---

### 🔹 Types of Resets

1. **Asynchronous Reset**
   - Reset is effective **independent of the clock**.  
   - Output goes to reset state immediately when reset is asserted.  

✅ Simulation:
asynchornous reset waveform
```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```
<img width="940" height="585" alt="image" src="https://github.com/user-attachments/assets/3b1335ac-cd21-4f4d-b822-ee4b222c4835" />
2. **Asynchronous set**
    - Similar to async reset, but forces flop to logic 1.

✅ Simulation:
Asynchornous set waveform
```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave  tb_dff_async_set.vcd
```
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/7aa036a3-44a4-4dff-bdb9-6200f1f34eec" />
3. **Synchronous Reset**
 - Reset works only when clock edge occurs.
 - Gives more predictable timing, often preferred in synthesis.

✅ Simulation:
```
iverilog dff_async_set.v tb_dff_syncres.v
./a.out
gtkwave  tb_dff_syncres.vcd
```
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/1226c4ae-d520-4f0a-bf23-445ee8b4146f" />

🔹 **Flop Synthesis Examples**

1.**Asynchronous Reset**
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
Netlist:

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/1b458496-d7b0-4d19-a2ef-49b4e4c8d0d1" />
2. **Asynchronous set**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
Netlist
<img width="940" height="589" alt="image" src="https://github.com/user-attachments/assets/35360e0c-e5a6-49d6-86ea-947a8fc7ada2" />

3. **Synchronous Reset**
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
Netlist: 

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/028dc8cc-5008-4da8-b493-5470334a8b7d" />


🔹 **Interesting Optimization Case**

 RTL code
```
gvim mutl_*.v -o
```
RTL View:
<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/69293013-b476-4ab7-b913-91f5c5676a8a" />

Taking example of mul2
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mul_2.v
synth -top mul2
```
No cells → no memory → simple wire mapping.
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
it is showing dont call abc as there is nothing to map
```
show
```

Synth view:
<img width="940" height="603" alt="image" src="https://github.com/user-attachments/assets/f996308d-6af5-404f-8f51-ef4150fc609a" />

Netlist
```
write_verilog -noattr mul2_net.v
!gvim mul2_net.v
```
<img width="940" height="599" alt="image" src="https://github.com/user-attachments/assets/09bf3c3f-ec1b-4b2e-a7d6-7c599867d333" />

taking example of mul8
synthesized view
<img width="940" height="598" alt="image" src="https://github.com/user-attachments/assets/84d4b6fd-6ff2-474a-842c-642ff7e7ce81" />
netlist
<img width="940" height="597" alt="image" src="https://github.com/user-attachments/assets/78a22dcb-f363-452d-a473-a790e74a5165" />

## ✅ Summary – Flop Coding & Optimization

- **Flops** are essential sequential elements with reset/set options.  
- **Asynchronous reset/set** → Immediate response, independent of clock.  
- **Synchronous reset** → Works with clock, ensures better timing predictability.  
- **Optimization cases (like multipliers)** → May result in no logic if synthesis simplifies the design.  
- **Synthesis tools** can optimize RTL code heavily depending on functionality and constraints.  






