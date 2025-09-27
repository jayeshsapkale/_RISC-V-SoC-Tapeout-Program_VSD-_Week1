# Day 5 – Optimization in Synthesis

## 📘 Topics  

### 1. IF/CASE Constructs
- **IF statements**  
  - Used for **conditional logic**.  
  - Synthesis tools can optimize redundant conditions.  

- **CASE statements**  
  - Used for **multi-way branching**.  
  - Supports **full case**, **incomplete case**, **overlapping case**.  
  - Optimization depends on **completeness** and **uniqueness** of case items.  

- **Key Learnings**  
  - Incomplete IF/CASE can infer **latches** unintentionally.  
  - Overlapping cases may cause **priority encoding** behavior.  
  - Synthesis tools can optimize **constant propagation** and **redundant branches**.  

---

### 2. For Loop and For-Generate
- **For Loop**  
  - Executes **sequential statements** during simulation.  
  - Can be used for repetitive combinational/sequential logic.  

- **For-Generate**  
  - Executes **at compile/synthesis time**.  
  - Generates **hardware instances** of repeated modules or logic.  
  - Useful for scalable designs (like buses, arrays of registers).  

- **Key Learnings**  
  - Loops reduce code repetition.  
  - For-generate ensures **structural replication** in hardware.  
  - Tools optimize loops by **unrolling** or **sharing resources**.  

---

## 🧪 Labs  

### Labs on Incomplete IF
- **44-SKY130RTL D5SK2 L1** → Lab Incomplete IF Part 1  
- **45-SKY130RTL D5SK2 L2** → Lab Incomplete IF Part 2  

### Labs on Incomplete/Overlapping CASE
- **46-SKY130RTL D5SK3 L1** → Lab Incomplete Overlapping CASE Part 1  
- **47-SKY130RTL D5SK3 L2** → Lab Incomplete Overlapping CASE Part 2  
- **48-SKY130RTL D5SK3 L3** → Lab Incomplete Overlapping CASE Part 3  
- **49-SKY130RTL D5SK3 L4** → Lab Incomplete Overlapping CASE Part 4  

### Labs on For Loop and For-Generate
- **53-SKY130RTL D5SK5 L1** → Lab For and For-Generate Part 1  
- **54-SKY130RTL D5SK5 L2** → Lab For and For-Generate Part 2  
- **55-SKY130RTL D5SK5 L3** → Lab For and For-Generate Part 3  
- **56-SKY130RTL D5SK5 L4** → Lab For and For-Generate Part 4  
