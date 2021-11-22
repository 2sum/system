## Process Vs Thread: ##

- Process is execution of Program, while thead is a semi-process.
- one or more Process creates via one or more system call, while multiple threads can be created via single system call
- Context switch between process is slower than threads.
- Process has its own stack, memory and data. While Threads onlu has their own stack, others they share

## Heap, Data, Text, Stack:
- Stack: All data needed by function call in a program.
- Heap: Dynamic memory allocation takes palce here, begins at end of BSS(Block started symbol) , grows upwards to higher memory address. brk(),sbrk() system call to adjust memory
- Block started symbol:
  - Un initilaized data segment, data initialized to 0, static int i will be allocated to BSS.
  - Data:  Contains initialized global, static variables which have predefined value and can't be modified its divided into read-only and read-write space
  - Text: M/c language instructions or code stored
