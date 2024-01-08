# Introduction to the ARM Architecture

## Overview
The ARM architecture is a dominant force in various market segments due to its simplicity, efficiency, and low power consumption. It's a Reduced Instruction Set Computer (RISC) architecture, known for its:
- Large uniform register file.
- Load/store architecture.
- Simple addressing modes.
- Uniform and fixed-length instruction fields.

## Key Features
- **Performance**: Supports a wide spectrum of performance points.
- **Low Power Consumption**: Ideal for devices requiring low energy use.
- **Implementation Size**: Small, leading to less power consumption.
- **Balance**: Offers a good balance of performance, code size, power consumption, and silicon area.

## ARM Architecture Characteristics
- **RISC Core**: Reduced Instruction Set Computing principles.
- **Enhancements**: Includes ALU and shifter control, auto-increment/decrement addressing, Load and Store Multiple instructions, and conditional execution.

## ARM Registers (A1.1.1)
- **Total**: 31 general-purpose 32-bit registers.
- **Visible Registers**: 16 at any time, with others aiding in exception processing.
- **User Mode**: Unprivileged, with limited access compared to privileged modes.
- **Special Registers**:
  - R13: Stack Pointer (SP).
  - R14: Link Register (LR).(For a single call, the return address is stored in the LR. This is more efficient than using a call stack for every function call)(Intel CPUs don't have)
  - R15: Program Counter (PC).

## Exceptions (A1.1.2)
- **Types**: Seven types, each with a dedicated privileged mode.
  - reset
  - attempted execution of an Undefined instruction
  - software interrupt (SWI) instructions, can be used to make a call to an operating system
  - Prefetch Abort, an instruction fetch memory abort
  - Data Abort, a data access memory abort
  - IRQ, normal interrupt
  - FIQ, fast interrupt.
- **Banked Registers**: Specific registers for each exception mode.
#### `Exception process`:
When an exception occurs, the ARM processor halts execution in a defined manner and begins execution at
one of a number of fixed addresses in memory, known as the exception vectors. There is a separate vector location for each exception, including reset.<br>
An operating system installs a handler on every exception at initialization. Privileged tasks are normally run in System mode to allow exceptions to occur without state loss as the same registers are used. This is because System mode uses the same registers as User mode, rather than a separate set of privileged registers.

## Status Registers (A1.1.3)
- **CPSR**: Current Program Status Register, holding processor status and condition flags.
- **SPSR**: Saved Program Status Register, for storing CPSR of the task before the exception.

## ARM Instruction Set (A1.2)
- **Conditional Execution**: Almost all instructions can execute based on condition codes.
Broadly categorized into:
- **Branch Instructions**: For control flow alteration by writing the PC(Program Counter)
- **Data-processing Instructions**: Performs calculations on the general-purpose registers
  - Arithmetic/logic instructions
  - Comparison instructions
  - Single Instruction Multiple Data (SIMD) instructions
  - Multiply instructions on page A1-8
  - Miscellaneous Data Processing instructions on page A1-8
- **Status Register Transfer Instructions**: Transfer between CPSR/SPSR and general-purpose registers.
- **Load and Store Instructions**: Data movement from memory into a register.
- **Coprocessor Instructions**: Data processing, transfer, and register operations specific to coprocessors.
- **Exception-generating Instructions**: Software interrupt instructions to trigger software interrupts, Software breakpoint instructions to trigger an abort exception.

## Thumb Instruction Set (A1.3)
- A 16-bit subset of the 32-bit ARM instruction set, offering code density improvements.(Code density refers to the amount of instructions that can be packed into a given amount of memory space.)
