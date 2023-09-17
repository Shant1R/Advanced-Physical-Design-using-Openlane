# Advanced-Physical-Design-using-Openlane
This repository is made to document the coursework done under Kunal Ghosh for ASICs and covers Advanced Physical Design using OpenLane and Sky130.

## DAY 1 - Inception of Open Source EDA

<details>
  <summary><strong>How to Communicate with Computers</strong></summary>
  Under this course, we will be looking into and learning how to design a chip, and a brief introduction to what is IPs and Macros.
  
  ![Screenshot from 2023-09-09 13-31-07](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/3e66eeb4-4133-4d36-8deb-2e4557ffb99d)

The image represents a package namely *QFN-48 Quad NO-leads*. The stucture is known as a package. It has a various i/p o/p ports with jtag to program it and various extensions as per need. It is important to note IPs and Marcos are different. Marcos are pure digital logic implementations, whereas IPs have some sort of intelligence in their functioning and generation. The IPs are provided by the foundry in operation and manufacture, and they provide some interface files that helps to communicate with the IPs. 

***Introduction to RISC-V***

RISC-V is an open-source instruction set architecture (ISA) for computer processors. An instruction set architecture defines the set of instructions that a processor can execute and the organization and behaviour of those instructions. RISC-V is unique in that any single company or organization does not own it. and it is freely available for anyone to use, modify, and implement without the need for licensing fees or proprietary restrictions.

