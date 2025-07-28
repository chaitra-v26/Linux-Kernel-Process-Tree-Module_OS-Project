# Linux Kernel Process Tree Module

A Linux kernel module that creates multiple processes/threads in a binary tree structure and displays their task names, states, and process IDs in hierarchical format. Demonstrates kernel-level thread management, process tree visualization and memory management for Operating Systems project.

## 🎯 Project Overview

This project was developed as part of an Operating Systems course to demonstrate understanding of Linux kernel programming concepts. The kernel module creates a binary tree of kernel threads, where each node represents a process/thread with its associated metadata.

**Problem Statement**: Create a Linux kernel module that spawns multiple processes/threads (children and siblings) and outputs their task names, states, and process IDs in a tree structure while the task is executing.

## ✨ Features

- 🌳 **Binary Tree Creation**: Generates a hierarchical structure of kernel threads
- 📊 **Process Information Display**: Shows task names, PIDs, and parent-child relationships
- 🔄 **Recursive Thread Management**: Implements recursive thread creation and cleanup
- 🧠 **Memory Management**: Proper kernel memory allocation and deallocation
- 📝 **Kernel Logging**: Comprehensive logging using `printk()` for debugging
- 🔒 **Signal Handling**: Graceful thread termination with signal management
- 🏗️ **Data Structures**: Custom tree implementation using Linux kernel linked lists

## 🛠️ Tech Stack

### Programming Languages
- **C**: Core kernel module implementation
- **Makefile**: Build configuration and compilation

### Kernel APIs & Libraries
- `linux/init.h` - Module initialization/cleanup
- `linux/module.h` - Module metadata and macros
- `linux/kernel.h` - Kernel utility functions
- `linux/sched.h` - Process/task management
- `linux/kthread.h` - Kernel threading API
- `linux/signal.h` - Signal handling
- `linux/slab.h` - Memory allocation (`kmalloc`, `kfree`)
- `linux/list.h` - Kernel linked list implementation

### Development Tools
- **GCC**: GNU Compiler Collection for kernel compilation
- **Make**: Build automation tool
- **Linux Kernel Build System**: For module compilation
- **dmesg**: Kernel message logging and debugging

### System Requirements
- **Linux Kernel**: Version 4.x or higher
- **Architecture**: x86_64 (tested on Ubuntu/Debian)
- **Permissions**: Root/sudo access required

## 📋 Prerequisites

### System Requirements
- Linux distribution (Ubuntu 20.04+ recommended)
- Kernel headers installed
- GCC compiler
- Make utility
- Root/sudo privileges

### Installing Dependencies

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r)
```

**CentOS/RHEL/Fedora:**
```bash
sudo yum groupinstall "Development Tools"
sudo yum install kernel-devel kernel-headers
```

**Arch Linux:**
```bash
sudo pacman -S base-devel linux-headers
```

## 🚀 Installation & Usage

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/linux-kernel-process-tree-module.git
cd linux-kernel-process-tree-module
```

### 2. Build the Module
```bash
make
```

**Expected Output:**
```
make -C /lib/modules/6.5.0-25-generic/build M=/home/user/project modules
make[1]: Entering directory '/usr/src/linux-headers-6.5.0-25-generic'
CC [M] /home/user/project/my_kernel_module12.o
MODPOST /home/user/project/Module.symvers
CC [M] /home/user/project/my_kernel_module12.mod.o
LD [M] /home/user/project/my_kernel_module12.ko
make[1]: Leaving directory '/usr/src/linux-headers-6.5.0-25-generic'
```

### 3. Load the Module
```bash
sudo insmod my_kernel_module.ko
```

### 4. View the Output
```bash
sudo dmesg 
```

### 5. Remove the Module
```bash
sudo rmmod my_kernel_module
```

### 6. Clean Build Files
```bash
make clean
```

## 📁 Project Structure

