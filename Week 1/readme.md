# WEEK 1
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Participants](https://img.shields.io/badge/Participants-3500+-success?style=for-the-badge)
![India](https://img.shields.io/badge/Made%20in-India-saffron?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdo....)
## OBJECTIIVE
- To Yosys tool to generate netlist for various design
- To optimize design 
- To use iverilog and Gtkwave.
### Day 1
**Yosys**
- A RTL to netlist converter. It requires design file and standard library file.
- For verification of netlist level design given by yosys use iverilog and gtkwave and use same testbench file as used before.
- **.lib** file consists of all standard cell used to generate netlist. It contains cell of different speed. Use fast cells for less delay 
           but chances of hold violation increases , Use slow cells for no hold violation but chnaces of setup violation iincreases tradeoff fast cell and slow cell(medium).
**Iverilog**
Icarus Verilog (iverilog) is an open-source Verilog simulator. It compiles Verilog source code into an executable simulation.
Combined with GTKWave, it provides visibility into signal waveforms.

Workflow:
-Write RTL and testbench.
-Compile with iverilog.
-Run the output executable.
-Open .vcd dump in GTKWave.

This ensures functional correctness of the RTL before synthesis.
Below github repo consists of std lib file
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files/
```
**Compile and verify**
 ```bash
 iverilog good_mux.v tb_good_mux.v
 ./a.out 
 gtkwave tb_good_mux.vcd
 ```

## Yosys
- Workflow:
    -Read RTL → ```bash read_verilog your_design.v ```
    -Synthesize Logic → ```bash synth -top <module name> ```
    -Map to Standard Cells → ```bash abc -liberty your_lib.lib```
    -View Design (Optional) → ```bash show ```
    -Export Netlist → write_verilog output.v
- example
 ```bash
  yosys
 ```
 ```bash
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  read_verilog good_mux.v
  synth -top good_mux
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  show
 ```
![Output](good_mux.png) 

- For Netlist
```bash
 write_verilog -noattr good_mux_netlist.v
 !vim good_mux_netlist.v
```
- Performed Gate level synthesis using Sky130 PDK standard cells.

### Day - 2 --> Introduction to timing library and syntehsis methododlogy 
In __hierarchical design__, a big system is broken down into smaller sub-systems, arranged like a tree structure (top → middle → bottom)
 -Advantages:
   -Clear organization and modularity
   -Easier maintenance and scaling
   -Reusable sub-modules
   -Parallel team development is possible.
-Disadvantages:
 -More planning required initially
 -Slightly more overhead in connecting modules.
In __flat design__ , the entire system is built in one large block without breaking it into sub-modules. All components are described together.It's used for simple designs.
 -Advantages:
  -Fast to design for small systems
  -No extra module connections.
 -Disadvantages:
  -Hard to debug and modify
  -Not reusable
  -Not scalable for large designs
  -Becomes messy and confusing

### Examples:
- For Hierarchial design.
   - Open Yosys
     ```bash
     yosys
     ```
   - Synthesis
     ```bash
     read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     read_verilog multiple_modules.v
     synth -top multiple_modules 
     abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
     show multiple_modules
     ```
 - __OUTPUT__
   ![multiplt_module](multimodules.png)

- For Flat design
  - Open Yosys
     ```bash
     yosys
     ```
   - Synthesis
     ```bash
     read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     read_verilog multiple_modules.v
     synth -top multiple_modules 
     abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     flatten
     show multiple_modules
     ```
 - __OUTPUT__
   ![Flatten](flatten_synthesis.png)

## Synchronous and Asynchronous flip flops

-A __synchronous__ flip-flop changes its output only when a clock pulse arrives.
-Clock signal controls when the flip-flop updates.
-All flip-flops are triggered together using the same clock.
-Inputs like __SET__ and __RESET__ also work only at the clock edge.
 -__Iverilog and gtkwave__
  ```bash
  iverilog dff_syncres.v tb_dff_syncres.v 
  ./a.out
  gtkwave tb_dff_syncres.vcd
 ```
- __Synthesis__
  ```bash
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  read_verilog dff_syncres.v
  synth -top dff_syncres
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  show
  ```
- __Output__
  ![dff_sync](dff_sync.png)

-An __asynchronous__ flip-flop can change its output anytime, independent of the clock, usually through asynchronous SET or RESET pins.
-Special inputs like preset (SET) and clear (RESET) act immediately, not waiting for clock.
-These are often called “direct inputs.”

-__Iverilog and gtkwave__
  ```bash
  iverilog dff_async.v tb_dff_async.v 
  ./a.out
  gtkwave tb_dff_async.vcd
 ```
- __Synthesis__
  ```bash
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  read_verilog dff_async.v
  synth -top dff_async
  dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
  show
  ```
- __Output__
  ![dff_async](dff_async.png)


  ### Day 3 - Combinational and sequential circuit optimization
   - __Logic optimization__ is the process where a synthesis tool automatically transforms a gate-level circuit into a  smaller,faster, and more power-efficient version without changing its logical function. The primary goal is to meet the design constraints for timing (performance), power consumption, and chip area.

   - __Combinational logic__ optimization means reducing the complexity (gates, area, delay, power) of a digital logic circuit without changing its function.
      - __Karnaugh Map (K-map) / Quine–McCluskey Minimization__
      -Tools internally use advanced algorithms (similar to K-map for small circuits) to get minimum SOP (Sum of Products)           or POS (Product of Sums).
      -K-map is practical for up to 4–6 variables.
      -Quine–McCluskey is algorithmic and works for larger functions.
      - __Common Subexpression Elimination__
       -If the same logic expression appears multiple times, tools compute it once and share the result.
       -Example:
       -Y1 = A·B + C
       -Y2 = A·B + D
       -Instead of two A·B blocks → compute A·B once, use for both Y1 and Y2.
     - __Constant Propagation and Folding__
      -If some inputs are fixed (0 or 1), tools substitute constants and simplify.
      -Example:
       -Y = A·0 → Y = 0
       -Z = B + 1 → Z = 1
     - __Unused Logic Elimination__
      -If some part of the circuit does not affect any output, tools remove it.
       -Example:
        -A temporary wire not connected to any output i.e eliminated.
     - __Don’t-Care Condition Exploitation__
      -If some input combinations never occur, tools can treat them as “X” (don’t care) and optimize aggressively.
       Common in FSMs, encoders, decoders.
        Benefit: Reduces number of minterms, simplifies expressions.
    - __Technology Mapping Optimization__
     -After logic simplification, tools map optimized logic to available standard cells or FPGA LUTs.
      They try to pick the smallest and fastest combination of gates for the given function.
    - __MUX (Multiplexer) Optimization__
     -Multiplexer optimization means simplifying or reorganizing large multiplexer structures to reduce area, power, and           delay.
     -This usually happens when the HDL code has nested if-else or case statements.
     - If we have nested if-else statements GLN will use multiple mux for exah if - else statement.

### In yosys optimization
    - Use Command:
     ```bash
     opt_clean -purge
     ```
    - __Example__
    -
  
         
