# xv6 - A re-implementation of Unix Version 6

xv6 is a re-implementation of Dennis Ritchie's and Ken Thompson's Unix
Version 6 (v6). xv6 loosely follows the structure and style of v6,
but is implemented for a modern RISC-V multiprocessor using ANSI C.

## ACKNOWLEDGMENTS

xv6 is inspired by John Lions's Commentary on UNIX 6th Edition (Peer
to Peer Communications; ISBN: 1-57398-013-7). See also https://pdos.csail.mit.edu/6.1810/, which provides
pointers to online resources for v6.

The following people have made contributions: Russ Cox (context switching,
locking), Cliff Frey (MP), Xiao Yu (MP), Nickolai Zeldovich, and Austin
Clements.

We are also grateful for the bug reports and patches contributed by
Takahiro Aoyagi, Marcelo Arroyo, Silas Boyd-Wickizer, Anton Burtsev,
carlclone, Ian Chen, Dan Cross, Cody Cutler, Mike CAT, Tej Chajed,
Asami Doi, Wenyang Duan, eyalz800, Nelson Elhage, Saar Ettinger, Alice
Ferrazzi, Nathaniel Filardo, flespark, Peter Froehlich, Yakir Goaron,
Shivam Handa, Matt Harvey, Bryan Henry, jaichenhengjie, Jim Huang,
Matúš Jókay, John Jolly, Alexander Kapshuk, Anders Kaseorg, kehao95,
Wolfgang Keller, Jungwoo Kim, Jonathan Kimmitt, Eddie Kohler, Vadim
Kolontsov, Austin Liew, l0stman, Pavan Maddamsetti, Imbar Marinescu,
Yandong Mao, Matan Shabtay, Hitoshi Mitake, Carmi Merimovich, Mark
Morrissey, mtasm, Joel Nider, Hayato Ohhashi, OptimisticSide,
phosphagos, Harry Porter, Greg Price, RayAndrew, Jude Rich, segfault,
Ayan Shafqat, Eldar Sehayek, Yongming Shen, Fumiya Shigemitsu, snoire,
Taojie, Cam Tenny, tyfkda, Warren Toomey, Stephen Tu, Alissa Tung,
Rafael Ubal, Amane Uehara, Pablo Ventura, Xi Wang, WaheedHafez,
Keiichi Watanabe, Lucas Wolf, Nicolas Wolovick, wxdao, Grant Wu, x653,
Jindong Zhang, Icenowy Zheng, ZhUyU1997, and Zou Chang Wei.

## ERROR REPORTS

Please send errors and suggestions to Frans Kaashoek and Robert Morris
(kaashoek,rtm@mit.edu). The main purpose of xv6 is as a teaching
operating system for MIT's 6.1810, so we are more interested in
simplifications and clarifications than new features.

## BUILDING AND RUNNING XV6

You will need a RISC-V "newlib" tool chain from
https://github.com/riscv/riscv-gnu-toolchain, and qemu compiled for
riscv64-softmmu. Once they are installed, and in your shell
search path, you can run "make qemu".

---

## Modifications by Manely Ghasemnia Hamedani

This fork of xv6 introduces several key enhancements and new system calls as part of an educational operating systems project. These are grouped into three phases:

### Phase 1: `child_processes` System Call

Introduced a new syscall `child_processes` that retrieves and displays all descendant processes of the calling process.

**Features:**
- Lists all descendant processes.
- Outputs each process’s:
  - Name
  - PID
  - Parent PID
  - Status (e.g., running, sleeping).

### Phase 2: Multithreading Enhancements

This phase focuses on multithreading improvements and introduces key system calls for thread management.

**Features:**
- **`create_thread`**: Creates a new thread with a specific runner function and arguments.
- **`stop_thread`**: Allows a thread to stop execution.
- **`join_thread`**: Allows a thread to wait for another thread to complete its execution before proceeding.
  
In this phase, thread management is enhanced with a new **`thread` structure** that supports multiple thread states like `joined`, `runnable`, `running`, `free`, etc. This enables efficient management of multiple concurrent threads in the system.

**Test Programs:**
- **`threadtest.c`**: A test program that demonstrates thread creation, joining, and stopping.

### Phase 3: CPU Scheduling Enhancements

This phase adds multiple CPU scheduling improvements to better manage process execution.

**Key Features:**
- **Set CPU Quota**: The system now supports setting CPU quotas for processes, helping to manage CPU usage more efficiently across tasks.
- **`cpu_usage`**: A new mechanism to track the CPU usage of individual processes.
- **Deadline Scheduling**: New scheduling techniques to handle deadlines for processes with real-time requirements.

**Core Features:**
- **`set_cpu_quota`**: This system call enables setting CPU limits for specific processes.
- **`cpu_usage`**: Tracks and reports the CPU usage of processes to help optimize performance.
- **New Scheduling Algorithms**: 
  - **`MinCU`**: Minimum CPU usage scheduling.
  - **Round-Robin**: Enhanced round-robin scheduling with deadlines.
  
This phase also introduces a more advanced `top` command that displays process statistics, including their CPU usage, quota, and execution status.

**Test Programs:**
- **`cpuschedtest.c`**: Demonstrates and validates the new CPU scheduling features, including CPU quotas and deadline management.

---

### Additional Enhancements

#### `killall` System Call
Terminates all processes except core system ones (`init`, `shell`, etc.), helping with system resets or testing.

#### Testing Programs

Several user-space programs were created to test and demonstrate the new features:

- **`childrentest.c`**: Spawns multiple child processes and displays their details via `child_processes`.
- **`reptest.c`**: Simulates crashes and uses `myrep` to print crash logs.
- **`sysrep.c`**: Reads and prints the contents of `/reports.bin` using `sysrep`.

---

## LICENSE

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
