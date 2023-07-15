# 0-basics 

## Background knowledge (very basic skip to next if you know how to compile and can do basic analysis)
I take the very standard unix view when it comes to compilers they are
a part of the pipeline when generating programs and analysing them. When I was
a Reverse Engineer I was intrested in going backwards which is in my opinion less
complex than going forward namely because the compiler builds (*.c, *.cpp, *.rs) 
to functional object code and links them. That bit about linking can be extremely complex
take the following code

```c
//c 
#include <stdio.h>
int main() {
    printf("Hello \n");
    return 0;
}
```
You can build this code many ways for many operating systems, and architectures i'm going to focus on 2.
Gcc and clang. If you are targeting x86 its very naturally to use gcc as it is an expert forward
x86 systems but if you want to build for all the following [x86, arm, riscv, YourCompaniesCoolNewArch] you are likely to use
clang. But why? The reason clang is prefered is because it is a modular compiler framework. 
Gcc while becoming more modular was not built on the same ethos to start with. 
So lets build the above. I work a lot on ubuntu's default distro; which 
has gcc by default you might have to download clang but you really want a whole llvm toolchain
```bash
#!bash
gcc a.c -o g.o 
clang a.c -o c.o
```

On either file you can examine the object files with some of the following
```bash
#!bash
vimdiff <(readelf -a g.o) <(readelf -a c.o)  # board information about the elf file that is generated
nm c.o              # information about the symbol locations useful often
llvm-objdump-14 g.o # prints the asm of the binary with source if available
```

The astute reader will notice that we used llvm-objdump on the gcc created binary. 
This is key a good compiler generates generic standard binary outputs. gcc and llvm 
in my opinion are good compilers they can generate object files, elf but they also can 
ingest and many types of files and pipeline them to some other format. While the two
object files like differ there interperation should be equal to the input of the author.
If its not there is a bug but compilers tend to be much higher quality than your average
software product so if output does not match intent its 99.999% of the time because the 
authors code is wrong. This is relabitity fact is extremely important and taken for granted

### Usage information
```bash
#!bash
man readelf 
man nm  # man llvm-nm-14 
man llvm-objdump-14
```
Did I really just say use man? The answer is absolutely; the reason is that your llvm/gcc version differ from mine.
The man pages are very useful to understanding parts of the toolchain and what they do.
While clang can build a file complete it really has several steps and invokes several binaries
to do this. The advantage of this is that you can stop at specific step for speed of compile, but
also introspection. If compiling your whole project takes hours try breaking you compile into 
stages, its very rare that in large projects you edit every file at once. Breaking you compile
into phases is who large companies scale building. 

