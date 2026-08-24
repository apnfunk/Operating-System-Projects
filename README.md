# Operating System Projects

A collection of operating system projects exploring process management, scheduling, virtual memory, file systems, and UNIX shell implementation.

The repository contains:

- 🐚 A standalone **Custom UNIX Shell** (available in the `custom-shell/` directory on the `main` branch).
- 🖥️ Multiple **xv6 kernel extensions**, each maintained in its own feature branch.

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

- Implemented a **Weighted Round Robin (WRR)** scheduler with CPU time slices proportional to process priority.
- Added `set_priority(pid, priority)` and `get_priority()` system calls.
- Guaranteed minimum CPU allocation to lower-priority processes to prevent starvation.
- Protected priority updates using spinlocks for safe multicore execution.

---

### 2. [Copy-on-Write (COW) Fork](https://github.com/apnfunk/Operating-System-Projects/tree/feature/COW(copy-on-write))

**Focus:** Virtual Memory Optimization

- Implemented **atomic reference counting** for physical pages.
- Modified `fork()` to initially share read-only pages between parent and child.
- Allocated private pages only upon write faults.
- Reduced memory usage and improved `fork()` performance.

---

### 3. [Demand Paging & MRU Replacement](https://github.com/apnfunk/Operating-System-Projects/tree/feature/demand-paging)

**Focus:** Virtual Memory & Swapping

- Implemented **Most Recently Used (MRU)** page replacement.
- Added swapping between physical memory and simulated disk.
- Introduced `getpagestat()` to monitor page faults, swap-ins, and swap-outs.

---

### 4. [Large Files & Symbolic Links](https://github.com/apnfunk/Operating-System-Projects/tree/feature/filesystem_extensions)

**Focus:** File System Extensions

#### Large File Support

- Extended xv6 with **doubly-indirect block addressing**, increasing the maximum supported file size from approximately **268 KB to 8 MB**.
- Updated block allocation and deallocation to support three-level block mapping while preserving the original inode layout.

#### Symbolic Links

- Implemented the `symlink(target, path)` system call.
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
git checkout feature/filesystem_extensions
```

Then build xv6:

```bash
make clean
make qemu
```