![risc1](https://github.com/Shant1R/RISC-V/assets/59409568/a9782f60-fa86-454a-af08-6a7d56a4c4e2)
 
 - Application software (apps) and hardware are linked by 'system software'.There are various layers of **system software**. This includes major components like Compiler and Assembler.
 - The compiler compiles high-level codes like C and C++ to Instructions(eg: the codes inside .exe files) that can be read by the Assembler.
 - The Assembler converts it into binary codes which the machine can understand. The instructions act as an interface between the high-level language and the machine language.
 - The converted binary is then given to an RTL snippet that understands the instruction. This is done by a Hardware Description Language (HDL).
 - This is basically called RTL implementation and a netlist is being generated. with this, a physical design implementation of the design is generated.

The RISC-V has been designed to support extensive customization and specialization which can be extended  with  one  or  more  optional  instruction-set  extensions,  but  the  base  integer instructions cannot be redefine. The different instructions included in RISC-V are listed below.

1. Pseudo instructions - For e.g- mv,li,ret etc
2. Base integer instruction (RV64I, RV32I)-For e.g-lui,addi etc
3. Multiply extension (RV64M) -For e.g- mulw,divw etc
4. Single and double floating point instruction (RV64F, RV64D) -For e.g- flw,fadd etc
5. Application binary instruction 
6. Memory allocation and stack pointer
  
</details>

<details>
<summary><strong>SoC Design and OpenLane</strong></summary>

Under this section, we will look into the requirements and components of open source digital design of SoC - System on Chip for Application specific Integrated circuits.
  
  ![Screenshot from 2023-09-09 14-20-03](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/705c36e0-b84f-49c8-878a-c7fd8a2494b9)

The necesaary components are Resistor Transistor Logic Intellectual Property (RTL IPs), Electronic Design Automation (EDA) Tools and Process Design Kit (PDK) data, as shown in the image above. Now, we look into the open source tools and platforms that provide us with the various necessary tools. Initials days, most of the tools were under proprietary tools, but with the growth of the community and other benefactors, it became possible for the existance and maintainence of the open-source platforms.  

- *Opensource RTL Designs*: github, librecores, opencores
- *Opensource EDA tools*: QFlow, OpenROAD, OpenLANE
- *Opensource PDK data*: Google Skywater130 PDK

![Screenshot from 2023-09-09 14-20-33](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/74d7c9ab-e268-4336-9f83-9316f7d2e8e0)

Now, that we know the tools required, we will look into a ***simlplied flow from RTL code to GDSII*** and look into the steps involed in the deisgn.
  
![Screenshot from 2023-09-10 11-17-31](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/152f3807-db53-4cd1-9012-8a26b3679b1d)

We wil briefy go over the various steps and processes.

- *Synthesis*: RTL Converted to gate level netlist using standard cell libraries (SCL). An RTL design is created for a design specification using HDLs like Verilog or VHDL, or it can be created using high-level synthesis tools like SystemC, MATLAB HDL Coder etc.

- *Floor & Power Planning*: Planning of silicon area to ensure robust power distribution and has three stages
  - *Chip floor planning* : the chip is partitiones between different system building blocks and the IO ports are positioned.
  - *Macro fLoor planning* : the pin locations, dimensnions and rows are defined. 
  - *Power Planning* : the power network are connected to reduce the resistance and EM issues.

- *Placement*: Placing cells on floorplan rows aligned with sites
  - *Global Placement*: for optimal position of cells
  - *Detailed Placement*: for legal positions

- *Routing*: The routing stage involves determining the physical interconnections between standard cells, including metal layers and wires. OpenLane uses tools like TritonRoute to create a routed design that adheres to design rule constraints.

- *Signoff*: After placement and routing, OpenLane performs detailed design rule checking (DRC) and final verification to ensure the layout complies with fabrication constraints and meets specified requirements for timing, area, and power.

### Open Source ASIC Flow
With the release of open-source PDK, the whole open-source ASIC flow and methodology has been defined under ***OpenLane*** 
- We will look into the entire OpenLane flow, The flow displayed is much more detailed step wise than the one just overviewed. We will go over them one by one.
  
![Screenshot from 2023-09-10 12-30-10](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/62981537-0189-48e2-b70c-e2aaf21e7118)

1. ***Architectural Design*** – A system engineer will provide the VLSI engineer with specifications for the system that are determined through physical constraints. The VLSI engineer will be required to design a circuit that meets these constraints at a microarchitecture modeling level.

2. ***Synthesis*** -  The various steps are under -
   - *RTL Design/Behavioral Modeling* – RTL design and behavioral modeling are performed with a hardware description language (HDL). EDA tools will use the HDL to perform mapping of higher-level components to the transistor level needed for physical implementation. HDL modeling is normally performed using either Verilog or VHDL. One of two design methods may be employed while creating the HDL of a microarchitecture:
     - RTL Design – Stands for Register Transfer Level. It provides an abstraction of the digital circuit using
       i. Combinational logic
       ii. Registers
       iii. Modules (IP’s or Soft Macros)
     - Behavioral Modeling – Allows the microarchitecture modeling to be performed with behavior-based modeling in HDL. This method bridges the gap between C and HDL allowing HDL design to be performed
       
   - *RTL Verification* - Behavioral verification of design

   - *Logic Synthesis* – Logic synthesis uses the RTL netlist to perform HDL technology mapping. The synthesis process is normally performed in two major steps:

   - *GTECH Mapping* – Consists of mapping the HDL netlist to generic gates what are used to perform logical optimization based on AIGERs and other topologies created from the generic mapped netlist.

   - *Technology Mapping* – Consists of mapping the post-optimized GTECH netlist to standard cells described in the PDK

   - *Standard Cells* – Standard cells are fixed height and a multiple of unit size width. This width is an integer multiple of the SITE size or the PR boundary. Each standard cell comes with SPICE, HDL, liberty, layout (detailed and abstract) files used by different tools at different stages in the RTL2GDS flow.

   - *Post-Synthesis STA Analysis*: Performs setup analysis on different path groups.

3. ***DFT Insertion*** - Design-for-Test Circuit Insertion

4. ***Floor Planning and Power Planning*** - This is done by OpenROAD flow. The macros and IPs are placed in the core before proceding further. This is called as pre-placement. Floor planning is done separately for the macros and it is called macro floor planning. They are placed in such a way that they are closer to the inputs/outputs/other macros where more connections are present. Then to prevent the loading effects de-coupling capacitors are placed so that the logic states are well within the noise margin.
   When several blocks tap power from a single source, there is a problem of Voltage Droop at the Vdd and Ground Bounce at the Vss which can again push the logic out of the required noise margin into the undefined state. To mitigate this Vdd and Vss are placed as horizontal and vertical strips in the chip so that the blocks can tap power from the nearest source.

6. ***Placement*** - Place the standard cells on the floorplane rows, aligned with sites defined in the technology lef file. Placement is done in two steps: Global and Detailed.
   - In Global placement tries to find optimal position for all cells but they may be overlapping and not aligned to rows.
   - Detailed placement takes the global placement and legalizes all of the placements trying to adhere to what the global placement wants.


7. ***Clock Tree Synthesis(CTS)*** - Clock tree synteshsis is used to create the clock distribution network that is used to deliver the clock to all sequential elements. The main goal is to create a network with minimal skew across the chip. H-trees are a common network topology that is used to achieve this goal.

8. ***Fake Antenna and diode swapping*** - Long wires acts as antennas and cause accumulation of charges during the fabrication process damaging the transistor. To avoid this bridging is used to pass the wire through different layers or an antenna diode cell is added to leak away the charges
   - OpenLane approach - Insert Fake Diode to every cell input during placement. This matches the footprint of the library of the antenna diode. The Antenna Checker is run to check for violations, if there are violations then the fake diode is swapped with a real one.
   - OpenROAD approach - In the global route step, the antenna violation is addressed automatically by inserting an antenan diode OpenLane allows the user to chose either of the above approaches

9. ***Routing*** - This step is used to implement the interconnect using the different metal layers specified in the PDK. There are two steps
   - Global Routing - This is done inside the OpenROAD flow (FastRoute)
    - Detailed Routing - This is performed using TritonRoute outside the OpenROAD flow after the global routing. Before performing this step the Logic Equivalence Check is performed by Yosys, since OpenROAD does some optimisations the circuit.

10. ***RC Extension*** - From the .def file, the parasitic extraction is done to generate the .spef file (Standard Prasitic Exchange Format) which produces an accurate analog model of the circuit by including the parasitic effects due to wires, parasitic capacitances, etc.,

11. ***Static Timing Analysis(STA)*** - At this stage again OpenSTA is used to perform the Static Timing Analysis..

12. ***Sign-off***
    - *Design Rule Check* (DRC) is performed by Magic
    - *Layout Versus Schematic* (LVS) is performed by Netgen

13. ***GDS II Extraction*** - The routed .def file is used my Magic to generate the GDSII file.
</details>

<details>
<summary><strong>Open Source EDA Tools</strong></summary>

- To install and set up the environment for the OpenLane refer to [KanishR1 GitHub](https://github.com/KanishR1/Physical-Design-Using-Openlane)

- Now, we will enter the interactive mode for the workflow.
```bash
make mount
./flow.tcl -interactive
 ```

- Next we set the required package

```bash
package require openlane 0.9
``` 

- Now, to run the synthesis, we will first prep the design and run the synthesis

```bash
prep -design picorv32a
run_synthesis
```

- *NOTE* --> The netlist synthesis will be stopped for this because the netlist file of the given example already exists. To check the results and reports, one can refer the following folders shown below.

![Screenshot from 2023-09-10 15-27-48](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/a7454e45-25fb-4b87-aff7-cabd51b12047)

- The netlist file is under synthesis under the results folder as the verilog file. The various synthesis reports can be refered under the synthesis under reports.



</details>

## DAY 2 - Good Floor Plan vs. Bad Floor Plan & Introduction to Library cells

<details>
<summary><strong>Chip Floor Planning</strong></summary>
We will look into two parameters, Utilization factor and Aspect ratio, but before that we must look into the important terms in chip design.
  
  - *Die* : It is a small semiconductor material specimen that houses the core and the fundamental circuit is fabricated over this.
  - *Core* : It is the section of the chip where the fundamental design is placed.

***Utilisation Factor***
- The ratio of area occupied by the cells in the netlist to the total area of the core
- Best practice is to set the utilisation factor less than 50% so that there will be space for optimisations, routing, inserting buffers etc.,

***Aspect Ratio***
- Aspect ratio is the ratio of height to the width of the die.
- Aspect Ratio of 1 indicates that the die is a square die

These two Parameters are important to derive the width and height of the core and die, and now we can move ahead to define the location of preplaces cells. 

***Pre-placed Cells***
- Whenever there is a complex logic which is repeated multiple times or a design given by a third-party it can be perceived as abstract black box with input and output ports, clocks etc. We can also create black boxes ourselves for the design in case as per the requirements. They can be IPs or Macros
- These Macros and IPs are placed in the core at first before placing the standard cells and power planning. These are optimally such that the cells which are more connected to each other are placed nearby and oriented for input and ouputs.
- Once they have been placed, the location are not altered later on for routing. Thus they have been fixed on the chip.
- These pre-placed cells have to be surrounded with de-coupling capacitors.

***De-coupling Capacitors***
- The resistances and capacitances associated with long wire lengths can cause the power supply voltage to drop significantly before reaching the logic circuits. This can lead to the signal value entering into the undefined region, outside the noise margin range.
- De-coupling capacitors are huge capacitors charged to power supply voltage and placed close the logic circuit. Their role is to decouple the circuit from power supply by supplying the necessary amount of current to the circuit. They pervent crosstalk and enable local communication.

***Power Planning***
- Each block on the chip, however, cannot have its own decap unlike the pre-placed cells. Thus, when multiple units are discharging, we observe a ground bumb and in case of multiple charing units, we see a voltage droop.
- When thses are under noise range designed, we won't face any issue, but if they get beyond the defined noise range, we experience undesired behaviour from the design.
- To fix this issue, we will go for a better power plan for the chip, such that each unit can use the Vdd and Gnd near to it.
- A common way to accomplish this is to have VDD and VSS pads connected to the horizontal and vertical power and GND lines which form a power mesh.

***Pin Placement***
- The input, output and Clock pins are placed optimally such that there is less complication in routing or optimised delay.
- Note - CLK needs least resistive path, as they provide signals to all the flops continuously, thus have bigger IO ports.
- There are different styles of pin placement in openlane like *random pin placement*, *uniformly spaced* etc.,


***Run Floorplan on OpenLane***

- Importance files in increasing priority order:
  - *floorplan.tcl* - System default envrionment variables
  - *conifg.tcl*
  - *sky130A_sky130_fd_sc_hd_config.tcl*

- Floorplan envrionment variables or switches:
  - *FP_CORE_UTIL* - floorplan core utilisation
  - *FP_ASPECT_RATIO* - floorplan aspect ratio
  - *FP_CORE_MARGIN* - Core to die margin area
  - *FP_IO_MODE* - defines pin configurations (1 = equidistant/0 = not equidistant)
  - *FP_CORE_VMETAL* - vertical metal layer
  - *FP_CORE_HMETAL* - horizontal metal layer

*Note: Usually, vertical metal layer and horizontal metal layer values will be 1 more than that specified in the files*

Now, we will look into how to generate the floorplan using OpenLane.
```bash
run_floorplan
```

![Screenshot from 2023-09-10 18-27-10](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/6fcde85c-8222-4dd5-8f07-2de8b25b4386)

- We may review floorplan files by checking the ```floorplan.tcl```. The system defaults will have been overriden by switches set in ```conifg.tcl``` and further overriden by switches set in ```sky130A_sky130_fd_sc_hd_config.tcl```.

- Post the floorplan run, a .def file will have been created within the ```results/floorplan``` directory. It has the various informations such as the die area and unit lenghts used.

![Screenshot from 2023-09-10 18-54-00](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/906e337b-c9f7-4bb7-a612-a85216314525)


***View Floorplan on Magic***

To view the floorplan, Magic is invoked after moving to the ```results/floorplan``` directory:

```bash
 magic -T ~/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```

![Screenshot from 2023-09-10 19-52-24](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/92ba2d51-fa92-4af2-b809-3ba0666873c8)

One can zoom into Magic layout by selecting an area with left and right mouse click followed by pressing "z" key.

Various components can be identified by using the what command in tkcon window after making a selection on the component.

Zooming in also provides a view of decaps present in picorv32a chip.

The standard cell can be found at the bottom left corner.

You can clearly see I/O pins, Decap cells and Tap cells. Tap cells are placed in a zig zag manner or you can say diagonally

![Screenshot from 2023-09-10 19-53-44](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/3c17a30b-4362-4cc7-b9b0-447662ab54c9)


</details>

<details>
<summary><strong>Library Binding and Placement</strong></summary>

First and foremost, we need to bind the netlist with physical cells. We have shapes for OR, AND and every cell for pratice purpose. But in reality we dont have such shapes, we have give an physical dimensions like rectangles or squares weight and width. This information is given in libs and lefs. Now we place these cells in our design by initilaising it.

Now we look into Placement and its optimisation.

***Optimise Placement***

The next step is placement. Once we initial the design, the logic cells in netlist in its physical dimisoins is placed on the floorplan. Placement is perfomed in 2 stages:

- *Global Placement*: Cells will be placed randomly in optimal positions which may not be legal and cells may overlap. Optimization is done through reduction of half parameter wire length.
- *Detailed Placement*: It alters the position of cells post global placement so as to legalise them. Legalisation of cells is important from timing point of view.

Optimization is stage where we estimate the lenght and capictance, based on that we add buffers. Ideally, Optimization is done for better timing.

- Run placement on OpenLane
```bash
run_placement
```
![Screenshot from 2023-09-10 23-42-54](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/0775be8f-5965-4da1-82ad-d9cecbe81af8)

- The objective of placement is the convergence of overflow value. If overflow value progressively reduces during the placement run it implies that the design will converge and placement will be successful. Post placement, the design can be viewed on magic within ***results/placement*** directory:

```bash
magic -T ~/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```
![Screenshot from 2023-09-10 23-51-48](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/c93feb35-26c1-4108-b39b-78d1b1fbee7a)

- Zoomed in image.
![Screenshot from 2023-09-10 23-52-25](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/9f1c190c-fd29-467e-9258-1bb8668744e5)

***Note**: Power distribution network generation is usually a part of the floorplan step. However, in the openLANE flow, floorplan does not generate PDN. The steps are - floorplan, placement CTS and then PDN*
</details>

<details>
<summary><strong>Cell Design anf Characterization Flow</strong></summary>

Under this section, we will go through a thorough insight into the Characterizatiob flow and various steps involved, what are my inputs given, my intermediate outputs and final results we get.

***Standard cell design flow involves the following***

- *Inputs*:
  - PDKs
  - DRC & LVS rules
  - SPICE models
  - Libraries
  - User-defined specifications.

- *Design steps*:
  - Circuit design
  - Layout design (Art of layout Euler's path and stick diagram)
  - Extraction of parasitics
  - Characterization (timing, noise, power).

- *Outputs*:
  - CDL (circuit description language)
  - LEF
  - GDSII
  - extracted SPICE netlist (.cir)
  - timing, noise and power .lib files

***Standard Cell Characterization Flow***

A typical standard cell characterization flow includes the following steps:

1. Read in the models and tech files
2. Read extracted spice netlist
3. Recognise behaviour of the cell and buffers
4. Read the subcircuits
5. Attach the necessary power sources
6. Apply stimulus to characterization setup
7. Provide necessary output capacitive loads
8. Provide necessary simulation command

Now all 8 steps are provided together as a configuration file to a characterization software called **GUNA**. 

![Screenshot from 2023-09-11 10-44-50](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/0fc3ad3b-fe65-453f-a939-b444c32ba657)

This software generates timing, noise, power models. These .libs are classified as *Timing characterization*, *power characterization* and *noise characterization*.
  
</details>

<details>
<summary><strong>General Timing and Characterization Parameters</strong></summary>

Under this section, we will look into the timing characterization and get an understanding of various semantics and syntax of the three .lib files for noise, power and noise.

First we go through the various ***Timing Parameter Definitions***

Timing defintion | Value
------------ | -------------
slew_low_rise_thr  | 20% value
slew_high_rise_thr |  80% value
slew_low_fall_thr | 20% value
slew_high_fall_thr | 80% value
in_rise_thr | 50% value
in_fall_thr | 50% value
out_rise_thr | 50% value
out_fall_thr | 50% value

***Propagation Delay***

The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. Poor choice of threshold values lead to negative delay values. Even thought you have taken good threshold values, sometimes depending upon how good or bad the slew, the dealy might be still +ve or -ve.

```bash
Propagation delay = time(out_thr) - time(in_thr)
```

***Transition Time***

The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```bash
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```
  
</details>

## DAY 3 - Design Library Cell using Magic Layout and NGspice Characterization

<details>
<summary><strong>CMOS Invertor NGspice Lab</strong></summary>

Under this section, we will go in depth of Invertor cell, we would download the .magic file and perform the post layout simulation on NGspice and post characterise the sample cell and plug it in the OpenLane flow. NGspice is an open-source engine used to perform simulations. 

***IO Placer - Revise***
- PnR is a iterative flow and hence, we can make changes to the environment variables in the fly to observe the changes in our design.
- now, we want to change my pin configuration along the core from equvi-distance randomly placed to someother placement, we will set that IO mode variable on command prompt as shown 
```bash
set ::env(FP_IO_MODE) 2
```

- Floorplan after chaning the format of IO placement. We can see the pins are now not equi-distant.
![Screenshot from 2023-09-11 11-40-57](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/abd71a15-f2cc-4394-a4e5-ae75bb6e6989)


***Spice Deck Creation***
- Spice deack is the connectivity information of netlist. Thus it is a netlist that contains component connectivity, inputs to be provided and tap points for taking output and connectivity of the substrate.
- The source of PMOS is connected to Vdd and Source of NMOS is connected to GND, Vss in this case. Vin is given to the gates and Vout is taken out. We take the Cload as ```10fF``` for now.
- Now we define the PMOS and NMOS width and length as ```0.375um``` and ```0.25um``` respectively. We give 2.5V as Vdd and Vin. Common Vss is given.
- Identify the nodes, name them. Nodes are points between which a component is connected.
- We can now write the spice deck. We also specify the simulation type.
- We also import the model file for NMOS and PMOS for information of parameters related to transistors

 ![Screenshot from 2023-09-11 12-07-50](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/907f7818-01f2-45f6-a1c6-a7717c97c606)


***Spice Simulation***
- We will run the simulation for the deck created with different widths and lengths for the PMOS and NMOS.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/0965a8af-23bd-4de8-b330-c6afe17dc156)

- From the waveform, irrespective of switching the shape of it are almost same. We can see the characteristics are maintained across all sizes of CMOS. So CMOS as a circuit is a robust device hence use in designing of logic gates. Parameters that define the robustness of the CMOS are

- ***Switching Threshold (Vm)***
  - It is the point where out ```Vin = Vout```. To determine, we extend a 45 degree line from the origin.
  - At this point, both the transistors are in saturation region, means both are turned on and have high chances of current flowing driectly from VDD to Ground called Leakage current.
  - At this point, ```Vgs = Vds``` and ```Idsn = -Idsp```

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/56f76007-f1c0-4d1b-b288-e04731f8e25e)

- ***Rise and Fall Delay***
  -  We will run a transient simulation and plot Vin and VOut with respect to time.
  -  To determine the Rise time, we take the rising input and corresponding falling output and note the time for ```Vdd/2```, ie. 50% of the Vdd.
  -  For fall time, same is repeated but for the falling input and corresponding rising input.

### Steps to GIT CLONE vsdstdcelldesign

- We will git clone a custom made repo for this course in the OpenLane directory of our local system.

```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```

- To invoke magic to view the sky130_inv.mag file, the sky130A.tech file must be included in the command along with its path. To ease up the complexity of this command, the tech file can be copied from the magic folder to the vsdstdcelldesign folder.

- Invertor Layout using Magic

![Screenshot from 2023-09-11 12-41-19](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/c8317c08-1e8c-451a-9bd9-9eb1c710ce24)

</details>

<details>
<summary><strong>Inception of Layout and CMOS Fabrication Process</strong></summary>

Under this section we will look into the Fabrication process.
We will look into the various steps for 16-mask fab procedure

***16-MASK CMOS Process***
1. *Selecting a substrate*
   - We choose an appropriate substrate as per requirement.
   - We go with the most common substrate available - P-type.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/23a3c468-8ab5-4ee7-aa33-284a96f0248f)


