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
- Whereas an API defines a source interface, an ABI defines the binary interface.
- we tend to call a particular ABI by its machine name such as *Alpha*, or *x86-64*.
	- Thus the ABI is a func of both the OS(say, Linux) and the architecture(say, x86-64)
- ABI is enforced by the *toolchain*-the compiler, the linker, and so on.
- the ABI is defined and implemented by the kernel and the toolchain.
# Standards
- Linux doesn't officially comply with any of numerous Unix standards.
- Instead Linux *aims* 2 most key prevalent standards: POSIX and the Single UNIX Specification(SUS).
## POSIX and SUS History
- the IEEE standardized system-level interfaces on Unix systems in the mid-1980s.
- Richard Stallman named POSIX(*phaz-icks*).
> The first result of this effort, issued in 1988, was IEEE Std 1003.1-1988 (POSIX 1988, for short). In 1990, the IEEE revised the POSIX standard with IEEE Std 1003.1-1990 (POSIX 1990). Optional real-time and threading support were documented in, respectively, IEEE Std 1003.1b-1993 (POSIX 1993 or POSIX.1b), and IEEE Std 1003.1c-1995 (POSIX 1995 or POSIX.1c). In 2001, the optional standards were rolled together with the base POSIX 1990, creating a single standard: IEEE Std 1003.1-2001 (POSIX 2001). The latest revision, released in December 2008, is IEEE Std 1003.1-2008 (POSIX 2008). All of the core POSIX standards are abbreviated POSIX.1, with the 2008 revision being the latest.

- In the late 1980s and early 1990s, Unix system vendors were engaged in the "Unix Wars".
- SUS subsumes the POSIX, so author will mention when syscall and other interfaces are standardized by POSIX.
# C Language Standards
- *K and R C* is developed around 1978.
- *ANSI C* was completed in 1989.
- ISO ratified *ISO C90* in 1990.
	- based ANSI C with small modification.
- *ISO C95* in 1995.
	- updated although rarely implemented.
- *ISO C99* in 1999.
	- large update including inline func, new data types, variable-length arrays, C++-style comments and new lib funcs
- latest ver is *ISO C11*.
	- significant feature of which is a formalized memory model
		- enabling the portable use of threads across platforms.
- on the C++ front, ISO standardization was slow.
- `ISO C98` was ratified in 1998.
	- improved compatibility across compilers.
- *ISO C++03* arrived in 2003.
- *C++ 11* heralded numerous language and standard lib additions and improvements.
	- many say C++11 is a distinct language from previous C++ revisions.
## Linux and the Standards
- Linux aims toward POSIX and SUS. It provides the interface doc in SUSv4 and POSIX 2008.
	- real-time (POSIX.1b), threading(POSIX.1c).
- Linux is believed to comply with POSIX.1 and SUSv3.
	- but no official certification.
- The *gcc* C compiler is ISO C99-compliant.
	- support for C11 is ongoing.
- The *g++* * C++ compiler is ISO C++03-compliant.
	- C++11 in development.
- *GNU C* are *gcc g++__* implement extensions.
- Linux hasn't had a great history of forward compatibility.
	- Interfaces documented by standards will remain source compatible.
	- Binary compatibility is ok across major ver of *glibc.*
	 - C is standardized.
	 - *gcc* will always compile legal C.
	 - Linux kernel guarantees the stability of syscalls, most importantly.
- LSB(Linux Standard Base) standardizes Linux system.
	- It extends POSIX and SUS.
## This Book and the Standards
- the book avoids paying lip service to any of the standards.
- modern Linux system
	- the Linux kernel(3.9), gcc(4.8), C library(2.17)
# Concepts of Linux Programming
- all Unix, Linux systems provide a mutual set of abstractions and interfaces.
	- these commonalities *define* Unix.
- This is the foundation of Linux system programming.
	- not an overview of Linux or programming env.
## Files and the Filesystem
- the file is the most basic and fundamental abstraction in Linux.
- Linux follows the *everything-is-a-file* philosophy.
	- system called Plan 9 is not.
		- born of Bell Labs.
		- called successor to Unix.
		- features innovative ideas.
			- adherent of the *everything-is-a-file* philo.
