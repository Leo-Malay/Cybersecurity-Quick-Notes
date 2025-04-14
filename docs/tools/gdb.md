# GDB - GNU Project Debugger

**GDB** is a powerful debugging tool used primarily for programs written in C, C++, and other languages.

- Allows users to monitor and control the execution of a program
- Enables them to set breakpoints, inspect variables, step through code line by line, and identify the source of errors or bugs
- Executes the program in a controlled environment where one can pause execution at specific points, examine the program’s state, modify data values, and resume execution

All of the features mentioned above makes it an essential tool for diagnosing and fixing issues in software development.

**Category:** Debugger

## Installation

If you are using **Linux**, you can use the following command to install GDB on your system.

```bash
sudo apt-get install gdb
```

On **MacOS** using **HomeBrew**, you can use the following command.

```bash
brew install gdb
```

To verify installation of your GDB, simply use the following command.

```bash
gdb --version
```

Here is the link to the official documentation for installing [GDB](https://sourceware.org/gdb/current/onlinedocs/gdb#Installing-GDB) additionally, if you wish to download, please use the official project [downloads](https://sourceware.org/gdb/download/) page.

## Usage

To get started, please ensure that GDB has been correctly installed. First we will learn to start and quit the gdb tool. To start the tool, you can either use the tool name and optionally, if you wish to run the tool for working on a specific file, you can pass the path to the file which will load the file code.

```bash
gdb                     # Starts GDB tool without any file

gdb ./program.c         # Starts GDB tool with file "program.c"
```

Once you the tool strats without any error, you must be able to see **(gdb)** as a placeholder for command input. Now at any point to time, you wish to exit the tool, you can use the **quit** command.

```bash
(gdb) quit
```

Awesome, now as we now know how to

## References

- [Sourceware - GDB](https://sourceware.org/gdb/)
- [Wikipedia - GDB](https://en.wikipedia.org/wiki/GNU_Debugger)
