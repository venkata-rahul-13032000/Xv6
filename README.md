# Xv6 Operating System Project README
## Project 1: Custom Head Command and Uniq Utility
### Project Overview
This project implements custom versions of the head command and the uniq utility within the Xv6 operating system. These utilities provide enhanced functionalities and customization options for users working with text files or input data.

### Custom Head Command
#### Overview
The `custom_head` command is designed for quick previews of text file content. It allows users to check headers, extract specific lines, and more.

#### Code Structure
`custom_head` Function

* Reads and prints the first `num_lines` from a given `file_descriptor`.
* Maintains a line buffer to collect characters of each line.
* Terminates when the desired number of lines has been printed.

`main` Function

* Entry point of the program.
* Initializes variables and the default number of lines to print.
* Processes command-line arguments to customize the number of lines and filenames.
* Invokes the `custom_head` function to print specified lines or default lines from stdin.

#### Approach
* Header Inclusion: Essential header files like `types.h`, `stat.h`, and `user.h` are included using `#include` directives.
* Custom Head Function: Implementation of `custom_head` as the core of the "head" command utility, responsible for processing input and displaying specified lines.
* Buffer for Data Storage: Efficient data processing using a 512-byte buffer named `buffer`.
* Line and Character Counting: Variables like `line_number` and `char_count` track the current line number and character count.
* Line Buffer (`line_buffer`): Temporary storage for lines using a character array.
* Reading and Displaying Data: Reads data in chunks from input, processes it character by character, and prints formatted lines with line numbers.
* Main Function: Handles command-line arguments and orchestrates the execution of the "head" command.

### Custom Uniq Utility
#### Overview
This code implements a custom uniq utility in C for removing duplicate lines while providing various customization options.

#### Code Structure
`strcasecmp` Function
* Enables case-insensitive string comparison using bitwise operations.

`custom_uniq` Function
* Core function for processing input and removing duplicates.
* Utilizes arrays (`pilot`, `output`, `repeat`) to store line data, unique lines, and their counts.
* Reads data from a file descriptor (or stdin) into `buf`, processing input line by line.
* Stores unique lines and counts, then prints results based on command-line arguments.
* Frees dynamically allocated memory.

`main` Function
* Entry point of the program.
* Initializes variables, including `file_descriptor` and `case_insensitive`.
* Handles different scenarios based on the number and values of command-line arguments.
* Supports case-insensitive search with the `-i` flag.

#### Approach
* Header Inclusion: Necessary header files (`types.h`, `stat.h`, `user.h`) are included for compatibility with file operations and standard I/O.
* Case-Insensitive Comparison: Implemented `strcasecmp` function using bitwise operations for accurate comparisons while ignoring case differences.
* `custom_uniq` Function: Core utility for removing duplicate lines and providing customization options. Uses arrays to store data and counts, processes input line by line, and prints results.
* `main` Function: Initializes variables and handles scenarios based on command-line arguments, supporting case-insensitive search with the `-i` flag.

## Project 2: Enhanced Process Management
### Project Overview
In this project, we extended the xv6 operating system by adding essential fields to the process control block (proc structure): creation time (ctime), end time (etime), and total time (rtime). Additionally, we implemented a new system call called `waitx` to record and retrieve this information, providing insights into the process life cycle. We also created a user-space testing script `test.c` that invokes existing functions like `uniq` and `head` and can be expanded to call newly created functions.

### Code Structure
Process Control Block (PCB) Enhancements
* Added new fields to the struct proc (Process Control Block):
  * `ctime`: Represents the creation time of a process.
  * `etime`: Signifies the end time of a process.
  * `rtime`: Records the total time a process has executed.

User-Space Testing Script `test.c`
* This C code tests the `waitx` system call in xv6.
* Forks a child process, executes a specified program in the child, and then waits for it to complete using `waitx`.
* Prints the runtime, creation time, and end time of the child process.

`waitx` system call
* Acquires the process table lock to ensure exclusive access to process-related data structures.
* Enters a loop to scan through the process table (ptable) and look for exited children processes (ZOMBIE state).
* Extracts essential information such as process ID (pid), creation time (ctime), and end time (etime) from the process structure (struct proc).
* Calculates the runtime (rtime) of the child process by subtracting the creation time from the end time and stores these values.
* Resets the process-related fields (ctime, etime, rtime) in the child process structure to zero to avoid lingering values.
* Releases the process table lock, cleans up the child process's resources (e.g., memory, stack), and returns the child process's PID.

