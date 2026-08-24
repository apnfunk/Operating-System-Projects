# Operating System Projects

A collection of operating system projects exploring process management, scheduling, virtual memory, file systems, and independent custom shell implementation.

The repository contains:

- A standalone **Custom Shell** (available in the `custom-shell/` directory on the `main` branch).
- Advanced operating system feature implementations in process scheduling, virtual memory, and file systems built upon the **[xv6-riscv](https://github.com/mit-pdos/xv6-riscv)** teaching operating system developed by the MIT PDOS group.(multiple feature extensions in their own branch)

> **Note:** The xv6 implementations involve deep, mutually exclusive modifications to core kernel subsystems (Process Scheduler, Virtual Memory, File System, and Trap Handling). Therefore, each implementation is maintained in a separate Git branch.

---

## Custom UNIX Shell (Main Branch)

**Focus:** UNIX Process Management & Shell Design

Implemented a POSIX-style command-line shell from scratch in C++ using low-level system programming concepts.

### Features

- Command execution with arguments
- Pipes (`|`) and input/output redirection (`<`, `>`)
- Background process execution (`&`)
- Wildcard expansion (`*`, `?`)
- Persistent command history
- Signal handling (`Ctrl+C`, `Ctrl+Z`)
- Built-in commands including `cd` and `delep`
- **Malware Detection (`squashbug`)**
  - Uses a **process activity heuristic** based on child-process count and process start time while filtering system processes.
  - Uses a **process ancestry heuristic** to identify suspicious executables through parent-child lineage traversal.

---

## xv6 Kernel Extension Modules

To view a particular implementation, switch to the corresponding feature branch.

### 1. [Weighted Round Robin Scheduler](https://github.com/apnfunk/Operating-System-Projects/tree/feature/weighted_rr_scheduler)

**Focus:** Process Scheduling & CPU Resource Management

- **Algorithm** : Implemented a **Weighted Round Robin (WRR)** policy where CPU time slices are proportional to user-defined process priorities.
- **Starvation Avoidance**: Designed the dispatcher to ensure low-priority processes receive minimal CPU guarantees, preventing indefinite blocking.
- **System Calls** : Added `set_priority(pid, n)` and `get_priority()` to allow dynamic userspace control over process execution weights.
- **Concurrency** : Secured the process control block (PCB) with **spinlocks** to prevent race conditions during priority updates in a multi-core environment.
  
---

### 2. [Copy-on-Write (COW) Fork](https://github.com/apnfunk/Operating-System-Projects/tree/feature/COW(copy-on-write))

**Focus:** Virtual Memory Optimization

- **Mechanism:** Implemented **atomic reference counting** for physical pages (kalloc.c).
- **Logic:** Modified `fork()` to initially share read-only pages between parent and child. Upon a write attempt (Page Fault), the kernel allocates a new page, copies data, and updates permissions.
- **Impact:** Significantly reduces memory overhead and fork() latency for large processes.
---

### 3. [Demand Paging & MRU Replacement](https://github.com/apnfunk/Operating-System-Projects/tree/feature/demand-paging)

**Focus:** Memory Virtualisation & Swapping

- **Page Replacement:** Implemented **Most Recently Used (MRU)** eviction policy to manage resident pages.
- **Disk Swapping:** Enabled swapping victim pages to a simulated disk interface when physical RAM is exhausted.
- **Monitoring:** Extended the kernel to track page faults, swap-ins, and swap-outs via the `getpagestat()` system call.
- 
---

### 4. [Large Files & Symbolic Links](https://github.com/apnfunk/Operating-System-Projects/tree/feature/Large-Files-%26-Symbolic-Links)

**Focus:** File System Extensions

#### **Large File Support:**
- Extended xv6 with **doubly-indirect block addressing**, increasing the maximum supported file size from approximately **268 KB to 8 MB** while preserving the original on-disk inode layout.
- **Block Management:** Enhanced block allocation and deallocation to support three-level block mapping (direct, single-indirect, and doubly-indirect blocks).
  
#### **Symbolic Links:** 

- Implemented the `symlink(target, path)` system call along with  a dedicated `T_SYMLINK` inode type for symbolic (soft) links..
- Added support for symbolic link resolution with bounded recursive traversal.
- Introduced the `O_NOFOLLOW` flag and integrated symbolic links into xv6 pathname resolution.

---

## Tech Stack

- **Languages:** C, C++, RISC-V Assembly
- **Kernel:** xv6 (RISC-V)
- **System Programming:** POSIX APIs
- **Tools:** QEMU, GDB, GNU Make

---

## Usage

Clone the repository:

```bash
git clone https://github.com/apnfunk/Operating-System-Projects.git
cd Operating-System-Projects
```

### Run the Custom UNIX Shell

```bash
cd custom-shell
make
./shell
```

### Run an xv6 Kernel Extension

Switch to the required branch:

```bash
git checkout feature/weighted_rr_scheduler
```

or

```bash
git checkout feature/COW(copy-on-write)
```

or

```bash
git checkout feature/demand-paging
```

or

```bash
git checkout feature/Large-Files-\&-Symbolic-Links
```

Then build xv6:

```bash
make clean
make qemu
```
