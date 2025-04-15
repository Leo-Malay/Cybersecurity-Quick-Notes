# GDB - GNU Project Debugger

**GDB** is a powerful debugging tool used primarily for programs written in C, C++, and other languages.

- Allows users to monitor and control the execution of a program
- Enables them to set breakpoints, inspect variables, step through code line by line, and identify the source of errors or bugs
- Executes the program in a controlled environment where one can pause execution at specific points, examine the program’s state, modify data values, and resume execution

All of the features mentioned above makes it an essential tool for diagnosing and fixing issues in software development.

**Category:** [Debugger](tools/index?id=debugger)

## Installation

If you are using **Linux**, you can use the following command to install GDB on your system.

```bash
($) sudo apt-get install gdb
```

On **MacOS** using **HomeBrew**, you can use the following command.

```bash
($) brew install gdb
```

To verify installation of your GDB, simply use the following command.

```bash
($) gdb --version
```

Here is the link to the official documentation for installing [GDB](https://sourceware.org/gdb/current/onlinedocs/gdb#Installing-GDB) additionally, if you wish to download, please use the official project [downloads](https://sourceware.org/gdb/download/) page.

## Usage

To get started, please ensure that GDB has been correctly installed. First we will learn to start and quit the gdb tool. To start the tool, you can either use the tool name and optionally, if you wish to run the tool for working on a specific file, you can pass the path to the file which will load the file code.

```bash
($) gdb                     # Starts GDB tool without any file
($) gdb ./program.c         # Starts GDB tool with file "program.c"
```

Once you the tool strats without any error, you must be able to see **(gdb)** as a placeholder for command input. Now at any point to time, you wish to exit the tool, you can use the **quit** command.

```bash
(gdb) quit
```

Awesome, now as we now know how to start and quit the GDB, lets learn about some basic commands. Let's say on executing, we want the GDB to stop executing at a specific point in the code / assembly code. For this purpose we can setup a break point. To create a break point, you can either use the name of the function or the line number where the code is located. Sometimes if you want to create a pbreakpoint in assembly code, you can give the function name followed by offset.

```bash
(gdb) break <function_name>                       # e.g.: break main
(gdb) break <line_number>                         # e.g.: break 47
(gdb) break *<function_name>+<offset>             # e.g.: break *main+4
```

To see the list of all the breakpoints, you can use the following command.

```bash
(gdb) info breakpoints
```

To delete a specific break point, enter the **delete** command followed by the breakpint id. Optionally, if you want to delete all the breakpoint, one can just use **delete** and it will remove all the breakpoints.

```bash
(gdb) delete <id>                                     # e.g.: delete 3
(gdb) delete                                          # delete all the breakpoints
```

As required in one of the previous command, to be able to disassemble the code into the assembly code use the following command.

```bash
(gdb) disassemble <function_name>                  # disassemble main
```

Now assume that you are working remotely and want to see the source code but do not want to exit the GDB tool, you can do that using the following command.

```bash
(gdb) list                                          # e.g.: list
(gdb) list <line_number>                            # e.g.: list 23
```

At any point of time, if you wish to print the value of any variable, use the following command.

```bash
(gdb) print <variable_name>
```

Finally, we have all the breakpoints setup and now we want to run the code, we can simply use **run** command or if you have a payload file, then you can pipe the input to the program. UPon execution, if you have not breakpoints, the program will continue till EOF. However, if you do have breakpoints, then it would stop execution upon encountering a breakpoint.

```bash
(gdb) run                                   # Simply run the program.
(gdb) run < /path/to/payload                # Run program with input from an external file.
```

Now that you are at the breakpoint you can observe the values of the registers, look into the current frame and do other things.

```bash
(gdb) info registers            # Displays the values of all the registers.
(gdb) info frame                # Display the current frame.
```

Now you want to move to the next line in code, then you may want to use the **next**, but if you want to execute the next line in assembly code, you must use the **step** command.

```bash
(gdb) step              # Go to next line in assembly (Goes deep into the function)
(gdb) next              # Go to next line in code (Runs multiple assembly line)
```

It is possible to have a lot of code in between 2 different breakpoints, you can continue executing until you encounter another breakpoint using the following command.

```bash
(gdb) continue
```

> **ProTip:** You can directly keep pressing "Enter" to use the previous command. :wink:

## References

- [Sourceware - GDB](https://sourceware.org/gdb/)
- [Wikipedia - GDB](https://en.wikipedia.org/wiki/GNU_Debugger)
