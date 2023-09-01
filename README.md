# VSD-HDP
### Day 0 
## Table of Content

 Installation of Tools
- Yosys
- iverilog
- gtkwave

## Yosys Installation
  ``` html
  $ git clone https://github.com/YosysHQ/yosys.git
  $ cd yosys-master 
  $ sudo apt install make (If make is not installed please install it) 
  $ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
  $ make config-gcc
  $ make 
  $ sudo make install
  ```
 ![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/Yosys%20Installation.png)


## iVerilog Installation
  ``` html
  Steps to install iverilog
  $sudo apt-get install iverilog
  ```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/iverilog%20Installation.png)


## GTKWave Installation
  ``` html
Steps to install gtkwave
$sudo apt update
$sudo apt install gtkwave
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/GTK%20Wave%20Installation.png)


### Day 1
## Table of Content
- Objective
- Source Files
- Simulation Results
- Synthesis Results

## Objective
RTL design needs to satisfy timing analysis and synthesis result should match the expected functionality. Day 1 focusses on how to use iverilog and gtkwave to perform the timing analysis. Besides, Yosys is introduced to execute the synthesis process. 

## Source Files 
The RTL code and testbench for a 2x1 Mux (good_mux.v,) and the library files for synthesis have been used from the following repository
https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

## Simulation Results
The commands for timing analysis using iverilog and gtkwave are given below :
``` html
  $ iverilog good_mux.v tb_good_mux.v
  $ ./a.out
  $ gtkwave tb_good_mux.vcd

  ```

![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/good_mux_timing_gtkwave.png)

## Synthesis Results
The commands for synthesis of 2x1 mux using Yosys are given below :

``` html
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

$ read_verilog good_mux.v

$ synth -top good_mux

$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib    # converts rtl code to gates based on the library.

$ show   #command to show logical version of synthesis that is reliazed

```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/2x1%20good_mux%20synthesis.png)



### How to create netlist :

write_verilog good_mux_netlist.v   #we can give any name here .


write_verilog -noattr good_mux_netlist.v  #simplifies netlist 


### Day 2
## Table of Content
- Objective
- Source Files
- Simulation Results
- Synthesis Results


## Objective
Day 2 describes the implementation of the library file . 


## Source Files 
The RTL code and testbench for Day 2 and the library files for synthesis have been used from the following repository
https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

## Synthesis Results
### Hierarchical and Flattened Synthesis
The commands to synthesis multiple module file is given below :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: multiple_modules.v>
yosys> synth -top <name: multiple_modules>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: multiple_modules>
yosys> write_verilog -noattr <name: multiple_modules_hier.v>
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/hierarchical%20synthesis%20of%20multiple_module.png)

The netlist for multiple module is as followed:
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/netlist_hierarchical_multiple_modoules.png)

Commands to generate flattened version of synthesiss are :
``` html
yosys> flatten

```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/flattened_multiple_module.png)

netlist for flattned module can be viewed from this command
``` html
yosys> write_verilog -noattr <name: multiple_modules_flat.v>
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/netlist_flattened_multiple_modules.png)


### Submodule Synthesiss :
Sometimes to get better timing resuslts we might need to sythesis circuits from submodule level. To accomplish sub module level synthesis the commands will be :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: multiple_modules.v>
yosys> synth -top <name: sub_module1>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: sub_module1>
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/sub%20module%20synthesis%20for%20sub_module1.png)


### DFF Asynchronous Reset Synthesiss :
Commands for generating timing analysis of DFF with Asynchronous reset are :
``` html
iverilog <name verilog: dff_asyncres.v> <name testbench: tb_dff_asyncres.v>
./a.out
gtkwave <name vcd file: tb_dff_asyncres.vcd>
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/DFF%20asynchronous%20reset%20waveform%20.png)

We can also synthesize DFF with Asynchronous reset by following commands :
Commands for generating timing analysis of DFF with Asynchronous reset are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_asyncres.v>
yosys> synth -top <name: dff_asyncres>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: dff_asyncres>
```
Output is provied below: 
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/synthesis_dff_async_reset.png)


