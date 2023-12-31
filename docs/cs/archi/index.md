# Computer Architecture 
## Computer Basics
### Number System
There are mainly three number systems: decimal, binary, and hexadecimal.

Decimal (base 10) is used by people, binary (base 2) is used by computers, where 0 represents OFF state (no volatge) and 1 represents ON state (have voltage), hexadecimal (base 16) is a concise representation of binary numbers. 

Some examples: $3525_{10}$, $100111_{2}$, FA289B$_{16}$. The convertions between them are already taught in high school.

### Units
There are several units e.g.: kilo, mega, giga, tera, peta. But notice that they mean different things in different scenarios. When dealing with size such as **memory or file**, 1 kilo = 2^10, 1 mega = 2^20. When dealing with **rate/frequency** such as # instructions per sec, # clock ticks per sec, network speed,  1 kilo = 10^3, 1 mega = 10^6.

### Class of Computers
- Personal Computers (PC)
    - General Purpose, variety of software
    - Subject to cost / performance tradeoff
- Server Computers  
    - Network based
    - High capacity, performance, **reliability**
    - Range from small servers to building sized
- Supercomputers
    - High-end scientific and engineering calculations
    - Highest capability but represent a small fraction of the overall computer market 
- Embedded Computers
    - Hidden as components of systems
    - Stringest power / performance / cost constraints

### Levels of Abstractions
Hardware => System Software (e.g.: Windows, Linux) => Application Software

Both hardware and software are organized into hierarchical layers, in which lower-level details are hidden to offer a simpler view at the higher levels.

There are levels of program code to cope with these layers and their interactions as well, namely binary machine language program, assembly language (e.g.: MIPS), and high-level language (e.g.: C/C++). 

The abstract interface between hardware and software is called `Instruction Set Architecture` (ISA) (e.g. Intel x86, ARM, MIPS). It can allow different implementations of varying cost and performance to follow the same instruction test architecture. Example: 80x86, Pentium, Pentium II, Pentium III, Pentium 4 all implement
the same ISA.

### Components of Computer 
- Input
    - To communicate with the computer
    - Data and instructions transferred to the memory
- Output
    - To communicate with the user
    - Data is read from the memory
- Memory
    - Large store to keep instructions and data
- Processor
    - Datapath: processes data according to instructions
    - Control: commands the operations of input, output, memory, and datapath according to the instructions

### Technology Trend
Vacuum Tubes (1950s) => Transistors (1950s and 1960s) => Integrated Circuits (1960s and 70s) => Very Large Scale Integrated (VLSI) Circuit (1980s and on)

Cost / performance is improving due to underlying technology development.

Moore's law: the number of transistors on microclips doubles every two years.

This makes novel applications possible such as AI, World Wide Web, search machines ...