2. *Creation of Active regions for transistors*
   - We have to make isolation for each pocket, this is done by growing Silicon Dioxide of 40nm over the P-type substrate, then deposit an 80nm layer of Silicon nitride.
   - Now deposit 1micron of photoresist. On this we make Mask1 and Mask 2 for the pockets and shower it with UV lights
   - The photoresist under the masks are protected and remaining is etched away with some chemical reaction. Now the mask is removed.
   - Now we etch off the extra silicon nitride, thus only silicon nitride left are the ones protected by the photoresist. Now Remove left photoresist.
   - Now, place the entire thing in oxidation furnace. Silicon nitride protects the SiO2 underneath from growing further.
   - The growth between the nitride layer acts as the isolation as they don't allow the transistor areas to communicate. This growth is also called bird's beak.
   - The remaining nitride layer is etched off.
   - This whole process is called ***LOCOS*** - *Local oxidation of Silicon*

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/952a1716-faf2-4a73-a8fd-d4df283efbe8)


3. *Formation of N-Well and P-Well*
   - The N-well and P-well regions are created separately.
   - P-well formation involves photolithography and ion implantation of p-type Boron material into the p-substrate. Energy required is 200keV.
   - N-well is formed similarly with n-type Phosphorus material. Energy requirement is 400keV.
   - This ion implantation damages the SiO2 layer.
   - High-temperature furnace processes drive-in diffusion to establish well depths, known as the twin-tub process.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/84d02f02-ccd2-452e-ae92-da8d84c5438e)

