# Programmers' Model

## Data Types
- **Byte (8 bits):** Smallest data type.
- **Halfword (16 bits):** Introduced in ARM version 4.
- **Word (32 bits):** Main data type for operations.
- **Support for Unaligned Data:** ARMv6 introduced unaligned data support for words and halfwords.
- **Signed and Unsigned Types:** Represent integers using normal binary format or two's complement format.

## Processor Modes
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/f5629faf-1c3b-4986-9b55-c75fdfc76cc1)<br>
- **Mode changes**: Can be made under software control, or can be caused by external interrupts or exceptions.<br>
- **User mode**: Most application programs run in User mode, which cannot access protected system resources or to change mode, other than triggering an exception.<br>
- **Privileged modes**: The modes other than User mode are known as privileged modes. They have full access to system resources and can change mode freely.<br>
Five of them are known as exception modes: FIQ, IRQ, Supervisor, Abort, Undefined. These are entered when specific exceptions occur. Each of them has some additional registers to avoid
corrupting User mode state when the exception occurs.
- **System mode**: System mode is one of the privileged modes, and it shares the same register set as the User mode. It has full access to the system's privileged resources.

## Registers
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/c4274b11-1a5f-4e1d-bbe9-49a0cfdbf5b2)<br>
- Total of 37 registers(Sum of the registers in the picture above is 37)
  - 31 general-purpose 32-bit registers, including a program counter. R0 ~ R15 visible at any time. (The PC is no longer treated as a general-purpose register in ARMv8 or later)
  - 6 status registers
### General-purpose registers
- **Unbanked Registers (R0 to R7 and R15)**: Each of them refers to the same 32-bit physical register in all processor modes. They are completely general-purpose registers, with no special uses implied by the architecture
- **Banked Registers (R8 to R14)**: Different physical registers depending on the processor mode. A specific name is used to point to a particular physical register.
  - Almost all instructions allow the banked registers to be used wherever a general-purpose register is allowed.
  - Some of the banked registers are unique to the mode, while others are shared (or overlapped) with other modes:<br>
    - Registers R8 to R12 have two banked physical registers each. One is used in all processor modes other than FIQ mode, and the other is used in FIQ mode.<br>
    - Registers R13 and R14 have six banked physical registers each. One is used in User and System modes, and each of the remaining five is used in one of the five exception modes. 
### Special Uses of these registers
- R13 (Stack Pointer): Used for stack operations.
- R14 (Link Register): Holds return address after a subroutine call. This is more efficient than using a call stack for every function call, which INTEL uses.
- R15 (Program Counter): R15 can be used in place of other general-purpose registers for certain special-case effects.<br>
By default, R15 operates as a program counter, used for reading or writing the address of the next's next instruction.<br>
This is due to the pipeline architecture of ARM processors, where instructions are pre-fetched.<br>

## Program Status Registers
The Current Program Status Register (CPSR) is accessible in all processor modes. It contains condition code flags, interrupt disable bits, the current processor mode, and other status and control information.<br>
Each exception mode also has a Saved Program Status Register (SPSR), that is used to preserve the value of the CPSR when the associated exception occurs.<br>
### Types of PSR bits
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/62c15732-41b0-4431-8a3c-22fc6feb9cb8)<br>
- **Reserved bits**: Reserved for future expansion. Implementations must read these bits as 0 and ignore writes to them.
- **User-writable bits(N, Z, C, V, Q, GE[3:0], E)**: Can be written from any mode.
- **Privileged bits(A, I, F, and M[4:0])**: Can be written from any privileged mode. Writes to privileged bits in User mode are ignored.
- **Execution state bits(J, T)**: Can be written from any privileged mode. Writes to execution state bits in User mode are ignored. They are always zero in ARM state.
### The condition code flags
The N, Z, C, and V (Negative, Zero, Carry and oVerflow) bits are collectively known as the condition code flags, often referred to as flags.<br>
The condition code flags in the CPSR can be tested by most instructions to determine whether the instruction is to be executed.(if else)<br>
#### The condition flags are usually modified by:
- Execution of a comparison instruction (CMN, CMP, TEQ or TST).
- Execution of some other arithmetic, logical or move instruction.
#### The flags can be modified in these additional ways:
- Execution of an MSR instruction.
- Execution of MRC instructions with destination register R15.
- Execution of some variants of the LDM instruction.
- Execution of an RFE instruction in a privileged mode that loads a new value into the CPSR from
memory.
- Execution of flag-setting variants of arithmetic and logical instructions whose destination register is
R15. These also copy the SPSR to the CPSR, and are intended for returning from exceptions.
### The Q flag
bit[27] of the CPSR is known as the Q flag and is used to indicate whether overflow and/or saturation has occurred in some DSP-oriented instructions.<br>
### The GE[3:0] bits
The SIMD instructions use bits[19:16] as Greater than or Equal (GE) flags for individual bytes or halfwords of the result.<br>
You can use these flags to control a later SEL instruction<br>
### The E bit
Controls load and store endianness for data handling.<br>
### The interrupt disable bits
- **A bit**: Disables imprecise data aborts when it is set. This is available only in ARMv6 and above. In earlier versions, bit[8] of CPSR and SPSRs must be treated as a reserved bit, as described in Types of PSR bits on page A2-11.
I bit Disables IRQ interrupts when it is set.
F bit Disables FIQ interrupts when it is set.
### The mode bits
M[4:0] are the mode bits. These determine the mode in which the processor operates.<br>
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/c8ce20f3-44b9-4d2f-8eb8-646f2ba047a4)<br>
### The T and J bits
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/c46e77e9-2693-4351-b576-62c73d8edc3a)<br>
The T and J bits select the current instruction set, as shown in Table A2-3.
### Other bits
Other bits in the program status registers are reserved for future expansion.<br>
In general, programmers must take care to write code in such a way that these bits are never modified. Failure to do this might result in code that has unexpected side effects on future versions of the architecture.

