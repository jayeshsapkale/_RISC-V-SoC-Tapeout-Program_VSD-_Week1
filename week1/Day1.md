
# 📘 Day 1 – Introduction to Verilog RTL Design & Synthesis

## 🔹 Topics Covered

### 1. Verilog & Testbench Basics

* What is a **Simulator**?
* What is a **Design** in RTL?
* What is a **Testbench**?
* How a simulator works.
* **Icarus Verilog (iverilog)** simulation-based flow.

### 2. Lab Work with Iverilog & GTKWave

* Lab introduction.
  * step 1: Clone the Workshop Repository
  ```
  git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
  cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
  ```
  * snapshot :
    <img width="940" height="547" alt="image" src="https://github.com/user-attachments/assets/0d694427-defe-457a-8691-369b9c79bb3a" />

  * Step 2: check all file lib ,my_lib,verilog_files
  * Step 3: Simulate the Design
  Compile the design and testbench:
```
   iverilog good_mux.v tb_good_mux.v
```
  Run the simulation:

    ./a.out
  View the waveform:

    gtkwave tb_good_mux.vcd
  * snapshot:
    <img width="940" height="663" alt="image" src="https://github.com/user-attachments/assets/75f3aa2b-9eba-4847-8004-089685016095" />
* Step 4: To see in file structre ie design and waveform

      gvim tb_good_mux.v -o good_mux.v
  *snapshot:
  <img width="940" height="708" alt="image" src="https://github.com/user-attachments/assets/e6af9080-f42d-4075-90c3-338a46b19d10" />


### 3. Introduction to Yosys (Synthesizer)

* What is a **Synthesizer**?
* How to verify synthesis results.
* **Logic synthesis flow:**

  * RTL design written in Verilog → Synthesized netlist (gate-level).
* Translation from **RTL to Gate-Level** using Yosys.

### 4. Understanding `.lib` Files

* What is a `.lib` file?
* Why do we need **different flavours of gates**?
* Role of **slow cells** and **fast cells**.
* Trade-off: **Faster cell vs Slower cell**.
* How synthesis tool selects appropriate cells from `.lib`.

### 5. Lab Work with Yosys

* Hands-on synthesis of a Verilog design.
* 1.Start Yosys

      yosys
* 2.Read the liberty library

      read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
  
* 3.Read design

      verilog_files/good_mux.v
  <img width="940" height="426" alt="image" src="https://github.com/user-attachments/assets/cb8d5a39-0d6f-4c8d-9649-07486735716c" />

* 4.what module is going to synthesize

      synth -top good_mux
  <img width="940" height="704" alt="image" src="https://github.com/user-attachments/assets/931c3a85-6291-404f-b5da-5a62128fe8e6" />
<img width="940" height="698" alt="image" src="https://github.com/user-attachments/assets/ef3eb9a1-10b8-46fe-ba48-5e5fc1d059b3" />

* 5.To generate it convert netlist it convert rtl file into a gate

      abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

<img width="940" height="698" alt="image" src="https://github.com/user-attachments/assets/07490c85-20be-4ff2-b07d-fd9710fa4aab" />

* 6.To show what logic used
  
      show
  <img width="940" height="686" alt="image" src="https://github.com/user-attachments/assets/92f681fb-964c-4ce2-8958-70546370a05a" />
  
* 7.To command netlist

      write_verilog good_mux_netlist.v
  <img width="940" height="238" alt="image" src="https://github.com/user-attachments/assets/787691a4-a3d6-4a55-b33e-00bbf0d5a148" />
  
* 8.To open it

      !gvim good_mux_netlist.v
  <img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/19b88369-0da6-4bc9-b420-b6ec33051d7f" />

* 9.for simplified netlist

      write_verilog -noattr good_mux_netlist.v
      !gvim good_mux_netlist.v
  
  <img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/ff1fbaac-d7c3-49ac-b4aa-497d5aa69ebc" />
   
---

✅ **Outcome:**
Day 1 introduced me to **simulation (iverilog, GTKWave)** and **synthesis (Yosys, Sky130 PDK)**.
I learned the **end-to-end RTL design flow**, from writing a Verilog design → testing with a testbench → simulating → synthesizing → analyzing gate-level output.
