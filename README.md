# Large Files & Symbolic Links (xv6 File System Extensions)

This branch extends the **MIT xv6-riscv** file system with two major features:

1. **Large File Support** using doubly-indirect block addressing.
2. **Symbolic Link** support with transparent pathname resolution.

> Note: This branch intentionally includes only the modified xv6 source files required for the assignment. The complete xv6 codebase is available from the original MIT repository and the other feature branches in this project.
---

## Task 1: Large File Support

### Overview

Extended the xv6 inode structure to support **doubly-indirect block addressing**, increasing the maximum file size from approximately **268 KB** to **8 MB** while preserving the original on-disk inode size.

### Implementation

- Reduced `NDIRECT` from **12 → 11** to accommodate an additional doubly-indirect pointer without changing the inode layout.
- Added a doubly-indirect block pointer to the inode.
- Extended `bmap()` to support:
  - Direct blocks
  - Single-indirect blocks
  - Doubly-indirect blocks
- Allocated metadata blocks on demand.
- Updated `itrunc()` to recursively free all indirect metadata blocks.
- Preserved the original **128-byte** on-disk inode structure.

### Files Modified

| File | Changes |
|------|---------|
| `kernel/fs.h` | Updated inode layout, block constants, and maximum file size definitions. |
| `kernel/fs.c` | Implemented doubly-indirect block allocation in `bmap()` and recursive cleanup in `itrunc()`. |

### Results

- Maximum file size increased to approximately **8 MB**.
- Supports three-level block addressing:
  - Direct
  - Single Indirect
  - Doubly Indirect

---

## Task 2: Symbolic Links

### Overview

Implemented POSIX-style **symbolic (soft) links** by introducing a new inode type and the `symlink()` system call.

### Implementation

- Added a new inode type `T_SYMLINK`.
- Implemented the `symlink(target, path)` system call.
- Extended `open()` to automatically resolve symbolic links.
- Added support for the `O_NOFOLLOW` flag.
- Prevented infinite recursive link traversal using a bounded depth limit.
- Integrated symbolic links into xv6 pathname resolution.

### Files Modified

| File | Changes |
|------|---------|
| `kernel/stat.h` | Added `T_SYMLINK` inode type. |
| `kernel/fcntl.h` | Added `O_NOFOLLOW` open flag. |
| `kernel/syscall.h` | Registered new system call number. |
| `kernel/syscall.c` | Added syscall mapping. |
| `kernel/sysfile.c` | Implemented `sys_symlink()`, symbolic link resolution in `open()`, and updated `create()`. |
| `user/user.h` | Added user-space declaration. |
| `user/usys.pl` | Generated syscall stub. |

### Validation

The implementation was verified through tests covering:

- Symbolic link creation
- Reading through links
- Writing through links
- Circular link detection
- `O_NOFOLLOW` behavior
- Chained symbolic links
- Safe unlinking of symbolic links

---

## Build

```bash
make clean
make qemu
```

---

## Technologies

- C
- xv6-riscv
- File Systems
- Virtual Memory
- Block Allocation
- POSIX System Calls