4. *Formation of Gate Terminal*
   - Gate is the most important terminal as here we control the input voltage.
   - Important parameters for gate formation include oxide capacitance and doping concentration.
   - A polysilicon layer is deposited and photolithography techniques are applied to create NMOS and PMOS gates.
   - The SiO2 layers over Nwell and Pwell are etched off using polysulpuric acid and fresh layer is made with goof thickness.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/e882aa97-ca64-4869-9b5d-d41ad1b76de1)

5. *Lightly-Doped Drain(LDD) Formation*
   - This is done to achieve a doping profile --> P+, P-, N for NMOS and N+, N- and P for PMOS.
   - LDD is created to control hot electron and short channel effects.
   

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/11e88b98-aaa3-4077-b46b-abff9b3f38c3)

6. *Source and Drain Formation*
   - Thin oxide layers are added to avoid channel effects during ion implantation.
   - N+ and P+ implants are performed using Arsenic implantation and high-temperature annealing.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/9830c896-7a03-47d5-b1be-d1e56ae01f94)
    
7. *Local Interconnect Formation*
   - Thin screen oxide is removed through etching in HF solution.
   - Titanium deposition through sputtering is initiated.
   - Heat treatment results in chemical reactions, producing low-resistant titanium silicon dioxide for interconnect contacts and titanium nitride for top-level connections, enabling local communication. 

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/7d491565-933b-43c4-a764-ee8f1945d074)

