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

Under this section, we will look into the requirements
  
</details>


## DAY 2
## DAY 3
## DAY 4
## DAY 5
