# xv6 Kernel Extensions

This repository serves as a monorepo for advanced operating system feature implementations built upon the xv6 RISC-V kernel.

> **Note:** Because these features involve deep, mutually exclusive modifications to core kernel subsystems (Process Scheduler, Virtual Memory, Trap Handling), they are isolated in separate **Git Branches**.

## Implementation Modules

To view the code for a specific feature, please **switch to the corresponding branch** using the dropdown menu above or the links below.

### 1. [Branch: Weighted Round Robin Scheduler](https://github.com/apnfunk/Operating-System-Projects/tree/feature/weighted_rr_scheduler)
**Focus:** Process Scheduling & CPU Resource Management
*   **Algorithm:** Implemented a **Weighted Round Robin (WRR)** policy where CPU time slices are proportional to user-defined process priorities.
*   **Starvation Avoidance:** Designed the dispatcher to ensure low-priority processes receive minimal CPU guarantees, preventing indefinite blocking.
*   **System Calls:** Added `set_priority(pid, n)` and `get_priority()` to allow dynamic userspace control over process execution weights.
*   **Concurrency:** Secured the process control block (PCB) with spinlocks to prevent race conditions during priority updates in a multi-core environment.

### 2. [Branch: Copy-on-Write (COW) Fork](https://github.com/apnfunk/Operating-System-Projects/tree/feature/COW(copy-on-write))
**Focus:** Virtual Memory Optimization
*   **Mechanism:** Implemented **Atomic Reference Counting** for physical pages (`kalloc.c`).
*   **Logic:** Modified `fork()` to map parent pages as read-only in the child. Upon a write attempt (Page Fault), the kernel allocates a new page, copies data, and updates permissions.
*   **Impact:** Significantly reduces memory overhead and `fork()` latency for large processes.

### 3. [Branch: Demand Paging & MRU Replacement](https://github.com/apnfunk/Operating-System-Projects/tree/feature/demand-paging)
**Focus:** Memory Virtualization & Swapping
*   **Page Replacement:** Implemented **Most Recently Used (MRU)** eviction policy to manage resident pages.
*   **Disk Swapping:** Enabled swapping victim pages to a simulated disk interface when physical RAM is exhausted.
*   **Monitoring:** Extended the kernel to track page faults, swap-ins, and swap-outs via the `getpagestat()` system call.
* 
### 4. [Branch: Large Files & Symbolic Links](https://github.com/apnfunk/Operating-System-Projects/tree/feature/Large-Files-%26-Symbolic-Links)

**Focus:** File System Extensions

This branch extends the xv6 file system with two major features:

#### Task 1: Large File Support (Doubly-Indirect Blocks)

- **Large Files:** Extended the xv6 inode structure with a **doubly-indirect block pointer**, increasing the maximum supported file size from approximately **268 KB to 8 MB**.
- **Block Mapping:** Enhanced the file system to support **three-level block addressing** (direct, single-indirect, and doubly-indirect) with on-demand block allocation.
- **Memory Management:** Updated block deallocation logic to recursively free indirect metadata blocks during file truncation.
- **Compatibility:** Preserved the original on-disk inode size by reducing the number of direct pointers instead of modifying the inode layout.

#### Task 2: Symbolic Links

- **System Call:** Implemented a new `symlink(target, path)` system call for creating symbolic (soft) links.
- **Path Resolution:** Extended `open()` to transparently resolve symbolic links while preventing infinite loops through bounded recursive traversal.
- **POSIX Support:** Added support for the `O_NOFOLLOW` flag, allowing applications to open the symbolic link itself instead of its target.
- **File System Integration:** Added a dedicated inode type (`T_SYMLINK`) and integrated symbolic links into xv6's file creation and pathname resolution logic.
---

## Tech Stack
*   **Kernel:** C (RISC-V Architecture)
*   **User Space:** C
*   **Assembly:** RISC-V (`trampoline.S`, `entry.S`)
*   **Tools:** QEMU Emulator

## Usage
To run any specific implementation:
1. Clone the repo:
   ```bash
   git clone https://github.com/apnfunk/Operating-System-Projects.git
   cd xv6-enhanced-kernel
   ```
2. Switch to the desired feature branch:
    ```bash
    git checkout feature/weighted_rr_scheduler
    ```

    ```bash
    git checkout feature/COW(copy-on-write)
    ```

    ```bash
    git checkout feature/demand-paging
    ````

     ```bash
    git checkout feature/Large-Files-&-Symbolic-Links
    ````
3. Compile and run:
    ```bash
    make clean
    make qemu
    ```

---