8. *Higher Level Metal Formation*
    - To achieve suitable metal interconnects, non-planar surface topography is addressed.
    - Chemical Mechanical Polishing (CMP) is utilized by doping silicon oxide with Boron or Phosphorus to achieve surface planarization.
    - TiN and blanket Tungsten layers are deposited and subjected to CMP.
    - An aluminum (Al) layer is added and subjected to photolithography and CMP.
    - This constitutes the first level of interconnects, and additional interconnect layers are added to reach higher-level metal layers.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/28b1cc30-49bc-4b40-bbbf-ad23b29ae14a)
  
9. *Dielectric Layer Addition*
    - Finally, a dielectric layer, typically Si3N4, is applied to safeguard the chip.
  
This complex process results in the creation of advanced integrated circuits with multiple layers of interconnects, essential for modern electronic devices.

### *Introduction to SKY130 Basic Layout and LEF*

From Layout, we see the layers which are required for CMOS inverter. Inverter is, PMOS and NMOS connected together.

- Gates of both PMOS and NMOS are connected together and fed to input(here ,A), NMOS source connected to ground(here, VGND), PMOS source is connected to VDD(here, VPWR), Drains of PMOS and NMOS are connected together and fed to output(here, Y).
- The First layer in skywater130 is localinterconnect layer(locali) , above that metal 1 is purple color and metal 2 is pink color.
- If we want to see connections between two different parts, place the cursor over that area and press S one times. The tkson window gives the component name.

![Screenshot from 2023-09-11 14-45-36](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/06f1731a-3266-4c8f-9085-e3c376cf8290)

***LEF - Library Exchange File***
- The layout of a design is defined in a specific file called LEF.
- It includes design rules (tech LEF) and abstract information about the cells.
  - *Tech LEF* - Technology LEF file contains information about the Metal layer, Via Definition and DRCs.
  - *Macro LEF* - Contains physical information of the cell such as its Size, Pin, their direction.

***Designing standard cell*** 
- First we need to provide bounding box width and height in tkson window. lets say that width of BBOX is 1.38u and height is 2.72u. The command to give these values to MAGIC is ```property Fixed BBOX (0 0 1.32 2.72)```
- After this, Vdd, GND segments which are in metal 1 layer, their respective contacts and atlast logic gates layout is defined Inorder to know the **logical functioning of the inverter**, we extract the spice and then we do simulation on the spice.

***SPICE extraction in MAGIC***

 To extract it on spice we open TKCON window, the steps are :
 
 - Know the present directory - pwd
 - create an extration file - the command is ```extract all``` and *sky130_inv.ext* files has been created
 - create spice file using .ext file to be used with our ngspice tool - the commands are
   - ```ext2spice cthresh 0 rthresh 0``` - extracts parasatic capcitances also since these are actual layers - nothing is created in the folder
   - ```ext2spice``` - a file *sky130_inv.spice* has been created.

![Screenshot from 2023-09-11 14-57-15](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/9babe270-cf93-49ef-b06b-4f6ec2b90865)



   
</details>


<details>
<summary><strong>Lab on SKY130 Tech File</strong></summary>

Under this section, we will go over how to infer the spice deck file and how to run the transient analysis using NGspice. Once the simulation is done, we will characterise the simulation plot. 

***Spice Deck***
- The design is scaled to ```0.01u```
- The NMOS and PMOS are defined as
  - ```cell_name drain_node gate_node source_node model_file_name```
 ```bash
M1000 Y A VGND VGND nshort_model.0 w=35 l=23
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
 ```
- We will include the model files for NMOS and PMOS from the ```libs``` directory.
 ```bash
  .include ./libs/nshort.lib
  .include ./libs/pshort.lib
 ```

- Now, we set up the connections to the nodes with ground, Vdd and input pulses.
  - VGND to VSS 0V
  - Supply voltage VPWR to GND.
  - Sweeping a pulse input.
- Now we set the transient analysis.
```bash
VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)
.tran 1n 20n
.control
run
.endc
.end
```
- Final Spice deck for simulation.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/572ba693-3394-4c69-aa60-0623757747ff)


***NGpsice Simulation and Characterization***

- Code to run the simulation
```bash
ngspice sky130_inv.spice
```

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/8b3c3414-8ed8-448b-8c93-64a2a97c3add)


- To get the plot for output against time with the sweeping input
```bash
plot y vs time a
```
![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/fc4e2ed1-4691-491b-9b2a-75cadf0757f5)

- Now we have to characterise the plot.
- There are four timing parameters used to characterize the inverter standard cell:
  - *Rise transition* - Time taken for the output to rise from 20% to 80% of max value => ```2.240 - 2.143 = 0.067ns```
  - *Fall Transition* - Time taken for the output to fall from 80% to 20% of max value => ```4.0921 - 4.049 = 0.0431ns```
  - *Cell Rise delay* - Difference in time(50% output rise) to time(50% input fall) => ```2.17333 - 2.13 = 0.0433ns```
  - *Cell Fall delay* - Difference in time(50% output fall) to time(50% input rise) => ```4.076 - 4.0501 = 0.0259ns```

### *DRC Challenges*

Under this section, we will go over 
- In-depth overview of Magic's DRC engine
- Introduction to Google/Skywater DRC rules
- *Lab* : Warm-up exercise : Fixing a simple rule error
- *Lab* : Main exercie : Fixing or create a complex error

***Introdution to Magic and Skywater PDK***

