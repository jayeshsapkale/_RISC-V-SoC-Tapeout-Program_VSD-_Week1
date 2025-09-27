# Day 4 – GLS, Blocking vs Non-Blocking, and Synthesis-Simulation Mismatch  

### 1. GLS, Synthesis-Simulation Mismatch, and Blocking/Non-Blocking Statements  

- **Introduction to Gate-Level Simulation (GLS)**  
  - Netlist is logically same as RTL code.  
  - Same testbench can be reused with the netlist as DUT.  
  - GLS verifies **logical correctness after synthesis**.  

- **Why GLS?**  
  - Verify functionality post-synthesis.  
  - Ensure reset behavior and uninitialized flop handling.  
  - Check that design meets timing requirements.  
  - With **delay annotation (SDF)**, GLS can be **timing-aware**.  

---

### 2. Synthesis-Simulation Mismatch  

- **Causes:**  
  - Missing sensitivity list (`always @(a)` vs `always @(*)`).  
  - Blocking vs non-blocking misuse.  
  - Non-standard/un-synthesizable Verilog coding.  

- **Blocking (`=`) vs Non-Blocking (`<=`)**  
  - Inside `always` block:  
    - `=` → **Blocking assignment**: executes statements **in order**.  
    - `<=` → **Non-blocking assignment**: evaluates all RHS at entry, assigns to LHS **in parallel**.  

- **Caveats with Blocking Statements**  
  - Can cause mismatches between RTL simulation and synthesized hardware.  
  - ✅ Best practice:  
    - Use **blocking (`=`)** for *combinational* logic.  
    - Use **non-blocking (`<=`)** for *sequential* (clocked) logic.  
---

### 3. Lab on gls and synthesisi
#### GLS & Synth-Sim Mismatch  

**Files used:**  
Verilog code for a simple 2:1 multiplexer using a ternary operator:
```
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

Function: y = i1 if sel = 1; else y = i0.
```
gvim ternary_operator_mux.v -o bad_mux.v -o good_mux.v
```
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/559f91f1-3633-4954-8986-213a7fdb4782" />

first simulate basic ternary operator

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/38235fcf-506e-4d26-a7e9-7098edf750e3" />

synthesization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux.v
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr ternary_operator_mux_net.v
show
```

<img width="940" height="587" alt="image" src="https://github.com/user-attachments/assets/3a610861-fbaf-463b-b535-77f847a02878" />

 gls simulaton

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_vcd
```

<img width="940" height="598" alt="image" src="https://github.com/user-attachments/assets/626e3f68-c2f4-4ce0-8a7b-49c5ca2ba9ec" />

### Synthesis simulation mismatch behavior

Verilog code with intentional issues:
```
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```
Issues:
Incomplete sensitivity list: Should include i0, i1, and sel.
Non-blocking assignment in combinational logic: Should use blocking assignments (=).
Corrected version:
```
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```
 simulation:
 
 <img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/5fddadf2-2d64-4c41-bd02-083ad52189c4" />
 
 gls:
 
 <img width="940" height="599" alt="image" src="https://github.com/user-attachments/assets/be6069aa-d88f-497c-b9d8-ab9acec46fb2" />


### synth sim mismatch blocking statement
Verilog code:
```
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```
What’s wrong?
The order of assignments causes d to use the old value of x—not the newly computed value.
Best Practice: Assign intermediate variables before using them.
Corrected order:
```
always @ (*) begin
  x = a | b;
  d = x & c;
end
 ```

 <img width="940" height="607" alt="image" src="https://github.com/user-attachments/assets/49348108-3116-44aa-8857-ee0efff0beac" />

 simualtion
 
 <img width="940" height="602" alt="image" src="https://github.com/user-attachments/assets/cdd69556-aace-4431-81c9-35842a59ba2b" />

 gls simualtion

 
<img width="940" height="601" alt="image" src="https://github.com/user-attachments/assets/02cec293-543a-4245-8207-5d1993070be5" />
This Is clearly synth sim mismatch caused by blocking statement

## ✅ Key Learnings

- GLS validates **post-synthesis design correctness**.  
- Simulation-Synthesis mismatch is a **common debugging challenge**.  
- Always use **non-blocking (`<=`)** for sequential logic, **blocking (`=`)** for combinational logic.  
- Complete sensitivity list is **mandatory** (`always @(*)`).  
- Avoid **unintended latches** and **`initial` blocks** in synthesizable designs.



 
