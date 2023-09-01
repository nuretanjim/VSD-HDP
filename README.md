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

### DFF Asynchronous Set Synthesiss :
Commands for generating timing analysis of DFF with Asynchronous set are :
``` html
iverilog <name verilog: dff_async_set.v> <name testbench: tb_dff_async_set.v>
./a.out
gtkwave <name vcd file: tb_dff_async_set.vcd>
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/timing_async_set.png)

We can also synthesize DFF with Asynchronous set by following commands :
Commands for generating timing analysis of DFF with Asynchronous set are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_async_set.v>
yosys> synth -top <name: dff_async_set>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: dff_async_set>
```
Output is provied below: 


![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/dff_async_synth.png)


### DFF Synchronous Reset Synthesiss :
Commands for generating timing analysis of DFF with Synchronous reset are :
``` html
iverilog <name verilog: dff_async_set.v> <name testbench: tb_dff_sync_reset.v>
./a.out
gtkwave <name vcd file: tb_dff_async_set.vcd>
```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/timing_analysis_sync_reset.png)

We can also synthesize DFF with Asynchronous set by following commands :
Commands for generating timing analysis of DFF with synchronous reset are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_syncres.v>
yosys> synth -top <name: dff_syncres>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: dff_syncres>
```
Output is provied below: 


![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/synthesis_synch_res.png)


### Optimization of Special Circuit (Mult_2):
Commands for optimized mux_2 synthesis are :
``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: mult_2.v>
yosys> synth -top <name: mul2>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: mul2>

```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/syth_mul_2.png)

We can also generate the netlist by following commands :


``` html
yosys> write_verilog -noattr <name: mul2_net.v>
```
Output is provied below: 


![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/mul_2_netlist.png)

### Optimization of Special Circuit (Mult_8):
Commands for optimized mux_8 synthesis are :
``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: mult_8.v>
yosys> synth -top <name: mult8>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: mult8>
yosys> write_verilog -noattr <name: mult8_net.v>

```
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/synth_mult8.png)

We can also generate the netlist by following commands :


``` html
yosys> write_verilog -noattr <name: mul8_net.v>
```
Output is provied below: 


![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/synth_mult8.png)

### Day 3
## Table of Content
- Objective
- Synthesis Results

#### Objective
Day 3 discussess about optimizations for both combinational and sequential circuits. For combinational circuits optimizations are done for area and power savings ,  to achieve constant propagation , minimizing / simplifying boolean expressions. 
For sequential circuits some key optimization aspects are as followed:

 - Sequential constant propagation
 - state optimization
 - retiming
 - sequential logic cloning

#### Synthesis Result
##### Combinational logic optimizations for opt_check.v
Commands to synthesize optimized mux are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file:opt_check.v>
yosys> synth -top <name: opt_check>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show

```
Output is provied below: 
![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/Synth_opt_check.png)


##### Combinational logic optimizations for opt_check2.v
Commands to synthesize optimized mux are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check2.v>
yosys> synth -top <name: opt_check2>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show

```
Output is provied below: 

![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/opt_check2.png)

##### Combinational logic optimizations for opt_check3.v
Commands to synthesize optimized mux are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check3.v>
yosys> synth -top <name: opt_check3>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show

```
Output is provied below: 

![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/opt_check3.png)


##### Combinational logic optimizations for opt_check4.v
Commands to synthesize optimized mux are :

``` html
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check4.v>
yosys> synth -top <name: opt_check4>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show

```
Output is provied below: 

![alt text](https://github.com/nuretanjim/VSD-HDP/blob/main/opt_check4.png)