For running the DRC we need to have an understanding of the technology node we are working on. For this one can refer the following
  - Magic --> [link](http://opencircuitdesign.com/magic/)
  - Skywater PDK --> [Link](skywater-pdk.readthedocs.io/en/main) *We will keep this with use during the labs to refer the various DRC rules and Documentations.*
  - Github Repo for Skywater PDK --> [github](https://github.com/google/skywater-pdk)

***Lab Setup***
- Setup to view the layouts
- For extracting and generating views, Google/skywater repo files were built with Magic
- Technology file dependency is more for any layout. hence, this file is created first.
- Since, Pdk is still under development, there are some unfinished tech files and these are packaged for magic along with lab exercise layout and bunch of stuff into the tar ball

```bash
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```

- Once we have downloaded the archive in the home directory, we extract it to get the lab .mag files

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/d488110d-9956-4ddf-845f-e02e231de84a)

- There is a hidden file ```.magicrc``` which directs to the various resources for the lab work ahead.

***MAGIC***

- Run Magic.For better graphic use, the command belwo is used:
```bash
magic -d XR
```

- To open a file we can load the file as such -->
![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/8dab106c-ae61-4b4b-b034-11060215c712)

- Other way to load it is by defining the name while running magic.
```bash
magic -d XR <file_name>.mag
```

- We will open up met3.mag
- We see multiple independent example metal layouts with some DRC errors. We can refer these errors in the the Skywater PDK design rules which are flageed in the DRC engine.
- We can make a frame around a metal region and in command window write ```drc why``` --> this gives us the DRC violated.

![Screenshot from 2023-09-16 12-48-36](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/f5b6034a-ceb4-4f92-9817-6ff508245e41)

- Magic uses a lot of derived layers. To see these layers we can make a large box area and use following commands to see metal cut

```bash
cif see VIA2
```

### *LAB*

*Exercise 1*
- Load the poly.mag
- Check the drc violation for poly.9
- Refer the error using skywater pdk design rules
  - We find that distance between regular polysilicon & poly resistor should be 22um but it is showing 17um and still no errors . We should go to sky130A.tech file and modify as follows to detect this error.
- In line this
```bash
*******************************************************
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
*******************************************************
```
 edit as shown.
```bash
*******************************************************
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
*******************************************************
```

- Now the second edit. In line this

```bash
*******************************************************
spacing xhrpoly,uhrpoly,xpc alldiff 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
*******************************************************
```
edit as shown.
```bash
*******************************************************
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
*******************************************************
```
- After this, we ```tech load sky130.tech``` file and execute ```drc check```

![Screenshot from 2023-09-16 14-22-57](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/87605e8a-6860-4ad4-a0fe-7a51b1f7d1d1)

- We can select poly.9 and run ```drc why``` to check for errors. Now it fine.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/6f074a94-f1aa-4b0b-8cd4-ffbbeb6a1a67)

</details>

## DAY 4 - Pre-layout Timing Analysis and Importance of Good Clock Tree

<details>

<summary><strong>Timimg Modelling using Delay Models</strong></summary>

***Standard Cell LEF generation***

During Placement, entire mag information is not necessary. Only the PR boundary, I/O ports, Power and ground rails of the cell is required. This information is defined in LEF file. The main objective is to extract lef from the mag file and plug into our design flow.

***Grid into Track***
- *Track*: A path or a line on which metal layers are drawn for routing. Track is used to define the height of the standard cell.

***Guidelines for making a standard cell***
- I/O ports must lie on the intersection on Horizontal and vertical tracks.
- Width of standard cell is odd mutliples of Horizontal track pitch or X direction pitch.
- Height of standard cell is odd mutliples of Vertical track pitch or y direction pitch.

The information regarding the tracks is given in ```/home/shant/.volare/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info```
```bash
li1 X 0.23 0.46
li1 Y 0.17 0.34
met1 X 0.17 0.34
met1 Y 0.17 0.34
met2 X 0.23 0.46
met2 Y 0.23 0.46
met3 X 0.34 0.68
met3 Y 0.34 0.68
met4 X 0.46 0.92
met4 Y 0.46 0.92
met5 X 1.70 3.40
met5 Y 1.70 3.40
```

- It tells us about all the metal layers as such.
- We learnt that the input port and output for should be on the intersection of horizontal and vertical tracks, to verify this we set the grids as
  ``` bash
  grid 0.46um 0.34um 0.23um 0.17um
  ```
- Now we see the layout on Magic again.

![Screenshot from 2023-09-16 15-05-56](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/3e04a9a5-e300-4513-a046-bda40e8e4b88)

- The second condition is also verified. The X-pitch is 0.46 and we can see that the standard cell is 3 times that, thus an odd multiple. 
- The same can be verified for the height of the standard cell.

***Creation of Ports***
- Once the layout is ready, the next step is extracting LEF file for the cell.
- Certain properties and definitions need to be set to the pins of the cell. For LEF files, a cell that contains ports is written as a macro cell, and the ports are the declared as PINs of the macro.
- Our objective is to extract LEF from a given layout (here of a simple CMOS inverter) in standard format. Defining port and setting correct class and use attributes to each port is the first step.

- Method for definng ports
  - In Magic Layout window, first source the .mag file for the design (here inverter). Then Edit >> Text which opens up a dialogue box.
  - For each layer (to be turned into port), make a box on that particular layer and input a label name along with a sticky label of the layer name with which the port needs to be associated. Ensure the Port enable checkbox is checked and default checkbox is unchecked.


![Screenshot from 2023-09-16 15-35-46](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/5045cfe3-0a54-4e4c-ab17-a6f24cd9384f)

  - Port A (input port) and port Y (output port) are taken from locali (local interconnect) layer. Also, the number in the textarea near enable checkbox defines the order in which the ports will be written in LEF file (0 being the first).
  - For power and ground layers, the definition could be same or different than the signal layer. Here, ground and power connectivity are taken from metal1 (Notice the sticky label). 
 

***Port Class and Port Use Attributes***
- After defining ports, the next step is setting port class and port use attributes.

- Select port A in magic:
  ```bash
  port class input
  port use signal
  ```
- Select Y area
  ```bash
  port class output
  port use signal
  ```
- Select VPWR area
  ```bash
  port class inout
  port use power
  ```
- Select VGND area
  ```bash
  port class inout
  port use ground
  ```

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/cdf67c2a-a4c3-4757-9973-07ed07d6388f)


***Extraction of LEF file***

- Name the custom cell through tkcon window as ```sky130_shant.mag```.
- We generate lef file by command:
```bash
lef write
```
- Upon checking the directory, we can see the lef file being generated.

![Screenshot from 2023-09-16 16-07-00](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/97aa50cf-1fca-4937-a6ea-d4b0aad4ba12)

- lef file generated.
	
 ![Screenshot from 2023-09-16 16-25-14](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/80aa72e4-9706-4593-ad9f-f21685cc463a)

