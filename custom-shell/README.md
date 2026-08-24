# Custom Unix Shell in C++

This project is a custom command-line shell implemented in C++ using low-level system programming concepts. It replicates core Unix shell functionalities such as command execution, piping, redirection, background job control, and process management, built entirely from scratch using POSIX system calls.

---

## Features

- Command execution with arguments (`ls -l`, `grep txt`, etc.)
- Pipelining with `|` (e.g., `cat file.txt | grep error`)
- Input/output redirection with `<`, `>`
- Background process support using `&`
- Persistent command history (with up/down arrow support)
- Wildcard expansion using `*`, `?` (e.g., `rm *.cpp`)
- Signal handling for `Ctrl+C` (SIGINT) and `Ctrl+Z` (SIGTSTP)

### Built-in Commands

- `cd` – Change the current working directory (supports `~`)
- `delep <file>` – Safely deletes a file by detecting and terminating processes currently locking it
- `sb <PID> [-suggest]` – Traverses the process hierarchy and identifies suspicious processes using heuristic-based analysis

### Malware Detection (`sb` / `squashbug`)

The shell includes a custom malware detection utility that identifies potentially malicious processes using two complementary heuristics:

- **Process Activity Heuristic:** Ranks suspicious processes based on the number of child processes spawned and process start time while filtering out system processes to reduce false positives.
- **Process Ancestry Heuristic:** Traverses the parent-child process tree to identify the originating executable through ancestry analysis.

---

## Technologies Used

- **Language:** C++17
- **Libraries:** `readline`, `unistd.h`, `fcntl.h`, `signal.h`, `sys/wait.h`, `glob.h`

### System Programming Concepts

- Process creation using `fork()` and `execvp()`
- Inter-process communication using `pipe()` and `dup2()`
- Signal handling and process control
- Wildcard expansion using `glob()`
- Command history using GNU Readline
- Command parsing and memory-safe argument handling