- even when the obj in question is not what you would <u>consider</u> a normal file.
### Regular files
- Linux doesn't enforce a structure beyond the byte stream at the system level.
	- system called VMS, provide highly structured files
		- support concept such as *records*.
			- Linux doesn't.
- it can write beyond the file offset.
	- padded with zeros.
- it can't write before the file offset.
- the kernel doesn't impose concurrent file access
	- when procs share single fd.
	- though it's unpredictable.
- i-node(*information node*, originally) is assigned an int called *i-node number*, *i-number, i-no*.
	- store metadata such as
		- modification timestamp, owner, type, length, the location of file's data.
		- but no filename.
	- i-node is both,
		- physical object located on disk in Unix-style filesystem.
		- conceptual entity, a data structure in the Linux kernel.
### Directories and links
- accessing a file via its inode number is cumbersome.
	- and also a potential security hole.
- Directory is a mapping table.
	- it has own inode.
	- thus it can recursively include directory and files.
- the kernel hops *directory entry, dentry* in the pathname to find the inode.
	- called *directory or pathname resolution*
	- there are *dentry cache*.
- the kernel use only add and remove links using special set of syscalls, bc it's sensible operations.
### Hard links
- multiple name resolve to the same i-node is allowed.
	- it's called *hard links*
- delete a file involves *unlinking*
	- removing its name and inode pair from a dir.
- it's uncertain, so there are *link count*.
	- if *link count* = zero, it's truly removed from the filesystem.
### Symbolic links
- hard link can't span filesystems, bc an i-node number is meaningless outside of the inode's own filesystem.
- symlinks(symbolic links) look like regular files.
	- has its own inode and data chunk.
		- contains the complete pathname of the linked-to file.
	- point to anywhere
		- different filesystems
		- non-existent file (*broken link*)
- symlink has overhead, since involving 2 files.
- symlink are opaque than hard links.
	- require special syscalls
		- it's a positive thing
			- as the link structure is explicitly made plain.
			- with symbolic links acting more as *shortcuts* than as filesystem-internal links.
### Special files
- they are kernel objects represented as files.
- Unix systems have a handful, Linux supports 4.
	- block device files
	- character device files
	- named pipes
	- and Unix domain sockets
- Linux provides syscall to create a special file
- Device access is via device files.
	- character devices and block devices.
		- character devices
			- linear queue of bytes.
			- keyboard is one example.
		- block device
			- array of bytes
			- user space access seekable device in any order.
			- generally storage device.
				- Hard disks, floppy drives, CD-ROM, flash memory.
		- Named pipes (often called FIFOS)
			- IPC that provides a communication channel over a fd.
			- regular pipe, they use memory via a syscall.
			- named pipes are accessed via a file.
				- called *FIFO special file*
			- Unrelated processes can access the file.
		- Sockets are an advanced form of IPC.
			- communication b/w procs, and also machines.
			- there are multiple varieties.
				- Unix domain socket
					- within the local machine.
					- special file called a socket file
				- over the Internet,
					- sockets might use a hostname and port pair.
### Filesystems and namespaces
- Linux , like all Unix systems, provides a global and unified *namespace* of files and dirs.
	- unified means, we can access floppy alongside from other media.
- a *filesystem* is a collection of files and dirs in a formal and valid hierarchy.
- first filesystem mounted is `/`, which is called the *root filesystem*.
- there are *virtual filesystems, network filesystems* other than usual filesystems.
- physical filesystems reside on block storage devices.
	- media-specific (ISO9600)
	- other Unix systems (XFS)
	- non-Uni systems(FAT)
- the smallest unit addressable on a block is the *sector*.
	- powers-of-2, with 512 bytes being common.
- the smallest logically addressable unit on a filesystem is the *block*.
- The block is an abstraction of the filesystem.
	- a block is usually a power-of-2 multiple of the sector size.
	- sector < block < page
		- page is the smallest unit addressable by the *MMU*
	- common size are 512 bytes, 1 kbytes , 4kbytes.
- Historically, Unix systems have only a single shared namespaces
	- viewable by all users and all procs
