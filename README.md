# RTL DESIGN USING VERILOG WITH SKY130 TECHNOLOGY

Course Content Index

Day1-Introduction to Verilog RTL design and synthesis

      .Introduction to open-source simulatoriverilog
      
      .Labs using iverilog and gtkwave
      
      .Introduction to Yosys and logic synthesis
      
      .Labs using Yosys and SKY130 PDKs
      
Day2-timing libs,hierarchical vs flat synthesis and efficient flop coding styles

      .Introduction to timing .libs
      
      .Hierarchical vs Flat synthesis
      
      .Various Flop coding styles and optimization
      
Day3-Combinational and Sequential optimization 

      .Introduction to optimization 
      
      .Combinational logic optimizations
      
      .Sequential optimizations for unused outputs
      
Day4-GLS,blocking vs non-blocking and Synthesis-Simulation mismatch

      .GLS ,Synthesis-Simulation mismatch
      
      .Labs on GLS,synthesis-simulation mismatch
      
      .Labs on Synthesis-Simulation mismatch for blocking statement
      
Day5-If case, for loop and for generate
      .If case construct
      
      .Labs on incomplete If case
      
      .Labs on Incomplete overlapping case
      
      .for loop and for generate
      
      .Labs on for loopand for generate




DAY1: Introduction to verilog RTL Design and Synthesis 

RTL Design is the implementation of specification written in the verilog code or set of verilog code ,it will be verified by the simulating the design with the help of simulator.

In this workshop "iverilog" is used for the simulation.

Tool used for the simulation is called as simulator. i.e, iverilog

To check whether design is meeting all the required specification or not is done by applying the stimulus to the design in the test bench(TB).

TB instantiate the design

TB not having any primary inputs or the primary outputs but design may have one or more than one primary inputs and primary outputs

