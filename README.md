# Maleos Kernel

A lightweight, modern hybrid operating system kernel designed for efficiency, modularity, and memory safety. **Maleos** bridges the performance benefits of a monolithic kernel with the isolated stability of a microkernel architecture.

---

## 🗺️ System Architecture Overview

Maleos utilizes a **Hybrid Architecture** featuring a highly optimized kernel core wrapped around a flexible Hardware Abstraction Layer (HAL).

Use code with caution.
+-------------------------------------------------------------------+
|                           USER SPACE                              |
|   +-------------------+  +-------------------+  +-------------+   |
|   |  User Apps / CLI  |  |  Native Utilities |  | GUI Engine  |   |
|   +---------+---------+  +---------+---------+  +------+------+   |
+-------------|----------------------|-------------------|----------+
| (System Call Interface / APIs)           |
+-------------v----------------------v-------------------v----------+
|                           KERNEL SPACE                            |
|                                                                   |
|   +-----------------------------------------------------------+   |
|   |                  System Call Interface                    |   |
|   +-----------------------------------------------------------+   |
|                                                                   |
|   +-------------------+  +-------------------+  +-------------+   |
|   | Process/Thread    |  | Virtual Memory    |  | Inter-Process|  |
|   | Scheduler         |  | Manager (VMM)     |  | Comm (IPC)  |   |
|   +-------------------+  +-------------------+  +-------------+   |
|                                                                   |
|   +-------------------+  +-------------------+  +-------------+   |
|   | Virtual File      |  | Network Stack     |  | Driver Shim |   |
|   | System (VFS)      |  | (TCP/IP)          |  | Layer       |   |
|   +-------------------+  +-------------------+  +-------------+   |
|                                                                   |
|   +-----------------------------------------------------------+   |
|   |        Hardware Abstraction Layer (HAL) / Bootloader      |   |
|   +-----------------------------------------------------------+   |
+-------------------------------------------------------------------+
|
[ PHYSICAL HARDWARE ]

### Core Subsystems

* **System Call Interface (SCI):** Secure gateway enabling unprivileged user applications to request isolated kernel privileges.
* **Process Scheduler:** Preemptive scheduler managing task execution slices across multiple threads using a prioritized distribution model.
* **Virtual Memory Manager (VMM):** Implements paging structures, protects memory boundaries, and manages physical and virtual resource allocations.
* **Virtual File System (VFS):** Abstraction layer providing uniform file access controls over distinct file storage formats.
* **Hardware Abstraction Layer (HAL):** Isolates the core kernel subsystems from machine-specific assembly implementations.

---

## 🛠️ Development & Roadmap Plan

The development of Maleos follows a strict six-stage lifecycle to guarantee baseline stability before advanced layer implementation:

[Phase 1: Boot] -> [Phase 2: Memory] -> [Phase 3: Multitask] -> [Phase 4: Drivers] -> [Phase 5: VFS] -> [Phase 6: Userland]

### 1. Phase 1: Bootstrapping & Baseline
* [ ] Multiboot-compliant bootloader integration (GRUB environment configuration).
* [ ] Global Descriptor Table (GDT) and Interrupt Descriptor Table (IDT) configuration.
* [ ] Early initialization display mapping via a basic VGA/framebuffer text driver.

### 2. Phase 2: Memory Management
* [ ] Reading hardware memory layouts provided by the bootloader map.
* [ ] 4KB physical block tracking via a custom Page Frame Allocator.
* [ ] Virtual memory paging setup and continuous dynamic kernel heap allocator.

### 3. Phase 3: Preemptive Multitasking
* [ ] Programmable timer integration (PIT/APIC) to drive scheduling intervals.
* [ ] Context switching implementation via CPU state registration backups.
* [ ] Dynamic scheduling thread queues for task execution handling.

### 4. Phase 4: Basic Input/Output Drivers
* [ ] Interactive hardware line bindings for standard input setups (PS/2 Keyboard).
* [ ] Secondary storage mass-device access pipelines (IDE/PATA/AHCI interfaces).

### 5. Phase 5: Virtual File System (VFS)
* [ ] Common inode/vnode tracking layouts within the VFS pipeline.
* [ ] Implement a lightweight structured file format (such as `ext2` or custom read-only file indexes).

### 6. Phase 6: User Land Execution
* [ ] Implement architectural privilege transitions (`syscall` and `sysret` setups).
* [ ] Binary compilation image handling (ELF binary loading structures).
* [ ] Launch user land interactive runtime shell console environment.

---

## ⚙️ Environment Setup & Toolchain

### Prerequisites

To compile and emulate the Maleos kernel, install the required cross-compilers, build management tools, and emulation suites:

```bash
# Ubuntu / Debian dependencies example
sudo apt update
sudo apt install build-essential bison flex libgmp3-dev libmpc-dev libmpfr-dev \
                 texinfo qemu-system-x86 grub-pc-bin xorriso gdb
```

### Build & Compilation Target
* **Target Architecture:** `x86_64-elf` (Cross-compiled environment targets)
* **Default Configuration Engine:** `Makefile` / `CMake` configuration frameworks

---

## 🚀 Running and Debugging

### Compilation
To compile the core source layout assets and package them into an execution-ready boot ISO:
```bash
make iso
```

### Simulation / Emulation
To quickly execute the compiled Maleos ISO within a safe isolated QEMU environment:
```bash
make run
```

### Remote Kernel Debugging
To initialize the execution matrix under localized GDB hooks for hardware line inspection:
```bash
# Runs QEMU paused, listening on localhost port 1234
make debug

# In a separate terminal session, connect using your GDB tool:
gdb -ex "target remote localhost:1234" -ex "symbol-file build/kernel.elf"
```

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file 