***Including Custom Cell ASIC Design***
- First, we transfer the lef file generated ```sky130_shant.lef``` into the ```/home/shant/OpenLane/designs/picorv32a/src``` directory.
- Then we will transfer the ```sky130_fd_sc_hd__fast.lib```, ```sky130_fd_sc_hd__slow.lib``` and ```sky130_fd_sc_hd__typical.lib``` into the same directory.

- For this, we edit the config.json file as below
```bash
{
    "DESIGN_NAME": "picorv32",
    "VERILOG_FILES": "dir::src/picorv32a.v",
    "CLOCK_PORT": "clk",
    "CLOCK_NET": "clk",
    "GLB_RESIZER_TIMING_OPTIMIZATIONS": true,
    "RUN_HEURISTIC_DIODE_INSERTION": true,
    "DIODE_ON_PORTS": "in",
    "GPL_CELL_PADDING": 2,
    "DPL_CELL_PADDING": 2,
    "CLOCK_PERIOD": 24,
    "FP_CORE_UTIL": 35,
    "PL_RANDOM_GLB_PLACEMENT": 1,
    "PL_TARGET_DENSITY": 0.5,
    "FP_SIZING": "relative",
    "LIB_SYNTH":"dir::src/sky130_fd_sc_hd__typical.lib",
    "LIB_FASTEST":"dir::src/sky130_fd_sc_hd__fast.lib",
    "LIB_SLOWEST":"dir::src/sky130_fd_sc_hd__slow.lib",
    "LIB_TYPICAL":"dir::src/sky130_fd_sc_hd__typical.lib",
    "TEST_EXTERNAL_GLOB":"dir::/src/*",
    "SYNTH_DRIVING_CELL":"sky130_vsdinv",
    "MAX_FANOUT_CONSTRAINT": 4,
    "pdk::sky130*": {
        "MAX_FANOUT_CONSTRAINT": 6,
        "scl::sky130_fd_sc_ms": {
            "FP_CORE_UTIL": 30
        }
    }
}
```

- Now, we integrate standard cell on **OpenLane** flow after ```make mount```, and follow up
  ```bash
  prep -design picorv32a -tag RUN_2023.09.11_06.05.06 -overwrite 
  set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  add_lefs -src $lefs
  run_synthesis
  ```
![Screenshot from 2023-09-16 17-53-30](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/d14f3bd4-00f9-4d8d-bfbb-45581c60f7fa)

- Synthesis log file
![Screenshot from 2023-09-16 18-12-47](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/8fc35890-e0f4-4d84-8acc-2653a204c9e0)

- Static timing analysis (STA) log file
![Screenshot from 2023-09-16 18-13-30](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/1a407d3f-d962-44a6-ad52-c4f2a0e2e355)

### *Delay Table*

Delay is a parameter that has huge impact on our cells in the design. Delay decides each and every other factor in timing. For a cell with different size, threshold voltages, delay model table is created where we can it as timing table. 

- *Delay of a cell depends on input transition and out load*. 

Lets say two scenarios, we have long wire and the cell(X1) is sitting at the end of the wire : the delay of this cell will be different because of the bad transition that caused due to the resistance and capcitances on the long wire. we have the same cell sitting at the end of the short wire: the delay of this will be different since the tarn is not that bad comapred to the earlier scenario. Eventhough both are same cells, depending upon the input tran, the delay got chaned. Same goes with o/p load also.

VLSI engineers have identified specific constraints when inserting buffers to preserve signal integrity. They've noticed that *each buffer level must maintain consistent sizing*, but *their delays can vary depending on the load they drive*. To address this, they introduced the concept of ***"delay tables"***, which essentially consist of 2D arrays containing values for input slew and load capacitance, each associated with different buffer sizes. These tables serve as timing models for the design.

When the algorithm works with these delay tables, it utilizes the provided input slew and load capacitance values to compute the corresponding delay values for the buffers. In cases where the precise delay data is not readily available, the algorithm employs a technique of interpolation to determine the closest available data points and extrapolates from them to estimate the required delay values.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/a800ffa4-5dd7-46d8-9be8-ff0869268807)

***Custom Cell inclusion in OpenLane Flow***

- We have seen till the synthesis for the custom standard cell in OpenLane flow, and verified the synthesis and STA log files. We will pick it from there now.
- First check the slack for the synthesis.

- The slack was positive, therefore we can proceed, else would have to work on the slack.
- Now we run the floorplan and placement processes.
```bash
run_floorplan
run_placement
```

![Screenshot from 2023-09-16 19-19-27](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/0a9eff93-ca66-405e-90f4-f29b91e96e46)

- Now, we check for legality &To check the layout invoke magic from the ```results/placement``` directory

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/13ec391f-d48f-4442-af06-aa85959fc795)


 
</details>

<details>

<summary><strong> Timing Analysis with Ideal Clocks using OpenSTA </strong></summary>

***Set-up Timing Analysis***

- Right now, we will consider the ideal clocks, thus the clock tree are not yet made.
- We take a single clock and anlysis launch and capture flops.
![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/b8c29f1b-0f4c-480e-b5da-7a11b4d862b2)

- In this, we assume that launch flop is triggered at the first posedge of clk and the capture flop recieves the value at the next posedge.
- Suppose there was some combinational logic between the two, the delay of the logic should be less than the time period of the clock.
- Thus the clock frequency and time period, and the combinational logic are designed with correspondence to each other.
- Therefore my setup time for the combinational logic should be less than the time period of the clock.

- Now, we will look into more real and practical conditions.
- We look into the capture flop. It is made of multiple gates and muxes, which will have there mosfets, resistances and capacitances.
- Thus will have delay associated to them.  

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/1c0aa597-e7d9-4fd7-b0ae-20ce1bc81a77)

- Suppose the flop was developed with 2 muxes as shown. We have to condsider the delays.
- This affect the combinational logic delay requirement. Now, the clock period T is not avaiable. The capture flop needs some setup time.
- Thus the time avaiable for the combinational logic now is T - setupTime of capture flop.

- *Clock Jitter* - clock is generated from PLL which has inbuilt circuit which cells and some logic. There might variations in the clock generation depending upon the ckt. These variations are collectivity known as clock uncertainity. In that jitter is one of the parameter. It is uncertain that clock might come at that exact time withought any deviation.
- That is why it is called clock_uncertainity Skew, Jitter and Margin comes into clock_uncertainity

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/7f3c072c-14b7-4f30-8748-d8c9e865d2cc)

***Post-Synthesis Analysis using OpenSTA***

Timing analysis is carried out outside the OpenLANE flow using OpenSTA tool. For this, pre_sta.conf is required to carry out the STA analysis. Invoke OpenSTA outside the openLANE flow as follows:

