## A1.1 About the ARM architecture

### Key Characteristics
- **Reduced Instruction Set Computer (RISC):** ARM is a RISC architecture, which includes:
  - Large uniform register file.
  - Load/store architecture. where data-processing operations only operate on register contents, not directly on memory contents
  - Simple addressing modes.
  - Uniform and fixed-length instruction fields, to simplify instruction decode.(to facilitate pipeline)

### Enhancements
- Control over the Arithmetic Logic Unit (ALU) and shifter.
- Auto-increment and auto-decrement addressing modes.
- Load and Store Multiple instructions.
- Conditional execution of almost all instructions.
These enhancements to a basic RISC architecture allow ARM processors to achieve a good balance of high
performance, small code size, low power consumption, and small silicon area.

## A1.1.1 ARM Registers
- **General-Purpose Registers:** 31 general-purpose 32-bit registers, 16 visible at any time. These 16 registerse are User mode registers.
- **Special Roles:**
  - **Stack Pointer (R13):** Used for stack operations.
  - **Link Register (R14):** Holds return address after a subroutine call. This is more efficient than using a call stack for every function call, which INTEL uses.
  - **Program Counter (PC, R15):** Points to next's next instruction of the current instruction being executed.

## A1.1.2 Exceptions
- **Types of Exceptions:**
  - reset
  - attempted execution of an Undefined instruction
  - software interrupt (SWI) instructions, can be used to make a call to an operating system
  - Prefetch Abort, an instruction fetch memory abort
  - Data Abort, a data access memory abort
  - IRQ, Interrupt request
  - FIQ, Fast interrupt request

- **Exception Modes and Banked Registers**:
  - Standard Registers and Exceptions: When an exception occurs, standard registers are replaced with registers specific to the exception mode.
  - Banked Registers for All Exception Modes: All exception modes have replacement banked registers for R13 (stack pointer) and R14 (link register).
- **Specifics of Fast Interrupt Mode**:
  - Additional Banked Registers: Fast interrupt mode has banked registers for R8 to R12, in addition to R13 and R14.
  - Purpose: These additional banked registers allow fast interrupt processing to begin without the need to save or restore these registers.
- **Behavior of Registers during Exceptions**:
  - R14 (Link Register): When entering an exception handler, R14 holds the return address for exception processing. It is used to return after processing the exception and to address the instruction that caused the exception.
  - R13 (Stack Pointer): Register 13 is banked across different exception modes, providing each exception handler with a private stack pointer.
#### `Exception process`:
When an exception occurs, the ARM processor halts execution in a defined manner and begins execution at
one of a number of fixed addresses in memory, known as the exception vectors. There is a separate vector location for each exception, including reset.<br>
An operating system installs a handler on every exception at initialization. Privileged tasks are normally run in System mode to allow exceptions to occur without state loss. This is because System mode uses the same registers as User mode, rather than a separate set of privileged registers.

## A1.1.3 Status Registers
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/a8d11d29-e894-4dab-aa30-fa40bdaacdd0)<br>
All processor state other than the general-purpose register contents is held in status registers.
- **Current Program Status Register (CPSR):** Holds processor status, condition code flags, interrupt disable bits, processor mode, etc.
- **Saved Program Status Register (SPSR):** Holds the CPSR of the task before an exception occurred. (Each exception mode has SPSR) 

## A1.2 ARM Instruction Set

### Classification
1. **Branch Instructions**
2. **Data-Processing Instructions**
3. **Status Register Transfer Instructions**
4. **Load and Store Instructions**
5. **Coprocessor Instructions**
6. **Exception-Generating Instructions**

### Features
- Conditional execution based on condition code flags.
- Most data-processing instructions can update the CPSR flags.

## A1.2.1 Branch Instructions
Used for control flow alteration by writing the PC(Program Counter)
- **Standard Branch Instruction:** Uses a 24-bit signed word offset for forward and backward branches.
- **Branch and Link (BL):** Preserves return address in the Link Register.
- **Instruction Set Switching:** Allows switching between ARM and Thumb instruction sets.

## A1.2.2 Data-Processing Instructions
- **Arithmetic/Logic Instructions:** Perform operations on two source operands and write the result.
- **Comparison Instructions:** Perform operations without writing the result, just updating condition flags.
- **SIMD Instructions:** Treat operands as parallel 16-bit or 8-bit numbers (ARMv6).
- **Multiply Instructions:** Various classes for different operations.

## A1.2.3 Status Register Transfer Instructions
- **Function:** Transfer contents between CPSR/SPSR and general-purpose registers.

## A1.2.4 Load and Store Instructions
Move data between memory and registers.
- **Types:** Regular, Multiple registers, Exclusive.
- **Addressing Modes:** Offset, Pre-indexed, Post-indexed.
- **Support for Unaligned Accesses:** Introduced in ARMv6.

## A1.2.5 Coprocessor Instructions
- **Types:** Data-processing, Data transfer, Register transfer.

## A1.2.6 Exception-Generating Instructions
- **Software Interrupt (SWI):** Used for OS-defined services(software interrupt).
- **Software Breakpoint (BKPT):** Causes an abort exception for debugging.

## A1.3 Thumb Instruction Set
A subset of ARM instruction set with 16-bit encoding for instructions.
It offers code density improvements.(Code density refers to the amount of instructions that can be packed into a given amount of memory space.)