## Exceptions
Exceptions are generated by internal and external sources to cause the processor to handle an event, such as an externally generated interrupt.<br>
The processor state just before handling the exception is normally preserved so that the original program can be resumed when the exception routine has completed.<br>
More than one exception can arise at the same time.<br>
All exception modes have replacement banked registers. When an exception occurs, standard registers are replaced with registers specific to the exception mode.<br>
### Types of exceptions
The ARM architecture supports seven types of exception. Table A2-4 lists the types of exception and the processor mode that is used to process each type.<br>
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/8f983210-851f-4d21-abf1-833acaa9c486)<br>
#### Reset
When the Reset input is asserted on the processor, the ARM processor immediately stops execution of the current instruction.
#### Undefined Instruction exception
If the ARM processor executes a coprocessor instruction, it waits for any external coprocessor to acknowledge that it can execute the instruction. If no coprocessor responds, an Undefined Instruction exception occurs.
#### Software Interrupt exception
The Software Interrupt instruction (SWI) enters Supervisor mode to request a particular supervisor (operating system) function.
#### Prefetch Abort (instruction fetch memory abort)
A memory abort is signaled by the memory system. Activating an abort in response to an instruction fetch marks the fetched instruction as invalid.<br>
A Prefetch Abort exception is generated if the processor tries to execute the invalid instruction.<br>
If the instruction is not executed (for example, as a result of a branch being taken while it is in the pipeline), no Prefetch Abort occurs.
#### Data Abort (data access memory abort)
A memory abort is signaled by the memory system. Activating an abort in response to a data access (load or store) marks the data as invalid. A Data Abort exception occurs before any following instructions or exceptions have altered the state of the CPU.<br>
- `Effects of data-aborted instructions`
If a Data Abort occurs in a data access instruction, the value of each memory location that the instruction stores to is:
  - unchanged if the memory system does not permit write access to the memory location
  - UNPREDICTABLE otherwise.
#### Interrupt request (IRQ) exception
The IRQ exception is generated externally by asserting the IRQ input on the processor. It has a lower priority than FIQ
#### Fast interrupt request (FIQ) exception
The FIQ exception is generated externally by asserting the FIQ input on the processor.<br>
FIQ is designed to support a data transfer or channel process.<br>
- `Why fast interrupt is faster`
  - **Dedicated Registers:**: FIQ has additional banked registers(private) to remove the need for register saving in such applications, therefore minimizing the overhead of context switching.
  - **Shorter vector**: There are fewer instructions to process before the ISR code is reached.
  - **Higher Priority**: FIQ has a higher priority than IRQ 
### Exception priorities
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/8a47a1ec-e28e-4cf5-b4a9-087883a2e507)
### Exception level
(not supported before ARMv7)<br>
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/7f28fbca-f3d2-4a1e-a5e2-50d572e5f3df)
### Exception process
When an exception occurs, execution is forced from a fixed memory address corresponding to the type of exception. These fixed addresses are called the **exception vectors**. There is a separate vector location for each exception, including reset.<br>
And the banked versions of R14 and the SPSR are used to save state so that the original program can be resumed when the exception routine has completed.<br>
To return after handling the exception, the SPSR is moved into the CPSR, and R14 is moved to the PC.<br>

## Endian
Endianness is the order of bytes of digital data.<br>
ARMv6 supports both big-endian and little-endian operation<br>
(Least Significant bit: the bit that has the lowest value)
- In little-endian mode, the least significant bit is stored at the smallest address. (most common)
- In big-endian mode, the most significant bit is stored at the smallest address.

## Unaligned access support
#### Prior to ARMv6
Traditionally, ARM expects memory accesses to be aligned
- Halfword accesses should be halfword-aligned.
- Word accesses should be word-aligned.
#### Changes with ARMv6:
ARMv6 introduced support for unaligned word and halfword data access.

## Synchronization primitives
#### LDREX (Load-Exclusive)
- When a thread executes an LDREX instruction, it reads data from a memory location and the processor marks this location as having been accessed exclusively by this thread.
- However, this 'exclusive' mark doesn't block other threads or cores from accessing the same memory location. They can still read from and write to this location.
#### STREX (Store-Exclusive)
- STREX checks if the memory location still has its exclusive access status. If no other thread has written to this location since the LDREX was executed, STREX considers the operation successful and writes the new value.
- If, however, another thread has written to the location, the STREX operation will fail, returning a non-zero value.
#### Typical Usage in a Loop:
In practice, the thread often repeatedly reads (LDREX) and tries to write (STREX) in a loop until STREX reports success (indicating no other writes occurred in the meantime).

## Jazelle extension
- **Background**: Jazelle was introduced to mitigate the overheads of JVM by allowing for the native execution of Java bytecode.
- **Operation**: When in Jazelle mode, the processor can directly execute Java bytecode instructions. This is achieved by mapping Java bytecodes to equivalent ARM processor instructions or sequences of instructions.
- **Decline of Jazelle**: The advancements in JIT have largely overshadowed the need for direct bytecode execution.

## Saturated integer arithmetic
Saturated arithmetic prevents the overflow/underflow of typical integer arithmetic. Instead, if an operation results in a value outside the representable range, it is set to the closest representable value (the maximum or minimum value of the data type).
- **For instance**, in 8-bit saturated arithmetic, if adding two numbers results in a value greater than 255, the result is set to 255. Similarly, if subtraction would result in a negative value, the result is set to 0.