```bash
sta pre_sta.conf
```

Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:

- Change synthesis strategy, synthesis buffering and synthesis sizing values
- Review maximum fanout of cells and replace cells with high fanout
- sdc file for OpenSTA is modified.

base.sdc is located in ```vsdstdcelldesigns/extras``` directory. So, we copy it into our design folder using ```cp my_base.sdc /home/emil/OpenLane/designs/picorv32a/src/```

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/d2ffb39b-5aee-4ba8-9972-80952d43ceab)

From the timing report, we can improve slack by upsizing the cells i.e., by replacing the cells with high drive strength and we can see significant changes in the slack. Since there were no timing violations, we can skip this step. 

Since clock is propagated only once we do CTS, *In placement stage, clock is considered to be ideal.* So only setup slack is taken into consideration before CTS.

</details>


<details>

<summary><strong> Clock Tree Synthesis TritonCTS and Signal Integrity </strong></summary>

***Clock Tree Synthesis (CTS)***
- This plays a vital role in the creation of integrated circuits (ICs), particularly in the realm of digital electronics, where precise timing is of utmost importance. CTS involves the establishment of an organized network or structure of pathways for distributing the clock signal within the IC. This meticulous process guarantees that the clock signal effectively reaches all the sequential components, such as flip-flops and registers, in a synchronized and punctual fashion.
- It can be implemeted in various ways and the choice of the specific technique depends on the design requirements, constraints, and goals.

- Some of the different types of approches to clock tree synthesis are:
  - *Balanced Tree CTS*:

    The clock signal is spread out evenly, like branches of a tree. This helps ensure that all parts of the chip get the clock at about the same time, reducing timing problems. It's a straightforward method, but it might not save as much power as other methods.
  - *H-tree CTS*:

    It is like a tree shape with the letter "H." It's great for spreading out clock signals across big chips. This tree structure helps make sure the timing is good and saves power, especially in large areas of the chip.
  - *Star CTS*:

    In a star CTS, the clock signal is distributed from a single central point (like a star) to all the flip-flops. This approach simplifies clock distribution and minimizes clock skew but may require a higher number of buffers near the source.
  - *Mesh CTS*:

    In a mesh CTS, clock wires are arranged in a mesh-like grid pattern, and each flip-flop is connected to the nearest available clock wire. It is often used in highly regular and structured designs, such as memory arrays. Mesh CTS can offer a balance between simplicity and skew minimization.
  - *Adaptive CTS*:

    Adaptive CTS techniques adjust the clock tree structure dynamically based on the timing and congestion constraints of the design. This approach allows for greater flexibility and adaptability in meeting design goals but may be more complex to implement.


***Crosstalk in VLSI***
- Crosstalk in VLSI refers to unwanted interference or coupling between adjacent conductive traces or wires on an integrated circuit (IC) or chip.
- It occurs when the electrical signals on one wire influence or disrupt the signals on neighboring wires.Uncontrolled crosstalk can lead to data corruption, timing violations, and increased power consumption.
- *Mitigation*: VLSI designers employ various techniques to mitigate crosstalk, such as optimizing layout and routing, using appropriate shielding, implementing proper clock distribution strategies, and utilizing clock gating to reduce dynamic power consumption when logic is idle

***Clock net sheilding in VLSI***
- Clock net shielding in VLSI refers to a technique used to protect the clock signal from interference or crosstalk. The clock signal is critical for synchronizing the operations of various components on a chip, and any interference can lead to timing issues and performance problems.
- VLSI designers may use shielding techniques to isolate the clock network from other signals, reducing the risk of interference. This can include dedicated clock routing layers, clock tree synthesis algorithms, and buffer insertion to manage clock distribution more effectively.
- VLSI designs often have multiple clock domains. Shielding and proper clock gating help ensure that clock signals do not propagate between domains, avoiding metastability issues and maintaining synchronization.

*Note* - *In this stage clock is propagated and make sure that clock reaches each and every clock pin from clock source with mininimum skew and insertion delay. Inorder to do this, we implement H-tree using mid point strategy. For balancing the skews, we use clock invteres or bufferes in the clock path. Before attempting to run CTS in TritonCTS tool, if the slack was attempted to be reduced in previous run, the netlist may have gotten modified by cell replacement techniques. Therefore, the verilog file needs to be modified using the ```write_verilog``` command. Then, the synthesis, floorplan and placement is run again.*

***LAB Continued***

We will continue after the synthesis, floorplan and placement. We run the CTS as 

```bash
run_cts
```

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/12044892-426c-4c30-ba0f-405e2a7ec5a7)

- Now, we verify the setup and hold time

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/3adae859-7f27-4c68-a170-2b60bfce8b82)

- We note the setup slack as ``` 13.31 ``` and hold slack as ``` 0.35 ```. Therefore, we are not in violation for timing constrints. 
 
</details>


<details>

<summary><strong> Timing Analysis with Real Clocks using OpenSTA </strong></summary>

***Setup Timing Analysis using Real Clocks***
- Analyzing setup time is a crucial element of designing digital circuits, especially in synchronous digital systems.
- It pertains to the duration during which a signal must remain steady and valid prior to the arrival of the clock edge.
- Guaranteeing the fulfillment of setup time prerequisites is vital for averting data errors and securing the correct functioning of the digital circuit.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/773db2c6-a1c5-4d7e-a92f-a2a43602c6bc)


- To ensure the setup time requirements are met we need to make sure of some things:
  - Selecting proper Filp flops or latches.
  - Optimize combinational logic
  - Clock Skew Analysis
  - Timing constraints

- Meeting setup time requrirements is cruical for a good digital circuit operation. If not done can result in data errors and multifunctioning of the circuit.

***Holding Timing Analysis using Real Clock***
- Analysis of hold time is an equally vital component of digital circuit design, especially in synchronous systems.
- It concerns the minimum duration during which a data input (D) needs to maintain its stability and validity after the clock edge before any changes can occur.
- Ensuring that hold time requirements are met is essential to prevent data corruption and ensure the proper operation of digital circuits.

![image](https://github.com/Shant1R/Advanced-Physical-Design-using-Openlane/assets/59409568/7a6c57f6-5307-4aea-9324-626f6c665268)

***LAB Continued***


 
</details>







## DAY 5 - Final Step for RTL2GDS using tritonRoute and OpenSTA

<details>

<summary><strong> Routing and Design Rule Check </strong></summary>
 
</details>


<details>

<summary><strong> Power Distribution Network and Routing </strong></summary>
 
</details>


<details>

<summary><strong> TritonRoute Feature </strong></summary>
 
</details>


