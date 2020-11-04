# CPE 1040 - Fall 2020

This is learning progression 004 for the Fall 2020 installment of the course CPE 1040: Introduction to Computer Engineering at MSU Denver.

Table of Contents
=================

* [CPE 1040 \- Fall 2020](#cpe-1040---fall-2020)
  * [Learning Progression 004: External LEDs](#learning-progression-004-external-leds)
    * [Step 4: Minimal assembly (part 1)](#step-4-minimal-assembly-part-1)
      * [1\. Study](#1-study)
        * [Central processing unit](#central-processing-unit)
        * [Instruction set architecture](#instruction-set-architecture)
        * [Registers](#registers)
        * [Processor addressing modes](#processor-addressing-modes)
        * [Load and store](#load-and-store)
        * [Branching revisited](#branching-revisited)
        * [Status bits](#status-bits)
        * [Clock cycles](#clock-cycles)
        * [Minimal instruction set CPU](#minimal-instruction-set-cpu)
          * [F\-4 MISC 16 bit instruction set](#f-4-misc-16-bit-instruction-set)
          * [In plain words](#in-plain-words)
          * [Symbols](#symbols)
        * [micro:bit hex files](#microbit-hex-files)
      * [2\. Apply](#2-apply)
      * [3\. Present](#3-present)

## Learning Progression 004: External LEDs

This progression introduces fundamentals of computing, including the binary system of data representation as well as the basics of memory and processing. We introduce assembly language in the context of a minimal instruction set processor. This is where the lowest layer of the software stack and the highest layer of the hardware stack coexist, and where user programs are translated into machine code and executed by the processor one instruction at a time. This is also the level of computing which directly correpsonds to the simplest theoretical models of a computer. We also introduce the input-output capabilities of the micro:bit and build an external circuit to serve as an extension to the built-in 5x5 LED matrix to run our Screensavers program on.

### Step 4: Minimal assembly (part 1)  
[[toc](#table-of-contents)]

#### 1. Study
[[toc](#table-of-contents)]

##### Central processing unit
[[toc](#table-of-contents)]

The actual processor component which performs the operations of the instruction set.  

##### Instruction set architecture
[[toc](#table-of-contents)]

The most basic _operations_ the processor executes. Note that operations are a very high abstraction level relative to electric signals and bit-states, which are part of the physical hardware of the computing device, and thus, the hardware stack. At the same time, operations are the _lowest abstraction level_ in the software stack. This is where the hardware and software stack intersect.

##### Registers
[[toc](#table-of-contents)]

Small number of very fast memory locations, deep inside the processor, used in the execution of the processor's instructions. They hold instructions, operands, results, and control state.


##### Processor addressing modes  
[[toc](#table-of-contents)]

List and illustrate with sketches.  

##### Load and store
[[toc](#table-of-contents)]

Link to memory.  

##### Branching revisited
[[toc](#table-of-contents)]

Moving through the address space.  

##### Status bits
[[toc](#table-of-contents)]

Program state. List with examples.  

##### Clock cycles
[[toc](#table-of-contents)]

Synchronous circuits. Motivate the clock!   

##### Minimal instruction set CPU
[[toc](#table-of-contents)]

[F-4 MISC](http://www.dakeng.com/misc.html)  
     
The instruction set for the F-4 (fast 4), a proof-of-concept minimal instruction set CPU, is listed below.  Each instruction has several addressing modes. Note that the mnemonics are borrowed from the venerable Motorola SY6502, used in the old 8-bit Nintendo, among other machines.  There is only one "A" register or accumulator, and a program counter.

###### F-4 MISC 16 bit instruction set
[[toc](#table-of-contents)]

Instruction | Opcode | Operand | Operation | Clocks
--- | --- | --- | --- | ---
ADDi imm | 00 01 | 16 bit value | imm+(A) --> A | 3
ADDm addr | 00 02 | 16 bit address | (addr)+(A) --> A | 4
ADDpc | 00 04 | null operand | PC+(A) --> A | 3
BVS addr | 00 08 | 16 bit address | (addr) --> PC if \<v>=1 | 3
LDAi imm | 00 10 | 16 bit value | imm --> A | 3
LDAm addr | 00 20 | 16 bit address | (addr) --> A | 3
LDApc | 00 40 | null operand | PC --> A | 3
STAm addr | 00 80 | 16 bit address | A --> (addr) | 3
STApc | 01 00 | null operand | A --> PC | 3

###### In plain words  
[[toc](#table-of-contents)]

Instruction | Verbal elaboration | Sketch
--- | --- | ---
ADDi imm | Add an immediate value _imm_ and the value at address in register A, and write the result into register A  | [ADDi](images/misc-0001-addi.png)
ADDm addr | Add the value at an address _addr_ and the value at address in register A, and write the result into register A  | [ADDm](images/misc-0002-addm.png)
ADDpc | Add the value of the program counter PC and the value at address in register A, and write the result into register A  | [ADDpc](images/misc-0004-addpc.png)
BVS addr | If the overflow bit \<v> is set (is equal to 1), write the value at an address _addr_ into the program counter PC  | [BVS](images/misc-0008-bvs.png)
LDAi imm | Load an immediate value _imm_ into register A   | [LDAi](images/misc-0010-ldai.png)
LDAm addr | Load the value at an address _addr_ into register A  | [LDAm](images/misc-0020-ldam.png)
LDApc | Load the value of the program counter PC into register A  | [LDApc](images/misc-0040-ldapc.png)
STAm addr | Store the value of register A at address _addr_  | [STAm](images/misc-0080-stam.png)
STApc | Store the value of register A in the program counter PC  | [STApc](images/misc-0100-stapc.png)

###### Symbols  
[[toc](#table-of-contents)]

Symbol | Interpretation
--- | ---
imm | A numeric literal (e.g. 6<sub>10</sub>)
A | The value in register A 
(A) | Value in memory location at address stored in register A
(addr) | Value in memory location at address _addr_
\<v> | A single bit which is set to 1 when the result of an instruction overflows
PC | Program counter holding the address of the next instruction to be executed

The four instructions can be summarized as Load, Store, Add, and Branch if Overflow Set.  Each memory operation is assumed to take one clock, and ALU operations take one clock also.  ALU (Arithmetic and Logic Unit) is something of a misnomer here, since this chip can only add.  For example, "ADD addr" takes four clocks, two to fetch the instruction and operand, one to read the memory, and one more to do the addition.

I estimate that about 1400 transistors would be needed to build this for a 16 bit implementation, which gives a 128k (2<sup>16</sup> words) space for programs and data.  With so few transistors, a very conservative performance estimate would be on the order of 50 MIPS if the memory is onboard the chip.

##### micro:bit hex files
[[toc](#table-of-contents)]

**TODO: Research & take apart a file and show contents (runtimes, program, etc.) _Where?_**  

#### 2. Apply
[[toc](#table-of-contents)]

1. `[<lernact-prac>]`Decoding instruction binary patterns. Reading program lines in binary.  
2. `[<lernact-prac>]`Counting clock cycles for a program.    
3. `[<lernact-prac>]`**[Optional challenge, max 5 extra step points]** Trace a program in F-4 MISC assembly.  
4. `[<lernact-prac>]`**[Optional challenge, max 10 extra step points]** Write a function in F-4 MISC assembly.  

#### 3. Present
[[toc](#table-of-contents)]

