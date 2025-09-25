# Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles 

## 📌 Topics Covered
1. **Timing Libraries (`.lib`)**
   - Introduction to timing `.lib` files  
   - Understanding characterization data  
   - Interpreting delays, setup/hold, and power data  

2. **Hierarchical vs Flat Synthesis**
   - Pros and cons of hierarchical synthesis  
   - When to use flat synthesis  
   - Practical examples in Yosys  

3. **Flop Coding Styles and Optimization**
   - Different flip-flop coding styles in Verilog  
   - Synthesis impact of coding styles  
   - Flop optimizations during synthesis  

---
## Lab work
**Introduction to timing .libs
```
gvim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/229af8ac-36d0-4eb0-bece-118336af5c5f" />
**Hierarchical vs Flat Synthesis**
*1.Hierarchical Synthesis*
multipe module netlist
```
gvim multiple_modules.v
```
<img width="940" height="621" alt="image" src="https://github.com/user-attachments/assets/33fde73e-00c0-48f3-8eb7-b8ecd429fdba" />
synthesize the multiple_modules.v
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```
<img width="940" height="679" alt="image" src="https://github.com/user-attachments/assets/81cc1e05-fadf-4706-8be4-b9949aa59c84" />

Netlist
```
write_verilog -noattr multiple_modules_hier.v
```

<img width="940" height="684" alt="image" src="https://github.com/user-attachments/assets/24914a7e-5182-458e-9591-42b6a90acb0a" />



