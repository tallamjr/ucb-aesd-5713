# Notes

<!-- mtoc-start -->

* [Week 1](#week-1)
  * [**Linux Overview**](#linux-overview)
  * [**Linux Command Line Basics**](#linux-command-line-basics)
  * [**System Programming Essentials**](#system-programming-essentials)
  * [**File Permissions**](#file-permissions)
  * [**Linux File Systems**](#linux-file-systems)
  * [**Processes and Threads**](#processes-and-threads)
  * [**Error Handling**](#error-handling)
  * [**Toolchain and Cross Compilation**](#toolchain-and-cross-compilation)
  * [**Scripting**](#scripting)
  * [**Logging with Syslog**](#logging-with-syslog)

<!-- mtoc-end -->

## Week 1

### **Linux Overview**

- **Adoption of Linux**:
  - Started with TiVo in 1999.
  - As of 2017, over 2 billion devices run Linux.
- **Advantages**:
  - Open source, modifiable, supports a wide range of hardware architectures.
  - Built-in functionalities like schedulers, network stacks, USB, Wi-Fi, etc.

### **Linux Command Line Basics**

- **Utilities**:
  - **`echo`**: Print a message.
    ```bash
    echo "Hello, Linux!"
    ```
  - **`pwd`**: Print the current working directory.
  - **`ls`**: List directory contents.
  - **`cd`**: Change directory.
  - **`mkdir`**: Create a new directory.
    ```bash
    mkdir new_directory
    ```
- **Redirection**:
  - `>`: Redirect output to a new file.
  - `>>`: Append output to an existing file.
    ```bash
    echo "This is a log" > log.txt
    echo "Another line" >> log.txt
    ```

### **System Programming Essentials**

- **Key Concepts**:
  - **Syscalls**: Kernel functions (e.g., `read()`, `write()`).
  - **API vs ABI**:
    - API: Source compatibility (e.g., `printf()`).
    - ABI: Binary compatibility (e.g., byte ordering, register use).
  - **POSIX**: Defines standards for compatibility across Unix-like systems.

### **File Permissions**

- Permission levels: User, Group, Others.
- Octal Representation:
  - Read = 4, Write = 2, Execute = 1.
  ```bash
  chmod 755 script.sh
  ```
- **Checking Permissions**:
  ```bash
  ls -l
  ```

### **Linux File Systems**

- **Everything is a File**: Devices, files, and directories are accessed similarly.
- **Links**:
  - **Hard Link**: Points to the same inode.
    ```bash
    ln file1 hardlink
    ```
  - **Symbolic Link**: Points to a filename (can span filesystems).
    ```bash
    ln -s file1 symlink
    ```

### **Processes and Threads**

- **Processes**:
  - Identified by PID.
  - Utilize virtualized memory and CPU.
- **Threads**:
  - Share the same memory space but have their own stacks.
- **Inter-Process Communication (IPC)**:
  - Pipes, Semaphores, Message Queues, Shared Memory.

### **Error Handling**

- Use **`errno`** from `<errno.h>` for error reporting.
  ```c
  if ((fp = fopen("file.txt", "r")) == NULL) {
      perror("Error");
      exit(EXIT_FAILURE);
  }
  ```
- Always save `errno` before another operation that might modify it.

### **Toolchain and Cross Compilation**

- **Toolchains**: Compiler, Linker, and Runtime Libraries.
- **Cross Compilation**:
  - Compiling for a different target architecture.
  ```bash
  aarch64-none-linux-gnu-gcc -o program source.c
  ```
- **Sysroot**: Contains libraries and headers for the target system.

### **Scripting**

- **Simple Script**:
  ```bash
  #!/bin/bash
  echo "Hello, World!"
  ```
- **Using Variables**:
  ```bash
  #!/bin/bash
  greeting="Hello"
  echo "${greeting}, Linux!"
  ```

### **Logging with Syslog**

- Use the **syslog API** for structured logging.
  ```c
  #include <syslog.h>
  syslog(LOG_INFO, "This is an informational message");
  ```
- Logs are stored in `/var/log/syslog`.