```
linux-kernel-process-tree-module/
├── my_kernel_module.c    # Main kernel module source code
├── Makefile               # Build configuration
├── README.md             # Project documentation
└── .gitignore           # Git ignore file (optional)
```

### File Descriptions

- **`my_kernel_module.c`**: Core implementation containing thread creation, tree management, and cleanup logic
- **`Makefile`**: Defines build rules for kernel module compilation
- **`README.md`**: Comprehensive project documentation

## 🔧 How It Works

### Architecture Overview

1. **Module Initialization**: Creates a root kernel thread
2. **Binary Tree Construction**: Recursively spawns child threads (2 per level)
3. **Tree Traversal**: Displays the complete hierarchy
4. **Memory Management**: Proper allocation/deallocation of tree nodes
5. **Module Cleanup**: Graceful termination of all threads

### Key Components

#### 1. Tree Node Structure
```c
struct tree_node {
    int pid;                    // Process ID
    char name[16];             // Thread name
    struct list_head children; // List of child nodes
    struct list_head sibling;  // Sibling node link
};
```

#### 2. Thread Function
- Each thread runs `child_function()`
- Handles signals for graceful termination
- Maintains interruptible sleep state

#### 3. Tree Creation Algorithm
- **Levels**: Configurable depth (default: 3 levels)
- **Branching**: Binary tree (2 children per node)
- **Naming**: Pattern `thread_<level>_<index>`

#### 4. Memory Management
- Uses `kmalloc()` for node allocation
- Implements proper cleanup with `kfree()`
- Handles allocation failures gracefully

## 📊 Sample Output

```
[INFO] Binary Tree Logger Module: Initialization
[INFO] Created root thread: PID=1234
[INFO] Created thread: PID=1235, Parent PID=1234, Level=1
[INFO] Created thread: PID=1236, Parent PID=1234, Level=1
[INFO] Created thread: PID=1237, Parent PID=1235, Level=2
[INFO] Created thread: PID=1238, Parent PID=1235, Level=2
[INFO] Created thread: PID=1239, Parent PID=1236, Level=2
[INFO] Created thread: PID=1240, Parent PID=1236, Level=2
[INFO] Process Tree Structure:
├── root_thread(1234)
    ├── thread_1_0(1235)
        ├── thread_2_0(1237)
        └── thread_2_1(1238)
    └── thread_1_1(1236)
        ├── thread_2_0(1239)
        └── thread_2_1(1240)
```

## 🐛 Troubleshooting

### Common Issues

#### 1. Compilation Errors
**Problem**: `fatal error: linux/module.h: No such file or directory`
**Solution**:
```bash
sudo apt install linux-headers-$(uname -r)
```

#### 2. Permission Denied
**Problem**: `Operation not permitted` when loading module
**Solution**: Ensure you're using `sudo`:
```bash
sudo insmod my_kernel_module.ko
```

#### 3. Module Already Loaded
**Problem**: `File exists` error
**Solution**: Remove existing module first:
```bash
sudo rmmod my_kernel_module
sudo insmod my_kernel_module.ko
```

#### 4. Build Directory Not Found
**Problem**: Missing kernel build directory
**Solution**: Install matching kernel headers:
```bash
sudo apt install linux-headers-$(uname -r)
```

### Debugging Tips

1. **Check kernel logs**: `sudo dmesg`
2. **Verify module loading**: `lsmod | grep my_kernel_module`
3. **Check system resources**: `free -h` and `ps aux`
4. **Monitor system**: Use `htop` or `top` while module runs

## 📚 Learning Outcomes

This project demonstrates proficiency in:

- **Kernel Programming**: Understanding of Linux kernel APIs and development practices
- **Process Management**: Creation and management of kernel threads
- **Data Structures**: Implementation of tree structures using kernel linked lists
- **Memory Management**: Proper allocation and deallocation in kernel space
- **System Programming**: Low-level programming concepts and debugging
- **Concurrency**: Thread synchronization and signal handling
- **Build Systems**: Makefile creation and kernel build process

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---