- Linux supports *process namespaces*
	- each proc has unique view
	- each proc inherits the namespace of its parent.
	- may create own namespace with mount point and a unique root directory
## Processes
- if files are the most fundamental abstraction in a Unix system, processes are the runner up.
- Procs are object code in exec, but they are more than that.
	- consists of data, resources, state, and a <u>virtualized computer</u>.
 - most common Linux format is *Executable and Linkable Format(ELF)* consists of *sections*.
	 - sections are linear chunks of obj code that load into linear chunks of memory.
	  - *text section*
		  - executable code and read-only data
			  - constant vars.
		  - typically read-only and executable
	  - *data section*
		  - initialized data
			  - C variables with defined values.
			  - typically readable and writable
	  - *bss section*
		  - uninitialized global vars.
			  - it's simply listed.
			  - bc C set 0 to all.
		  - the kernel map *zero page*(page of zeros).
		  - stands for *block started by symbol*
	  - *absolute section*
		  - non-relocatable symbols.
	  - *undefined section*
		  - a catchall.
  - Process request and manipulate resources
	  - through syscalls.
	  - resources are
		  - timers, pending signals, open files, network connections, hardwares, IPC.
		  - stored inside the kernel
			  - in the proc's *process descriptor*
  - A process is a virtualization abstraction
	  - the Linux kernel provides proc both
		  - a virtualized processor
		  - a virtualized memory
	  - from the processor's view, the view of the system is alone.
	  - same with memory.
  - the kernel allowing OS to concurrently manage the state of multiple independent procs.
## Threads
- each proc consists of one or more *threads of execution*.
- most procs consist of single thread(*single-threaded*)⇔*multithreaded*
- Traditionally, Unix programs have been single-threaded.
	- Unix's historic simplicity
	- fast proc creation times
	- robust IPC mechanisms
- A thread consists of
	- a *stack*
		- stores its local vars.
	- processor state
	- a current location in the object code
		- usually in the processor's *instruction pointer*
	- majority of the remaining parts of a proc are shared among all threads.
		- most notably the proc addr space.
- threads share the virtual memory abstraction while maintaining the virtualized processor abstraction
- the Linux kernel implements a unique view of threads:
	- they are simply normal processes that happen to share some resources.
- In user space, Linux implements threads in accordance with POSIX 1003.1c
	- known as *Pthreads*
- current Linux thread implementation
	- part of *glibc*
	- *Native POSIX Threading Library (NPTL)*
	- See [Chapter 7]
## Process hierarchy
- first proc is pid 1.
- procs form a strict hierarchy, *process tree* in Linux
	- root is *init process*
		- typically the *init* program
- `fork()` creates new proc.
	- every proc has a parent
		- except the first
		- if terminated, the kernel will *reparent* the child to the init process.
- when a proc terminates, first the kernel keeps parts of the proc resident in memory.
	- to allow the proc's parent to inquiry
		- about its status upon terminating
	- this inquiry is known as *waiting on* the terminated proc.
	- a *zombie* is a proc
		- terminated
		- but hasn't yet been waited upon
	- the init proc routinely waits on all of its children to reparented procs don't remain zombies forever.
## Users and Groups
- each proc is in turn associated with exactly one uid.
	- *real uid*
- Inside the Linux kernel, the uid is the only concept of a user.
- Users, however, refer to themselves and others through *usernames*, not numerical values.
- usernames and uid table is in */etc/passwd*
- *login* program
	- will spawns the user's *login shell* defined in */etc/passwd*
	- make the shell's uid equal to the user's.
	- child procs inherit the parents'.
- root's uid is 0.
	- can change a proc's uid.
	- *login* program runs as root.
- each proc has an *effective uid*,a *saved uid*, a *filesystem uid*.
- real uid is always the user who started the proc.
- effective uid may change.
- saved uid stores the original effective uid.
	- decide what effective uid values the user may change.
- filesystem uid usually equal to the effective uid
	- verify filesystem access
- Each user belongs to group(s), such as *primary* or *login group* listed in */etc/passwd*.
	- or possibly *supplemental groups* in */etc/group*
