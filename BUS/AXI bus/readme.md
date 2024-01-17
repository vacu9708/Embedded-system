# Introduction
AXI is designed to provide a high-performance communication interface between hardware components.

## Key features of AXI
- **High Performance**: Supports high-speed data transfer and efficient communication between different parts of a system.
- **Separate Read and Write Channels**: It has independent channels for read and write operations, enhancing throughput and efficiency.
- **Separate Address(control) and Data channels**: It allows address information to be issued ahead of the actual data transfer.
- **Support for Burst Transfers**: AXI allows burst-based transfers, which is efficient for moving large blocks of data.
- **Out-of-Order Transaction(pipeline)**: It allows for transactions to complete in any order, not necessarily the order they were initiated.
- **Flexible and Scalable**: It's adaptable to various types of implementations and can be scaled according to the system requirements.
- **Spport for Multiple Masters and Slaves**: AXI can handle communication between multiple master and slave devices, making it suitable for complex SoC (System on Chip) designs.

## 1.2 Architecture
The AXI protocol is designed to be burst-based and supports high-performance data transfers between master and slave components.
### Figure 1-1
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/d47218eb-c2ed-4b16-b2e9-fa1e2e70d9bc)<br>
Shows how a read transaction uses the read address and read data channels.(Separate address and data channel)
### Figure 1-2
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/26f148a9-74b1-4c45-b35f-d19f806510dd)<br>
Shows how a write transaction uses the write address, write data, and write response channels.
### 1.2.1 Channel Definition
The AXI protocol defines five independent channels, each employing a `VALID` and `READY` handshake mechanism to ensure proper data transfers.<br>
- The source(sender) uses the `VALID` signal to show when valid data or control information is available on the channel.
- The destination(recipient) uses the `READY` signal to show when it is prepared to accept the data.
- A **LAST** signal is also employed to denote the end of a burst of data transfers.
- **Read Address and Write Address Channels**: Read and write transactions each have their own address channel. These carry the address and control information for their respective transactions.
- **Read Data Channel**: This channel conveys both the read data and read response from the slave to the master.
- **Write Data Channel**: This channel carriees the write data from the master to the slave. 
- **Write Response Channel**: Provides a way for the slave to respond to write transactions. All write transactions use completion signaling, which occurs once for each burst, not for each individual data transfer within the burst.

Each channel supports different operations, such as variable-length bursts, transfers of various sizes (from 8 to 1024 bits), and system-level caching and buffering control. These features contribute to the flexibility and efficiency of the AXI protocol.

### 1.2.2 Interface and Interconnect
#### Figure 1-3
![image](https://github.com/vacu9708/Embedded-system/assets/67142421/c12520db-4dac-42b2-931e-764af058d693)<br>
Illustrates the structure of a typical system that includes multiple master and slave devices connected through an interconnect.
- **Master Devices**: Initiate transactions.
- **Slave Devices**: These are the responders to the transactions initiated by the masters.
- **Interconnect**: The interconnect allows communication between master and slave devices.
#### 1.2.3 Register Slices
- **Unidirectional Information Transfer**: Each AXI channel is designed to transfer information in only one direction.
- **Register Slices**: These can be inserted into any channel to help with timing and isolation, potentially adding a cycle of latency but allowing for a **trade-off** between latency(register slice) and maximum frequency of operation(direct, fast).

# Channel Handshake
## Handshake Process (3.1)
- **Purpose**: The handshake process in AXI is used to synchronize data and control information between the master and slave devices.(like TCP handshake)
- **Mechanism**: It uses a two-way VALID/READY flow control system to ensure data transfer occurs at an appropriate rate without loss or error.
- **Signals**:
  - **VALID**: Indicates data or control information is available from the source.
  - **READY**: Indicates the destination can accept the data or control information.
- **Conditions**: Both VALID and READY signals must be high for the data transfer to take place.
