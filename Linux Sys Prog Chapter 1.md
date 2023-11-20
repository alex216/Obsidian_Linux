![[Linux Sys Prog 1 index.png]]
- This book is about system programming on *Linux*.
	- Linux supports additional system calls, new features.
# System Programming
- Even X Window System exposes in full view the core Unix system API.
	- it can be said that this book is about Linux programming in *general*.
	- but doesn't cover Linux programming *env*, such as tutorial on *make*.
- system programs interface primarily with the kernel and sys libraries, application programs also interface with high-level libraries, which is similar.
## Why Learn System Programming
- what interesting sys calls are provided in Linux compared to other Unix? etc..
- Those questions are at the center of this book.
## Cornerstones of System Programming
syscalls, the C lib, the C compiler.
## System Calls
- System programming starts and ends with *system calls*.
- They are func invocation made from user space into the kernel in order to request service or resource from the OS.
- Linux implements far fewer syscalls than most other OS kernel.
- x86-64 architecture's syscalls is around 300, compared with thousands of syscalls on Microsoft Windows.
- Nonetheless, more than 90% is same in all architectures.
### Invoking system calls
- For security reason, user-space app can't directly execute kernel code or modify kernel data.
- a user-space app can "signal" the kernel that will invoke a syscalls.
- Then the app *trap* into the kernel to execute kernel code.
- mechanism varies from architecture to architecture.
- On i386, `int` with the value of `0x80` is a software interrupt instruction which user-space app executes.
- This causes the kernel to executes a software interrupt handler-the syscall handler.
- On i386, `ebx, ecx, edx, esi, edi`in order, pass the 5 parameters.
- if more, they use a pointer to a buffer in user space.
## The C Library
- The C library(*libc*) is at the heart of Unix applications.
- On modern Linux systems, the C library is provided by *GNU libc*, abbreviated *glibc*.
- *glibc* is more than its name. It provides wrappers for syscalls, threading support, basic app facilities other than the standard C library.
## The C Compiler
- In Linux, the standard C complier is provided by the *GNU Compiler Collection(gcc)*.
- Originally, *gcc* was GNU's version of *cc*, the *C Compiler*, Thus, *gcc* stood for *GNU C Compiler.*
- Nowadays, *gcc* is used as the generic name for the family of GNU compilers.
- However, *gcc* is also the binary used to invoke the C compiler.
- In this book, *gcc* means program *gcc*, typically.
### C++
- C++ add 2 more foundation, the standard C++ library and the GNU C++ compiler.
	- the C++ *library* implements C++ system interfaces and ISO C++11 standard.
		- It's provided by the *libstdc(libstdcxx)++* library
	- The *GNU C++ compiler* is the standard on Linux
		- It's provided by the g++ binary.
## APIs and ABIs
### APIs
- interfaces at the source level.
- usually it provides abstraction by providing a standard set of interfaces, func that higher-level  software can invoke lower-level.
- API is not a "contract".
	- API user(higher-level software) has zero input into the API sometimes.
	- API only ensure both follow the API.
	- they are *source compatible*.
## ABIs
- whereas an API defines a source interface, an ABI defines the binary interface.
- the ABI is a func of both the OS(say, Linux) and the architecture(say, x86-64)