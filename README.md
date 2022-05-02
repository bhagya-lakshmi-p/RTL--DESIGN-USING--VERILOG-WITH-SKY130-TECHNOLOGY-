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

read_verilog

write_verilog

Netlist file

Design


It is the representation of the design 
  in the form of standard cell

![image](https://user-images.githubusercontent.com/104748496/166217598-36b77345-5b37-441a-8719-744f707203f4.png) 
![image](https://user-images.githubusercontent.com/104748496/166223559-f4442675-dc5e-4480-ae32-994694ddab82.png)


