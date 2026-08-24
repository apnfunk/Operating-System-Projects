# Custom Unix Shell in C++

This project is a custom command-line shell implemented in C++ using low-level system programming concepts. It mimics core functionalities of the Unix shell such as command execution, piping, redirection, and background job control — built entirely from scratch using POSIX system calls.

---

## Features

- Command execution with arguments (`ls -l`, `grep txt`, etc.)
-  Pipelining with `|` (e.g., `cat file.txt | grep error`)
-  Input/output redirection with `<`, `>`
-  Background process support using `&`
-  Persistent command history (with up/down arrow support)
-  Wildcard expansion using `*`, `?` (e.g., `rm *.cpp`)
-  Signal handling for `Ctrl+C` (SIGINT) and `Ctrl+Z` (SIGTSTP)
-  Built-in commands:
  - `cd` – change directory (supports `~`)
  - `delep <file>` – safely deletes a file by detecting and killing processes locking it
  - `sb <PID> [-suggest]` – traverses parent processes and heuristically identifies suspicious ones

---

##  Technologies Used

- **Language:** C++ (C++17)
- **Libraries:** `readline`, `unistd.h`, `fcntl.h`, `signal.h`, `sys/wait.h`, `glob.h`
- **Concepts Implemented:**
  - `fork()`, `execvp()` for process execution
  - `pipe()` and `dup2()` for piping
  - Signal handling for process interruption and termination
  - Wildcard expansion with `glob()`
  - Command history using GNU Readline
  - Basic parsing and memory-safe argument handling

---


