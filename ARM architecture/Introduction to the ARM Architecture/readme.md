## Detailed ARM Architecture Study Notes

### 1. ARM Architecture Fundamentals
- **Type:** Reduced Instruction Set Computer (RISC).
- **Key Attributes:** Small implementation size, high performance, low power consumption.
- **Distinctive Features:**
  - Uniform register file.
  - Load/store architecture: Data-processing operations only on register contents.
  - Simple addressing modes: Load/store addresses from register contents and instruction fields.
  - Uniform, fixed-length instruction fields for easy decode.

### 2. ARM Registers
- **General Overview:** 31 general-purpose 32-bit registers; 16 visible at any time.
- **User Mode Registers:** Used by unprivileged code.
- **Special Registers:**
  - R13: Stack Pointer (SP).
  - R14: Link Register (LR) - holds return address for subroutines.
  - R15: Program Counter (PC) - points to the instruction two ahead of the current instruction.
- **Additional States:** Supports Thumb® and Jazelle® states.
- **System Mode:** For privileged tasks, with access to full functionality.

### 3. ARM Instruction Set
- **Instruction Classes:**
  - **Branch Instructions:** Standard Branch with 24-bit signed word offset; Branch and Link (BL) to store return address in LR.
  - **Data-Processing Instructions:** Arithmetic/logic operations, comparison, SIMD operations, multiply, and miscellaneous.
  - **Status Register Transfer Instructions:** Transfer between CPSR/SPSR and general-purpose registers.
  - **Load/Store Instructions:** Various sizes and addressing modes, including Load/Store Register and Multiple Register.
  - **Coprocessor Instructions:** For internal operations, data transfers, and register transfers.
  - **Exception-Generating Instructions:** SWI for software interrupts and BKPT for breakpoints.

### 4. Data-Processing Instructions
- **Arithmetic/Logic Instructions:** Two source operands, optional condition code flags update.
- **Comparison Instructions:** Similar to arithmetic/logic but without writing results to registers.
- **SIMD Instructions:** Parallel processing of 16-bit or 8-bit numbers, available in ARMv6.
- **Multiply Instructions:** Several classes for different multiplication operations.

### 5. Load and Store Instructions
- **Load and Store Register:** For 64-bit, 32-bit, 16-bit, and 8-bit data; supports unaligned loads/stores from ARMv6.
- **Addressing Modes:** Offset, pre-indexed, and post-indexed.
- **Load and Store Multiple Registers:** Efficient for subroutine entry/exit and block copies.
- **Exclusive Instructions:** For cooperative memory synchronization.

### 6. Exceptions and Status Registers
- **Exception Types:** Reset, Undefined instruction, SWI, Prefetch Abort, Data Abort, IRQ, FIQ.
- **Status Registers:** 
  - **CPSR:** Holds current operating status, condition code flags, interrupt disable bits, processor mode/state.
  - **SPSR:** Saves CPSR status before an exception.

### 7. Thumb Instruction Set
- **Characteristics:** 16-bit encoding, a subset of the ARM instruction set.
- **Benefits:** More efficient execution and smaller code size, suitable for smaller systems.