- each proc is therefore associated with a corresponding *group ID (gid)*.
	- *real gid, effective gid, saved gid*.
- procs are generally associated with a user's login group, not any of the supplemental groups.
- Unix is very simple.
	- proc with uid 0 had access.
- Linux is a more general *capabilities* system
	- instead of simple binary check
## Permissions
Linux has the same permi & security mechanism with Unix.
- execute permi is ignored on special files.
- for dirs.
	- read allows to read contents.
	- write allows new link
	- execute allows the dir to be entered and used in a pathname
- Linux supports access control lists(ACLs)
	- in addition to historic Unix permi.
	- ACLs allow for much more detailed
		- at the cost of
			- increased complexity
			- on-disk usage
## Signals
- one-way asynchronous notifications.
- the kernel → a proc, a proc → a proc, or a proc to itself.
- signals typically alert a proc to some event, such as segfault or \<C-c\>
- the Linux kernel has about 30 signals.
- Dut to signals' asynchrony
	- signal handlers must not stomp.
		- by executing only *async-safe(signal-safe)* func
## Interprocess Communication
- The Linux kernel implements most of historic Unix IPC
	- including defined by both System V and POSIX
- IPC mechanisms
	- pipes, named pipes, semaphores, message queues, shared memory, and futexes.
## Headers
- both the kernel itself and *glibc* provide them.
	- the standard C fare `<string.h>`
	- the usual Unix offerings `<unistd.h>`
## Error Handling
- return value(usually -1) and modifiable lvalue, `errno`
- *glibc* provides `errno` support both for library and syscalls.

| Error Code | Description                                     |
|------------|-------------------------------------------------|
| E2BIG      | Argument list too long                          |
| EACCES     | Permission denied                               |
| EAGAIN     | Try again                                       |
| EBADF      | Bad file number                                 |
| EBUSY      | Device or resource busy                         |
| ECHILD     | No child processes                              |
| EDOM       | Math argument outside of the domain of function |
| EEXIST     | File already exists                             |
| EFAULT     | Bad address                                     |
| EFBIG      | File too large                                  |
| EINTR      | System call was interrupted                     |
| EINVAL     | Invalid argument                                |
| EIO        | I/O error                                       |
| EISDIR     | Is a directory                                  |
| EMFILE     | Too many open files                             |
| EMLINK     | Too many links                                  |
| ENFILE     | File table overflow                             |
| ENODEV     | No such device                                  |
| ENOENT     | No such file or directory                       |
| ENOEXEC    | Exec format error                               |
| ENOMEM     | Out of memory                                   |
| ENOSPC     | No space left on device                         |
| ENOTDIR    | Not a directory                                 |
| ENOTTY     | Inappropriate I/O control operation             |
| ENXIO      | No such device or address                       |
| EPERM      | Operation not permitted                         |
| EPIPE      | Broken pipe                                     |
| ERANGE     | Result too large                                |
| EROFS      | Read-only filesystem                            |
| ESPIPE     | Invalid seek                                    |
| ESRCH      | No such process                                 |
| ETXTBSY    | Text file busy                                  |
| EXDEV      | Improper link                                   |

```c
#include <stdio.h>
void perror (const char *str);

if (close (fd) == -1)
	perror("close");

#include <string.h>
char *strerror (int errnum);

#include <string.h>
strerror_r (int errnum, char *buf, size_t len);
```
- the 2nd func returns pointer to str describing the error.
	- can change by `perror(), strerror()`
	- thus not thread-safe
- the `strerror_r()` func is thread-safe.
	- fills the buffer of `len` pointed at by `buf`
	- humorously set `errno` when error.
- common mistake is any library or syscall will override it
```c
if (fsync (fd) == −1){
	const int err = errno;// this is needed
	fprintf (stderr, "fsync failed: %s\n", strerror (errno));
	if (err == EIO){
		/* if the error is I/O-related, jump ship */
		fprintf (stderr, "I/O error on %d!\n", fd);
		exit (EXIT_FAILURE);
	}
}
```
- in single-threaded prog, `errno` is a global variable.
- in multithreaded prog, `errno` is stored per-thread,
	- and thus thread-safe.
## Getting Started with System Programming



