#  Process Management Subsystem in Linux

This repository contains example C programs and resources that demonstrate key concepts of **process management** in **Linux system programming**. These examples are ideal for learning how Linux handles process creation, execution, communication, and termination.

---

##  Folder Structure

process_management/
├── child_process.c # Creating child process using fork()
├── chroot_systemcall.c # Using chroot() system call
├── cow_fork.c # Copy-On-Write behavior in fork()
├── create_zombie_process.c # Demonstrating zombie process
├── deamon_process.c # Creating a daemon process
├── demo_execvp.c # Using execvp() to execute commands
├── demo_execvpe.c # Using execvpe() with custom environment
├── demo_fork.c # Basic fork() usage
├── ... # More examples.


---

##  Topics Covered

- `fork()`, `vfork()`, `clone()` system calls
- Parent-child process relationship
- Zombie and orphan processes
- Daemon processes
- `exec()` family of system calls (`execl()`, `execv()`, `execvp()`, etc.)
- `wait()` and `waitpid()` system calls
- `getpid()`, `getppid()`, `setpgid()`, and process groups
- Chroot jail using `chroot()`

---

##  How to Compile

Use `gcc` to compile any of the programs:

```bash
gcc filename.c -o output_name
./output_name

