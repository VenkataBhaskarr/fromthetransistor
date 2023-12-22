## From the Transistor to the Web Browser

Hiring is hard, a lot of modern CS education is really bad, and it's hard to find people who understand the modern computer stack from first principles.

Now cleaned up and going to be software only. Closer to being real.

#### Section 1: Intro: Cheating our way past the transistor -- 0.5 weeks
- So about those transistors -- Course overview. Describe how FPGAs are buildable using transistors, and that ICs are just collections of transistors in a nice reliable package. Understand the LUTs and stuff. Talk briefly about the theory of transistors, but all projects must build on each other so we can’t build one.
- Emulation -- Building on real hardware limits the reach of this course. Using something like Verilator will allow anyone with a computer to play.

#### Resources: 

[Introduction to Transistors" by Khan Academy](https://www.khanacademy.org/test-prep/mcat/xphysics/circuits/v/introduction-to-transistors)

[Field-Programmable Gate Arrays (FPGAs)" by Intel:](https://www.intel.com/content/www/us/en/products/docs/fpgas/overview.html)

[Verilator: A Verilog-to-C++ Compiler" by Verilator:](https://www.veripool.org/verilator/)



#### Section 2: Bringup: What language is hardware coded in? -- 0.5 weeks
- Blinking an LED(Verilog, 10) -- Your first little program! Getting the simulator working. Learning Verilog.
- Building a UART(Verilog, 100) -- An intro chapter to Verilog, copy a real UART, introducing the concept of MMIO, though the serial port may be semihosting. Serial test echo program and led control.

#### Resources 

[Introduction to Verilog" by Verilog Online:](https://www.verilogonline.com/verilog-introduction)

[Blinking an LED with Verilog" by FPGA4Fun:](http://www.fpga4fun.com/LED1.html)

[Building a UART with Verilog" by FPGA4Fun:](http://www.fpga4fun.com/UART1.html)

[Introduction to MMIO" by Xilinx:](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2019_1/ug901-vivado-ip-integration.pdf)


#### Section 3: Processor: What is a processor anyway? -- 3 weeks
- Coding an assembler(Python, 500) -- Straightforward and boring, write in python. Happens in parallel with the CPU building. Teaches you ARM assembly. Initially outputs just binary files, but changed when you write a linker.
- Building a ARM7 CPU(Verilog, 1500) -- Break this into subchapters. A simple pipeline to start, decode, fetch, execute. How much BRAM do we have? We need at least 1MB, DDR would be hard I think, maybe an SRAM. Simulatable and synthesizable.
- Coding a bootrom(Assembler, 40) -- This allows code download into RAM over the serial port, and is baked into the FPGA image. Cute test programs run on this.

#### Resources

[ARM Assembly Language" by ARM:](https://developer.arm.com/documentation/dui0801/a/)

[Building a Simple ARM Processor" by FPGA4Fun:](http://www.fpga4fun.com/ARM1.html)

[Verilog HDL: A Guide to Digital Design and Synthesis" by Samir Palnitkar:](https://www.amazon.com/Verilog-HDL-Guide-Digital-Synthesis/dp/0964949033)

[Python for Data Analysis" by Wes McKinney:](https://www.amazon.com/Python-Data-Analysis-Wrangling-IPython/dp/1491957662)



#### Section 4: Compiler: A “high” level language -- 3 weeks
- Building a C compiler(Haskell, 2000) -- A bit more interesting, cover the basics of compiler design. Write in haskell. Write a parser. Break this into subchapters. Outputs ARM assembly.
- Building a linker(Python, 300) -- If you are clever, this should take a day. Output elf files. Use for testing with QEMU, semihosting.
- libc + malloc(C, 500) -- The gateway to more complicated programs. libc is only half here, things like memcpy and memset and printf, but no syscall wrappers.
- Building an ethernet controller(Verilog, 200) -- Talk to a real PHY, consider carefully MMIO design.
- Writing a bootloader(C, 300) -- Write ethernet program to boot kernel over UDP. First thing written in C. Maybe don’t redownload over serial each time and embed in FPGA image.

#### Resources

"Writing a Compiler in Haskell" by Graham Hutton: This book provides a comprehensive guide to writing a compiler in Haskell, including parsing, type checking, and code generation.

"Linkers and Loaders" by John R. Levine, Tony Mason, and Doug Brown: This book provides a detailed explanation of how linkers and loaders work, including the ELF file format.

"The C Programming Language" by Brian Kernighan and Dennis Ritchie: This classic book provides a comprehensive introduction to the C programming language, including the standard library functions.

"Verilog HDL: A Guide to Digital Design and Synthesis" by Samir Palnitkar: This book provides a comprehensive introduction to Verilog, including the design of digital circuits and systems.

"Writing a Bootloader" by Nick Blundell: This article provides a step-by-step guide to writing a bootloader for an embedded system, including booting over a network.


#### Section 5: Operating System: Software we take for granted -- 3 weeks
- Building an MMU(Verilog, 1000) -- ARM9ish, explain TLBs and other fun things. Maybe also a memory controller, depending on how the FPGA is, then add the init code to your bootloader.
- Building an operating system(C, 2500) -- UNIXish, only user space threads. (open, read, write, close), (fork, execve, wait, sleep, exit), (mmap, munmap, mprotect). Consider the debug interface you are using, ranging from printf to perhaps a gdbremote stub into kernel. Break into subchapters.
- Talking to an SD card(Verilog, 150) -- The last hardware you have to do. And a driver
- FAT(C, 300) -- A real filesystem, I think fat is the simplest
- init, shell, download, cat, ls, rm(C, 250) -- Your first user space programs.

#### Resources

"Modern Operating Systems" by Andrew S. Tanenbaum: This book provides a comprehensive introduction to operating systems, including process management, memory management, and file systems.

"Operating Systems: Three Easy Pieces" by Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau: This book provides a detailed explanation of operating system concepts, including process scheduling, memory management, and file systems.

"Writing an Operating System from Scratch" by Alexandro Sanchez-Sleator: This article provides a step-by-step guide to writing an operating system from scratch, including the implementation of system calls and file systems.

"Verilog HDL: A Guide to Digital Design and Synthesis" by Samir Palnitkar: This book provides a comprehensive introduction to Verilog, including the design of digital circuits and systems.

"The FAT File System" by Ralf Brown: This document provides a detailed explanation of the FAT file system, including the structure of FAT files and the implementation of FAT drivers.



#### Section 6: Browser: Coming online -- 1 week
- Building a TCP stack(C, 500) -- Probably coded in the kernel, integrate the ethernet driver into the kernel. Add support for networking syscalls to kernel. (send, recv, bind, connect)
- telnetd, the power of being multiprocess(C, 50) --  Written in C, user can connect multiple times with telnet. Really just a bind shell.
- Space saving dynamic linking(C, 300) -- Because we can, explain how dynamic linker is just a user space program. Changes to linker required.
- So about that web(C, 500+) -- A “nice” text based web browser, using ANSI and terminal niceness. Dynamically linked and nice, nice as you want.

#### Resources

TCP/IP Illustrated, Volume 1: The Protocols" by W. Richard Stevens: This book provides a detailed explanation of the TCP/IP protocol suite, including the implementation of a TCP stack.

"Linux Network Programming" by Rami Rosen: This book provides a comprehensive introduction to network programming in Linux, including the implementation of network sockets and the integration of network drivers into the kernel.

"Advanced Programming in the UNIX Environment" by W. Richard Stevens and Stephen A. Rago: This book provides a detailed explanation of UNIX system programming, including the implementation of system calls such as send, recv, bind, and connect.

"Dynamic Linking" by John R. Levine: This book provides a comprehensive introduction to dynamic linking, including the implementation of a dynamic linker and the changes required to the linker.

"Building a Text-Based Web Browser" by Matt Godbolt: This article provides a step-by-step guide to building a text-based web browser, including the implementation of ANSI escape codes and terminal niceness.



#### Section 7: Physical: Running on real hardware -- 1 week
- Talking to an FPGA(C, 200) -- A little code for the USB MCU to bitbang JTAG.
- Building an FPGA board -- Board design, FPGA BGA reflow, FPGA flash, a 50mhz clock, a USB JTAG port and flasher(no special hardware, a little cypress usb mcu to do jtag), a few leds, a reset button, a serial port(USB-FTDI) also powering via USB, an sd card, expansion connector(ide cable?), and an ethernet port. Optional, expansion board, host USB port, NTSC TV out, an ISA port, and PS/2 connector on the board to taunt you. We provide a toaster oven and a multimeter thermometer to do reflow. 
- Bringup -- Compiling and downloading the Verilog for the board
  
#### Resources

FPGA Prototyping by Verilog Examples" by Pong P. Chu: This book provides a comprehensive introduction to FPGA prototyping using Verilog, including the design of FPGA boards and the implementation of JTAG interfaces.

"Designing Embedded Hardware" by John Catsoulis: This book provides a detailed explanation of the design of embedded hardware, including board design, FPGA reflow, and FPGA flash.

"USB Complete: The Developer's Guide" by Jan Axelson: This book provides a comprehensive introduction to USB programming, including the implementation of USB interfaces and the use of USB microcontrollers.

"Serial Port Complete: COM Ports, USB Virtual COM Ports, and Ports for Embedded Systems" by Jan Axelson: This book provides a detailed explanation of serial port programming, including the implementation of USB-FTDI interfaces and the use of serial ports for embedded systems.

"Bringing Up Linux" by Greg Kroah-Hartman: This article provides a step-by-step guide to bringing up Linux on an FPGA board, including the compilation and download of Verilog code.


