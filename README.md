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
## DAY 4 - Pre-layout Timing Analysis and Importance of Good Clock Tree
## DAY 5 - Final Step for RTL2GDS using tritonRoute and OpenSTA
