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
![Output](ana.png) 

- For Netlist
```bash
 write_verilog -noattr good_mux_netlist.v
 !vim good_mux_netlist.v
```

  
         
