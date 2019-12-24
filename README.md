CMPSC473 -**- mode: org -**-

# CMPSC473 Notes

## Topic 1 – Bit Manipulation

### Why Hex?

arises from it being difficult to represent numbers in binary or decimal
– its like an intermediate form binary is too verbose whereas decimal
takes time to convert hex is easy to represent and convert – in C they
are prefixed by 0x each digit in a hex number is 4 bits long from 0000
to 1111 – 16 different numbers from 0-F

### Bitmasks

0x12345678 & 0x00FF0000
<span class="underline">\_\_\_\_\_\_\_\_\_</span> 0x00340000 Literally
like a mask

### Word size

determines how large the address space is going to be for a machine is
equal to 2<sup>(word<sub>size</sub>)</sup> number of bytes 32-bit
machines has about 4gb number of bytes as address space

### How to negate a hex?

take 2's complement for example, 10 in hex is A and in binary is 1010
for -10 take 2's complement i.e. 0101+1 = 0110 which is 6 in hex 1010 +
0101 ignoring carry gives 0000 i.e. 10 + (-10) = 0

### memcpy vulnerability

void\* memcpy(void\* dest, void\* src, size<sub>t</sub> n) – here n is
an unsigned integer if we pass a negative number it will be interpreted
incorrectly because it will be case as an unsigned number with the MSB
as 1

## Topic 2 – C Programming and intro to OS

### Pointer Arithmetic

pointers in C are denoted using \<data<sub>type</sub>\>\*
\<identifier\>; they represent the memory address of the stored
variable. different datatypes have different pointer increment sizes
i.e. for char its 1 byte, for int its 4 bytes thus if int\* p = 0x1000
==\> p+1 = 0x1004 because its increment size is 4 bytes

### Structure Alignment

this is an optimization done by processors to reduce read and write
time. for a 32-bit machine the word size is 4 bytes. thus, the processor
can read 4 bytes at a time. let us assume the following struct:

``` c
struct{
 int a1;
 int a2;
 char c1;
 char c2;
 float f1;
}
```

this data will not be stored sequentially but will be padded to optimize
read times. say address begins from 0x1000 the addressing will be as
follows:

|        |        |            |            |
| ------ | ------ | ---------- | ---------- |
| 0x1000 | 0x1001 | 0x1002     | 0x1003     |
| a1     | a1     | a1         | a1         |
| 0x1004 | 0x1005 | 0x1006     | 0x1007     |
| a2     | a2     | a2         | a2         |
| 0x1008 | 0x1009 | 0x100A     | 0x100B     |
| c1     | c2     | **\*\*\*** | **\*\*\*** |
| 0x100C | 0x100D | 0x100E     | 0x100F     |
| f1     | f1     | f1         | f1         |

as you can see 0x100A and 0x100B are padded so that f1 can be read fast

### Include Guards

``` c
#ifndef HEADER_FILE_H
#define HEADER_FILE_H
/*code goes here*/
#endif
```

read the [wiki](https://en.wikipedia.org/wiki/Include_guard) for include
guards

### Process Layout

consists of threads and memory memory divided into following segments:

|                                                                               |
| ----------------------------------------------------------------------------- |
| stack : current state of execution, local variables and scope, grows downward |
| heap : malloc, dynamically allocated, grows upward                            |
| text : consists of instructions to be executed + read only data               |
| data : global vars, static vars                                               |

### Strings

strings are defined in c