`ps` Custom Command Implementation
* Added a custom `ps` command to xv6 for obtaining essential process information.
* Displays process details, including PID, status, start time, total execution time, and process name.
* Implementation includes a new system call `ps` and a user-level program `ps.c` to access and display process data.

### Approach
* Problem Understanding: Understood the requirement to add a new system call `waitx` in xv6 to retrieve runtime, creation time, and end time information of child processes.
* `waitx` System Call Design: Designed the `waitx` system call with three integer pointers as arguments (int *rtime, int *ctime, int *etime) to hold runtime, creation time, and end time.
* Process Structure Enhancement: Modified the struct proc in proc.h to include new fields `ctime`, `etime`, and `rtime`.
* Field Initialization: Implemented code to initialize these fields `ctime` during process creation, `etime` when a process exits, `rtime` calculated as the difference between `etime` and `ctime`.
* `waitx` Implementation: Acquiring the process table lock. Locating exited child processes in the ZOMBIE state. Extracting process information, including PID, `ctime`, and `etime`. Calculating `rtime`. Resetting `ctime`, `etime`, and `rtime` for consistency. Releasing the process table lock and returning the child's PID.
* `ps` Custom Command Implementation: Added a custom `ps` command to xv6, including a new system call `ps` and a user-level program `ps.c`. Implemented the ps() function in proc.c to retrieve and display process information based on the provided process name.

## Project 3: Scheduling Algorithms Implementation
### Project Overview
In this assignment, two scheduling algorithms, First-Come-First-Serve (FCFS) and Priority-Based Scheduling, have been implemented in the xv6 operating system. The average wait and turnaround times for processes, including "uniq (user)," "uniq (kernel)," "head (user)," and "head (kernel)," have been reported. To facilitate testing and evaluation, two test programs, `testsched.c` (FCFS) and `testschedpri.c` (Priority), have been created to execute the respective programs and record statistics.

### Code Structure
Function `scheduler(void)`
* This function is the core of the scheduler and is responsible for selecting and running processes based on two scheduling policies: First-Come-First-Serve (FCFS) and Priority-Based Scheduling.

Initialization:
* Declares pointers to process structures for iteration.
* Initializes a pointer to the CPU structure for the current CPU core.
* Sets the currently running process for this CPU core to NULL.

Infinite Loop:
* Represents an infinite loop, ensuring continuous scheduling.

Enabling Interrupts:
* Enables interrupts on the processor to respond to hardware and software interrupts.

Lock Acquisition:
* Acquires the process table lock to ensure exclusive access to process-related data structures.

Policy-Based Selection:
* Checks the value of `policyy` to determine the scheduling policy to use.

FCFS Policy (`policyy == 0`):
* Selects the process with the earliest arrival time.
* Switches to the chosen process, updates its state to RUNNING, and performs a context switch.
* Updates the process's state and records the end time (`etime`).
* Releases the process table lock.

Priority-Based Scheduling (`policyy == 1`):
* Selects the process with the highest priority.
* Iterates over the process table to find the process with the highest priority.
* Switches to the chosen process, updates its state to RUNNING, and performs a context switch.
* Updates the process's state and records the end time (`etime`).
* Releases the process table lock.

Release Lock:
* Releases the process table lock, allowing other threads or processors to access process-related data structures.

Usage:
* This scheduler code is used with `testsched.c` and `testschedpri.c` to test and evaluate the effectiveness of the scheduling policies in xv6.

### Approach
Problem Understanding:
* Thorough understanding of the task to implement two scheduling algorithms (FCFS and Priority-Based) in the xv6 operating system.

Code Structure Design:
* Planned the overall structure of the code to implement the scheduling algorithms, including the creation of a scheduler function.

Data Structure Enhancement:
* Modified the `struct proc` in the xv6 source code to include additional fields necessary for scheduling algorithms.

