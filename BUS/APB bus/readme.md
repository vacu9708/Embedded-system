https://chat.openai.com/c/90ed3bfd-879e-43ee-9459-f8d110edec1f
# APB
The APB (Advanced Peripheral Bus) is part of the AMBA (Advanced Microcontroller Bus Architecture) suite of protocols.<br>
AMBA is a widely used interconnection standard for connecting functional blocks (like CPUs, memory units, and peripherals).
- **Purpose**: APB is designed for low-bandwidth control accesses, for example, peripheral device control. Its primary use is to connect simple peripherals to the primary bus, which could be an AHB (Advanced High-performance Bus) or AXI (Advanced eXtensible Interface) bus in the AMBA standard.
- **Simplicity and Efficiency**: The APB is simpler compared to other buses in the AMBA family. It is optimized for minimal power consumption and reduced complexity of peripherals. This makes it ideal for simple, lower-speed tasks where high throughput is not a critical requirement.
- **Use Cases**: APB is commonly used for interfacing with low-speed peripherals like keyboards, mouse devices, UARTs, SPI (Serial Peripheral Interface) controllers(control signals don't require high speed).

## Timing diagram conventions
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/3790f151-9edd-4f73-bfe7-688a5ebd81aa)<br>
- **Bus change**: This indicates that multiple bits on the bus are changing simultaneously aside from just a single binary digit changing
- **Transient**: This is a temporary, intermediate state during the transition of a signal from one stable state to another. It's a period where the signal is neither high nor low
- **Bus to high impedance**: This transition shows the bus moving to a high impedance state, which is effectively an electrical disconnection. This state is used when multiple devices can drive the bus, but only one should do so at a time to avoid conflicts.

# 2.1 Write Transfers
Write transfers can be categorized into two types:
- With no wait states
- With wait states

## 2.1.1 With no wait states
### Figure 2-1: Write Transfer with no wait states
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/93555941-fc46-4125-9823-1e4ee744411a)<br>
This figure illustrates a basic write transfer process without wait states. The process follows a sequence of clock cycles (T0 to T4), where signals are asserted or deasserted at the rising edge of the clock.
- **At T0**: The process is idle, waiting for the next clock cycle.
- **T1**: The address (`PADDR`) and write data (`PWDATA`) become valid, and the write signal (`PWRITE`) and select signal (`PSEL`) are asserted.
- **T2-T3**: The `PENABLE` signal is asserted, indicating the Access phase is in progress, during which the address and data remain valid.
- **T4**: The `PENABLE` signal is deasserted, marking the end of the transfer. If another transfer is not immediately following, `PSELX` goes LOW.

The transfer completes at the end of this cycle, with all signals remaining valid throughout the Access phase.

## 2.1.2 With wait states
### Figure 2-2: Write Transfer with wait states
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/c505560d-11bf-4bd2-a303-0040d8cc97d5)<br>
This figure shows a write transfer that includes wait states, extending the process to T6.
- **T1-T3**: Similar to the no wait state scenario, `PADDR`, `PWRITE`, and `PSEL` signals are asserted.
- **T4-T5**: The `PENABLE` signal is asserted, but the `PREADY` signal goes LOW, indicating the wait states.
- **T6**: Marks the end of the transfer after the wait states.

The `PREADY` signal can extend the transfer by driving `PREADY` LOW when `PENABLE` is HIGH.

>Note: It is recommended that the address and write signals remain stable until another access occurs to reduce power consumption.
