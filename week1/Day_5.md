# Day 5 – Optimization in Synthesis

## 📘 Topics  

### 1. IF/CASE Constructs

#### IF Statements
- Used to create **priority logic** – the first condition with `if` has the **highest priority**.  
- **Caution / Danger:**  
  - Incomplete IF statements can **infer latches**.  
  - Bad coding style leads to unintended sequential behavior.  
- ✅ Best Practice:  
  - Ensure all branches are covered or use `else` to avoid inferred latches.

#### CASE Statements
- Used inside **always blocks**.  
- The controlling variable must be a **reg** type.  

**Caveats with CASE statements:**  
1. **Incomplete CASE**  
   - Can infer **latches**.  
   - ✅ Solution: Include a `default` branch.  

2. **Partial Assignment in CASE**  
   - Assign all outputs in every case segment.  
   - Avoid leaving outputs unassigned in some branches.  

3. **Overlapping CASE**  
   - Multiple case items match the same value.  
   - Be careful with **priority encoding** – synthesis may implement as priority encoder.  

### Lab 
### Labs on IF/CASE Constructs

#### 1. Incomplete IF – Latch Inference

**File:** `incomp_if2.v`  
```
module incomp_if2 (
    input i0, input i1, input i2, input i3,
    output reg y
);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```
### Observation

- **Incomplete IF statement** causes **latch inference**.  
- **Simulation** shows intended behavior, but **synthesis** infers a latch to hold the value when neither condition is true.


**Simulation:**

```
iverilog incomp_if.v tb_incomp_if.v
./a.out
gtkwave tb_incomp_if.vcd
```

<img width="940" height="584" alt="image" src="https://github.com/user-attachments/assets/e141a367-33bc-44ad-8a3f-5edd68130bc7" />

**Synthesization**
```
read_verilog incomp_if.v
synth -top incomp_tf
abc-liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="940" height="586" alt="image" src="https://github.com/user-attachments/assets/bf6c8807-02e0-4214-ab00-13da72590a71" />

**Incomplete CASE Simulation:**

Leaves some outputs unassigned → latches inferred.

simulation incomp_if2.v

```
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```

<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/c7d0a267-64c3-43e9-a804-892e23121e10" />

syntheziation
<img width="940" height="590" alt="image" src="https://github.com/user-attachments/assets/71e7da41-20c7-4c90-9e94-467530d5ef78" />

**incomplete cases**

files used

```
gvim comp_case.v -o incomp_case.v -o partial_case_assign.v -o bad_case.v
```
<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/e36c929a-8a44-4db6-b1de-050552b51e1a" />

simualtion of incomp case

<img width="940" height="600" alt="image" src="https://github.com/user-attachments/assets/67b00622-1377-4ec5-a9bc-eeaf3fb5c2ae" />

synthesization

<img width="940" height="586" alt="image" src="https://github.com/user-attachments/assets/7a7ab6ad-9ff4-4084-af5d-dafa0e9d42cc" />

***complete case**

```
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```

simulation

<img width="940" height="605" alt="image" src="https://github.com/user-attachments/assets/b67bb24c-9901-4f8d-a6df-c8cbf7bc5fca" />

synthesization

<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/66188461-41fb-43d0-bf70-518910e7574c" />
there are no latches

**partial case** 

synthezisation
<img width="940" height="600" alt="image" src="https://github.com/user-attachments/assets/555c7c3d-a229-4cef-9642-7ab81a4770b9" />

**bad case**

simulation


<img width="940" height="601" alt="image" src="https://github.com/user-attachments/assets/868ed975-dd76-42c1-97d3-9f99412890ca" />

overlaping wave is bad way of coding


### 2. for loop and for generate
for loop use in looping constructs used for evaluating expression
for generate is always used outside use to instantiate hardware

files used:
```
mux_generate is used
```
<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/ad1ebe8b-9efa-432f-863d-6553e1123731" />

simulation
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/f8a55826-b35d-4b6b-b87d-01ff6d5c131f" />

### for generate and for lab
file used 

<img width="940" height="597" alt="image" src="https://github.com/user-attachments/assets/07766068-c294-4b00-a8fb-8a1c96235d50" />

simulation

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/7580dd14-f3f3-4813-a971-dfa9a391a106" />

<img width="940" height="599" alt="image" src="https://github.com/user-attachments/assets/d5c1e8e4-e8a8-4265-b761-c7b2e4f77d83" />

same waveform in both case


rca.v and fa.v example

<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/920b2d81-e2e3-49fd-a7bf-8024e901a292" />

simulation

<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/57f56c09-d19d-4ce7-8cc2-677ac7ff1060" />









