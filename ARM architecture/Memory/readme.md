# Memory
## About the VMSA
A page table is maintained to track which virtual pages are mapped to which physical frames of memory.<br>
The virtual memory address is translated to a physical address using the page table.
For fast page access, the page table is stored in a cache called TLB.(Translation Lookaside Buffer)
The set of memory properties associated with each TLB entry includes:
- **Memory access permission control**: This controls whether a program has no-access, read-only access, or read/write access to the memory area. When an access is not permitted, a memory abort is signaled to the processor.
- **Memory region attributes**: These describe properties of a memory region. Examples include device (VMSAv6), non-cacheable, write-through, and write-back.
- **Virtual-to-physical address mapping**: An address generated by the ARM® processor is called a virtual address. The MMU allows this address to be mapped to a different physical address.

## Memory access sequence
When the ARM CPU generates a memory access, the MMU performs a lookup for a mapping for the requested modified virtual address in a TLB.<br>
If a matching TLB entry is found then the information it contains is used as follows:
1. The access permission bits and the domain are used to determine whether access is permitted. If the access is not permitted the MMU signals a memory abort. Otherwise the access is allowed to proceed.
2. The memory region attributes are used to control:
  - the cache and write buffer
  - whether the access is cached or uncached
  - the target memory type
  - whether the target memory is shared or unshared.
3. The physical address is used for any access to external or tightly coupled memory, and can be used to perform TAG matching for cache entries in physically tagged cache implementations.

![image](https://github.com/vacu9708/Embedded-system/assets/67142421/5ab02229-a6aa-4919-9231-0f59e859ec7d)
## TLB match process
### Key Points:
- TLB (Translation Lookaside Buffer): A cache that stores recent virtual-to-physical address translations for faster memory access.
- ASID (Address Space Identifier): Uniquely identifies a process's virtual memory space.
### Match Process:
#### Virtual Address Comparison
The processor compares the upper bits (bits 31-N) of the requested virtual address with the modified virtual addresses stored in TLB entries.<br>
N is based on the page size of the TLB entry (log2 of the page size).<br>
#### ASID Matching
If a TLB entry's modified virtual address matches:<br>
It must also be either:<br>
- Marked as global (applies to all ASIDs).
- Have an ASID that matches the current ASID (selecting the appropriate application space).
#### Multiple Matches
If multiple entries match, the TLB's behavior is unpredictable.<br>
The OS must ensure only one match by flushing TLBs when global mappings change.<br>
### Block Sizes:
TLB entries support various block sizes for efficient mapping:
- Supersections (16MB)
- Sections (1MB)
- Large pages (64KB)
- Small pages (4KB)

### TLB Miss:
If no match is found, hardware reads the translation table to find the mapping and adds it to the TLB for future use.

### Additional Notes:
- Tiny pages (1KB) are not supported in VMSAv6.
- Larger block sizes (supersections, sections, large pages) map more memory with fewer TLB entries.

## Enabling and disabling the MMU
### Controlling the MMU:
- The MMU is enabled and disabled by setting or clearing the M bit (bit[0]) in register 1 of the System Control coprocessor (CP15).
- It's disabled by default upon reset.
### Memory Access Behavior When MMU is Disabled:
#### Data Accesses:
- Treated as uncacheable and strongly ordered.
- Unexpected data cache hit behavior varies depending on implementation.
#### Instruction Accesses:
Behavior depends on cache architecture:
- Harvard cache: Cacheable or non-cacheable based on I bit (bit[12]) of CP15 register 1.
- Unified cache: Always non-cacheable.
#### Explicit Accesses:
- Strongly ordered.
- W bit (bit[3], write buffer enable) of CP15 register 1 is ignored.
#### Memory Access Permissions and Aborts:
- No permission checks are performed.
- No aborts are generated by the MMU.
#### Address Mapping:
- Flat address mapping: Physical address = modified virtual address.
#### FCSE PID:
- Should be zero (SBZ) when MMU is disabled.
- Clearing FCSE PID is recommended before disabling MMU.
#### Cache and TLB Operations:
- Cache CP15 operations work regardless of MMU state, but use flat mapping when MMU is disabled.
- CP15 TLB invalidate operations work regardless of MMU state.
#### Other Operations:
- Instruction and data prefetch operations work normally.
- Accesses to TCMs (Tightly Coupled Memories) work normally if TCM is enabled.
### Prerequisites for Enabling MMU:
- **CP15 Register Programming**: Set up relevant CP15 registers, including translation tables.
- **Instruction Cache Handling**:
  - Disable and invalidate instruction cache before enabling MMU.
  - Re-enable instruction cache along with MMU.