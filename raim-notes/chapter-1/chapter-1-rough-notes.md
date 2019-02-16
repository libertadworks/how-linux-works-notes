# The Big Picture

## Levels and Layers of Abstraction in a Linux System
- There's user space and kernel space.
- User processes can destroy some things, but they're (usually) not that bad.
- Kernel processes can end everything, but this can (should) be rare.
- Abstractions are given so we can understand things more easily.
- Hardware is the lowest level. (User) Processes are the highest level.

## Hardware: Understanding Main Memory
- Main memory has different states.
- Physical representation of bits is called an image, not a state.

## The Kernel
The kernel is the core of the operating system. It does several tasks related to the following sections, and it runs most stuff in two spaces: kernel and user space.

### Process Management
The kernel makes sure what processes are being run, and when it changes a process, it does a "context switch", where it saves the state of such a process, and then loads the state of the new focus process. The context switch is when it changes from user mode to kernel mode and viceversa.

### Memory Management
The kernel handles the memory spaces for the kernel processes and the user processes. Each user process has its own space, and it cannot modify the space of another process (though some may be able to share data from one another). The memory is handled with a memory management unit (MMU) that the CPU has, following an implementation of memory mapping called a page table.

### Device Drivers and Management
The kernel manages devices, as devices **cannot** be handled in userspace. Doing so can be catrastophic, as the user asking to shut down a device could take down the entire system. However, user processes may handle pseudodevices that run in userspace. Drivers are considered kernel code, to ease the implementation of them.

### System Calls and Support
There are several actions that user processes cannot do on their own; this includes spawning new processes. To do this, system calls (or syscalls) are needed. Some of these are `fork()` and `exec()`. The former creates an almost exact copy of the running process, while the latter spawns a new program that is given as an argument. Even when using a pseudodevice, user processes _will_ need to realize syscalls for special operations.

## User Space
Userspace is where all the processes spawned by a user are ran. Userspace contains its own memory limits, and a user can edit files owned by him. Several applications, ranging from web browsers to databases, are ran in userspace, though even in userspace you may consider them in different layers, as one may depend on the other running as a server.

## Users
Users are based on UNIX users, with the permissions of file accesses and spawning of programs. A user may choose to share files with other users in a group or everyone, depending on the given permissions. If these permissions are not given, a user cannot do such things on "foreign" files. Only the `root` user, also known as the superuser, can bypass all this.