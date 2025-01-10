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
* [Assignments](#assignments)
  * [Assignment 1](#assignment-1)
  * [Assignment 2](#assignment-2)
    * [Implementation:](#implementation)
    * [Validation/Deliverables:](#validationdeliverables)

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

## Assignments

### Assignment 1

### Assignment 2

Solution completed @ LINK

#### Implementation:

1. **Setup your assignment repository** using the GitHub Classroom instructions and links provided.

   - You should see your previous assignment content in the repository linked from the GitHub Classroom link at the top of the document when you complete these steps.

2. **Setup an ARM cross-compile toolchain** as described in Module 1 instructions and the wiki page at [Installing an ARM aarch64 developer toolchain](https://github.com/cu-ecen-aeld/aesd-assignments/wiki/Installing-an-ARM-aarch64-developer-toolchain).

3. **Create a file** `assignments/assignment2/cross-compile.txt` in your repository showing version, configuration, and sysroot path of `aarch64-none-linux-gnu-gcc` using `-print-sysroot` and `-v` options.

   - There are no restrictions on how you create or fill this file, and you may copy and paste from the command line output.
   - If using redirection, ensure content written to stderr is redirected correctly using the guidance [here](https://stackoverflow.com/a/876242).

4. **Write a C application** `writer` (`finder-app/writer.c`), which:

   - Implements file IO as described in LSP Chapter 2.
   - Requirements:
     - Unlike Assignment 1, **you do not need to create directories**—assume they already exist.
     - Set up **syslog logging** using the `LOG_USER` facility.
     - Write a syslog message: `"Writing <string> to <file>"`, where `<string>` is the text written and `<file>` is the file created.
     - Log unexpected errors with `LOG_ERR` level.
   - Example syslog levels:
     ```c
     syslog(LOG_DEBUG, "Writing %s to %s", string, file);
     syslog(LOG_ERR, "Error writing to file: %s", strerror(errno));
     ```

5. **Write a Makefile**, which includes:

   - A default target to build the `writer` application.
   - A clean target to remove the application and `.o` files.
   - Support for **cross-compilation**:
     - When `CROSS_COMPILE` is specified as `aarch64-none-linux-gnu-`, the build system should use the cross-compiler.

6. **Modify `finder-test.sh`**, ensuring:

   - Clean-up of any previous build artifacts.
   - Native compilation of `writer`.
   - Replacement of `writer.sh` with the `writer` application.

7. **Test the implementation** using the script:

   ```bash
   ./full-test.sh
   ```

8. **Verify file type** using the `file` utility:
   - Include the output of the `file` utility for a build using `CROSS_COMPILE` in `assignments/assignment2/fileresult.txt`.

#### Validation/Deliverables:

1. Your `assignments/assignment2/cross-compile.txt` file should show the version, configuration, and sysroot path (output of `-print-sysroot`).

2. The `finder-test.sh` script should return “success” when run.

3. Your writer application should meet requirements from assignment 1 regarding error handling.

4. Ensure all error handling has been implemented for `writer.c`.

   - Ensure syslog logging is set up and working properly (you should see messages logged to `/var/log/syslog` on your Ubuntu VM).
   - If using Windows Subsystem for Linux, you may need to manually start syslog with `sudo service rsyslog start`.

5. Your `assignments/assignment2/fileresult.txt` should show that you were able to cross-compile successfully.

6. Your GitHub Actions automated test script should pass on your repository, and the "Actions" tab should show a successful run on your last commit.

---
