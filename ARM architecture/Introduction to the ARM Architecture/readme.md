# ARM Architecture Overview

## 1. Introduction to ARM Architecture
ARM architecture is widely used due to its simplicity, efficiency in performance, and low power consumption. Key attributes include:

- **Reduced Instruction Set Computer (RISC) Features:**
  - Large uniform register file.
  - Load/store architecture.
  - Simple addressing modes.
  - Uniform and fixed-length instruction fields.

- **ARM-Specific Enhancements:**
  - Control over ALU and shifter.
  - Auto-increment and auto-decrement addressing modes.
  - Load and Store Multiple instructions.
  - Conditional execution of almost all instructions.

- **Balance in Design:**
  - High performance.
  - Small code size.
  - Low power consumption.
  - Small silicon area.

### A1.1.1 ARM Registers
- **General-Purpose Registers:**
  - 31 general-purpose 32-bit registers.
  - 16 visible at any time.

- **Special Registers:**
  - Stack Pointer (R13).
  - Link Register (R14).
  - Program Counter (R15).

- **Processor States:**
  - Thumb state.
  - Jazelle state.

### A1.1.2 Exceptions
- Seven types of exceptions.
- Different privileged processing modes for each exception.
- Specific banked registers for fast interrupt processing.

### A1.1.3 Status Registers
- **Current Program Status Register (CPSR):**
  - Contains condition code flags, interrupt disable bits, processor mode, and state.

- **Saved Program Status Register (SPSR):**
  - Holds the CPSR of the task before the exception occurred.

## 2. ARM Instruction Set
### A1.2.1 Branch Instructions
- Standard Branch instruction with a 24-bit signed word offset.
- Branch and Link (BL) instructions for subroutine calls.

### A1.2.2 Data-Processing Instructions
- Arithmetic/logic instructions.
- Comparison instructions.
- Single Instruction Multiple Data (SIMD) instructions.
- Multiply instructions.
- Miscellaneous Data Processing instructions.

### A1.2.3 Status Register Transfer Instructions
- Transfer contents of CPSR or an SPSR to/from a general-purpose register.

### A1.2.4 Load and Store Instructions
- Different types of load and store instructions, including Load and Store Register, Load and Store Multiple registers, and Load and Store Register Exclusive.

### A1.2.5 Coprocessor Instructions
- Data-processing, data transfer, and register transfer instructions for coprocessor interaction.

### A1.2.6 Exception-Generating Instructions
- Software interrupt instructions (SWI) and Software breakpoint instructions (BKPT).

## 3. Thumb Instruction Set
- A 16-bit subset of the ARM instruction set for more compact code.