![image](https://user-images.githubusercontent.com/104748496/166209855-65423a99-f0cd-4dca-bd8c-ea263025a322.png)

How the simulator works?

Simulator looks for the change in values of the input and the output is evaluated for every change in input values.There is no change in output value fot no change in input value.

iverilog based simulation

![image](https://user-images.githubusercontent.com/104748496/166211952-409b67dc-86a0-4d97-9991-96d55c8c517d.png)

Environment setup:
    --->> File setup:to running the labs
    
             .make directory vlsi(mkdir vlsi)
             
             .Install vsdflow by gitcloning 
             
             .my_lib contains all the required library
             
             .verilog model having the all the standard cell(sc) present in the lib
             
             .verilog files contain all lab files (experiment)
             
 ![gitclone](https://user-images.githubusercontent.com/104748496/166213657-98ce0bb5-7657-4ba4-860c-1ef77f05bf42.PNG)
 
 How to work with iverilog and gtkwave?
 
 design folder-->verilog_files
 
 load the design to the verilog by using commond-- (iverilog module name.v tb_module name.v)
 
 a.out file created and dumpthe tb_modulename.vcd file(./a.out)
 
 load the vcdfile to the simulator to view the wave( gtkwave tb_modulename.vcd)
 
 ![gtk wave command](https://user-images.githubusercontent.com/104748496/166215354-785667b9-0a26-4a0f-aecf-0b3d808b88de.PNG)

Synthesizer is the tool used for converting RTL to netlist. 

here, Yosys used as synthesizer tool.

It is the representation of the design 
  in the form of standard cell
  
  Verify the synthesis by passing netlist and testbench to the iverilog and observe the gtkwave by using VCD file. 

![image](https://user-images.githubusercontent.com/104748496/166227407-47826a6b-6175-4b91-9075-c60fdec14028.png)

LOGIC SYNTHESIS:

RTL design behavioral representation of required specification written in verilog HDL.

RTL----> synthesis ---->Digital design circuit

Synthesis: RTL to Gate level transformation.

The design is converted into gates and connections are made between gates.

This gives output as a file called netlist.

What is .lib?

> collection of logical modules.

> includes basic logic gates like AND,OR,NOT etc..(standard cells).

> Different flavours of same gate.

    .2 I/P AND GATE
       -slow
    
       -medium 
       
       -fast
    
    .3 I/P AND GATE 
    
       -slow
    
       -medium 
       
       -fast
    
    .4I/P AND GATE
       
        ----

        ----
        
        ----
        
  Selection of cells
  
   .Need to guide the synthesizer to select the flavour of cell, that is optimum for the implementation oflogic circuit.
   
   .Guidance offered to the synthesizer is called "constraint".
  
![image](https://user-images.githubusercontent.com/104748496/166228722-8cf323e3-fcb2-40fd-939b-c6d49eb2da02.png)

tclk is the minimum clk period needed and fclk will be the maximum clk frequency. For the maximum performance the delay should be minimum, this can be acheived by faster cells which eliminates the requirement of the medium and slow cells.

Requirement of Slow cells

For the  hold related time issues in the flop b, there is requiremnt of slow cells, where the delay should be minimum. Thus we require fast cells to meet the performance and slow cells to meet the hold. 

The load in the digital logic circuits is the capacitance, the faster the charging and the discharging of the capacitance lesser the cell delay.

wide transistors provide more delay and less area and power.

narrow transistors provide more delay and less area and power.

![synthesis illustration](https://user-images.githubusercontent.com/104748496/166235181-5b0ad691-d3f7-4c21-9ec9-9c83fb87975b.PNG)

//for invoke yosys

yosys

read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

read_verilog designmodule.v

synth -top modulename

abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib

show

exit

![yosys 1](https://user-images.githubusercontent.com/104748496/166236787-3358b43b-7c35-4f36-810c-b0760b6c8e76.PNG)


Day2: Hierachial and Flat synthesis


![library](https://user-images.githubusercontent.com/104748496/166238961-4b9f1629-64cc-4ee8-bbc1-b22375c6b5bb.PNG)

Library files

The library file used is sky130_fd_sc_hd_tt_025C_1v80.lib. The nomenclature for library has some meaning and shows important parameters most importantly the Process,Temperature and Voltage.

These parameters are show in the end of the name "tt_025C_1v80".

  .tt means the process is "typical"

  .025C shows the temperature - 25 degree celsius

  .1v80 shows the voltage - 1.8V

![libraryparameter](https://user-images.githubusercontent.com/104748496/166239091-eeee0642-192f-4e6c-b666-bd8c56bcb01c.PNG)


Flavours of standard cells

As mentioned before, the library contains different flavours of same logic gates. This is done mainly to meet timing constraints.

Faster cells increases the clock frequency. T(clk) > T(cq_launch_flop) + T(combi) +T(setup_capture_flop)

Slower cells are required to prevent hold violations. T(hold_capture_flop) < T(cq_launch_flop) + T(combi)

These different flavours are named different within the library. For example, the different flavours of and gate present are:

based on delay: sky130_fd_sc_hd_and2_0 > sky130_fd_sc_hd_and2_2 > sky130_fd_sc_hd_and2_4

based on power/area: sky130_fd_sc_hd_and2_0 < sky130_fd_sc_hd_and2_2 < sky130_fd_sc_hd_and2_4

![Capturhire vs fla](https://user-images.githubusercontent.com/104748496/166244109-edff8a9d-6965-48d7-9a54-aac5328c55a1.PNG)
                                                          
Why submodule synthesis?

.when we have multiple instance of same module

. divide and conquer when massive.

![multiple](https://user-images.githubusercontent.com/104748496/166244780-f2ed7d87-4d2a-489a-81c5-d2cef43e797c.PNG)

![multiple_modules](https://user-images.githubusercontent.com/104748496/166245111-42f1bf05-cc66-4357-99bf-a34c70c51e34.PNG)

![Capturmodule flat](https://user-images.githubusercontent.com/104748496/166245923-53fdc44c-dc07-4034-9e60-f97478bc1989.PNG)