Policy Selection:
* Implemented a mechanism (`policyy`) to select the scheduling policy (FCFS or Priority-Based).

FCFS Scheduling (`policyy == 0`):
* Implemented logic to select the process with the earliest arrival time.

Priority-Based Scheduling (`policyy == 1`):
* Implemented logic to select the process with the highest priority.

Process State Management:
* Ensured proper management of process states, transitioning processes to the "RUNNING" state when scheduled.

Time Tracking:
* Implemented mechanisms to track time-related information, such as start time, end time, and relevant timestamps for processes.

Locking and Synchronization:
* Used locks and synchronization mechanisms to ensure exclusive access to process-related data structures.

Report Generation:
* Calculated and stored relevant statistics, including wait times and turnaround times, after each process completed its execution.

Testing and Evaluation:
* Created test programs (`testsched.c` and `testschedpri.c`) to evaluate the performance of scheduling algorithms with different process characteristics.

## Project 4: Null Pointer Dereference Handling and Address Space Rearrangement
### Null Pointer Dereference Handling
#### Null Pointer Dereference Program
* Developed a program intentionally dereferencing a null pointer.
* Tested on Linux and xv6 to observe and compare behaviors.
#### Page Table Modification
* Explored xv6's page table setup.
* Updated to leave the first three pages unmapped, forcing a trap on null pointer dereference.
* Modified `exec()` and `userinit()` functions for address space initialization.

#### `fork()` Functionality
* Ensured child processes inherit the modified address space.
* Updated relevant code sections for proper inheritance.

#### Kernel Parameter Handling
* Examined kernel parameter management, focusing on pointers.
* Implemented checks to prevent issues related to bad pointers.

#### Makefile Changes
* Updated makefile to reflect changes in code segment's starting address.

#### Building and Running
* Followed standard xv6 build process.
* Compiled using gcc and executed on Linux and xv6.
* Tested various user programs to observe modified null pointer dereference behavior.

#### Code Changes
`exec.c`
* Modified sz initialization to leave the first three pages unmapped.

`syscall.c`
* Added conditions to check null pointer dereference in syscall arguments.

`vm.c`
* Adjusted loop variable initialization in copyuvm() function.

`makefile`
* Used -Ttext 0x3000 to set code segment's starting address.

#### Results
* Null Pointer Dereference: Successfully forced a trap on null pointer dereference in xv6.
* Verified proper handling of null pointer dereference in `uniq`, `head`, and `ps` commands.

### Address Space Rearrangement
#### Identify Code Sections
* Examined xv6 source code to locate sections for code initialization, heap management, and user stack allocation.

#### Update Stack Placement
* Redefined user stack's placement at the high end of xv6 user address space.
* Introduced a gap of at least five pages between stack and heap.

#### Modify Address Space Accounting
* Managed `sz` field in `proc` struct for accurate code and heap size tracking.
* Introduced `stacktop` for independent stack size monitoring.

#### Automatic Stack Growth
* Implemented logic for automatic stack growth in case of faults.
* Dynamically allocated new page and mapped it into the address space.

#### Code Changes
`exec.c`
* Introduced stacktop for stack size accounting.

`memlayout.h`
* Added USERTOP variable.

`proc.c`
* Modified stacktop initialization in userinit() and fork().

`syscall.c`
* Ensured current process's address remains below USERTOP limit.

`trap.c`
* Handled page faults for effective memory management.

`vm.c`
* Added function for stack expansion.
* Modified copyuvm() for child process memory replication.

#### Analysis of Commands (uniq, head, and ps)
After Null Pointer Dereference
* Commands executed without errors or abnormal terminations.
* No visible disruptions in output or behavior.
* Stack and memory layout remained stable.

After Stack Rearrangement
* Commands executed smoothly.
* Stack at end of address space, expanding in opposite direction.
* No visible disruptions in output or behavior.

### Analysis of Scheduling Algorithms (FCFS and Priority)
#### FCFS Scheduling
* FCFS operated as expected.
* No noticeable alterations in process execution order or completion time.
* Modifications did not impact FCFS behavior.

#### Priority Scheduling
* Priority scheduling operated as expected.
* No discernible alterations in process execution order based on priorities.
* Modifications did not affect Priority scheduling.
