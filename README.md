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
