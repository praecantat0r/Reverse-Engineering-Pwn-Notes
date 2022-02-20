# ***Buffer Overflows***

## *Memory*
Memory is the place where your program is loaded and it's all the local variables, parameters, functions are stored here. The processor then reads instructions from memory and executes them.  
All variables in memory (on intel x86 processeors) are stored in **little endian**. In little endian, the bytes are stored in **reverse order.**  
Memory addresses are given in **hexadecimal**.  
> ![image](https://user-images.githubusercontent.com/86436966/154855073-2a62f35c-3b8a-4a20-81b5-0b8ea45a1215.png)

**Kernel**: The main layer between the **OS and hardware**, a core that provides basic services for all other parts of the OS. This part also containts command line variables and enviroment variables.  
**Stack:** The stack is a **section of memory that stores temporary data**, that is executed when a function is called.
It works on LIFO(last-in-first-out) principle and it grows **downward towards lower values.**  
**Heap:** Holds all the **dynamically allocated memory**. Whenever we use malloc to get memory dynamically, it is allocated from the heap. The heap grows upwards in memory(from lower to higher memory addresses) as more and more memory is required.  
**Text:** Contains program code to be executed. It is read only and any attempt to write in it will lead to memory violation.  
**Data:** Contains global information for program. The DATA section has Initialised variables and BSS section contains Uninitialised variables.  

## *Registers:*
Registers are **small amounts of high-speed memory contained within the CPU**. There are a fixed number of registers that are used for different purposes and they all have a specific location in the CPU.
Registers can hold **pointers which point to memory addresses containing certain instructions for the program to perform**, this can be exploited by using a jump instruction to move to a different memory location containing malicious code.
For example a register called **Instruction Pointer(RIP,EIP)** is used to keep track of what instruction to be executed next by the CPU.
Intel assembly has 8 general purpose and 2 special purpose 32-bit register.

| Register | Type | Purpose |
|-|-|-|
| EAX | General Purpose | Stores the return value of a function. |
| EBX | General Purpose | No specific uses, often set to a commonly used value in a function to speed up calculations. |
| ECX | General Purpose | Occasionally used as a function parameter and often used as a loop counter. |
| EDX | General Purpose | Occasionally used as a function parameter, also used for storing short-term variables in a function. |
| ESI | General Purpose | Used as a pointer, points to the source of instructions that require a source and destination. |
| EDI | General Purpose | Often used as a pointer. Points to the destination of instructions that require a source and destination. |
| EBP | General Purpose | Has two uses depending on compile settings, it is either the frame pointer or a general purpose register for storing of data used in calculations. |
| ESP | General Purpose | A special register that stores a pointer to the top of the stack (virtually under the end of the stack). |
| EIP | Special Purpose | Stores a pointer to the address of the instruction that the program is currently executing. After each instruction, a value equal to the its size is added to EIP, meaning it points at the machine code for the next instruction. |
| FLAGS | Special Purpose | Stores meta-information about the results of previous operations i.e. whether it overflowed the register or whether the operands were equal. |

Registers show here are 32-bit but they can also be 64-bit, 16-bit and 8-bit.  
For example, the EAX register is RAX in 64-bit, AX in 16-bit and AL in 8-bit.  

## *Buffer:*
Most programs usually take an input and process an output on basis of that through some specified functions. These strings and arrays are stored in buffer. So buffer holds up objects of same data type. This input can be taken from:
 - Data typed in a prompt or gui.
 - Data sent to program over a network.
 - Data provided in a file.
 - Data provided in variables.

The CPU reads the input until it reaches a **null character**, which tells it about the termination of string. The buffer is provided a specific amount of space in the memory.   As reading an array involves reading towards higher memory addresses, the buffer is allocated memory from lower towards higher memory addresses in most systems.  

## *Pointers:*
A pointer is, a variable that stores a memory address as its value, which will correspond to a certain instruction the program will have to perform. The value of the memory address can be obtained by **dereferencing** the pointer.

## *Instructions:*
| Instruction Type | Description | Example |
|-|-|-|
| Moving Data Around | Used to move values and pointers. | mov, lea |
| Doing Nothing | The NOP instruction, short for “no operation”, simply does nothing. | NOP |
| Math and Logic | Used for math and logic. Some are simple arithmetic operations and some are complex calculations. | add, sub,inc, dec, and |
| Jumping Around | Used mainly to perform jumps to certain memory locations , it stores the address to jump to. | jmp, jne, jnz, je, jz call, ret,cmp, test | 
| Manipulating the Stack | Used for adding and removing data from the stack. | push, pop | 

## *Buffer Overflow:*
A buffer can be compared to a water tank. You have a fixed amount of space to fill and if you fill more than that it overflows and the water leaks everywhere. This is what happens during a buffer overflow.  
If user input is supposed to be 1000 characters and you input 1500, the input crosses the buffer boundary and **overwrites the memory surrounding it**. This happens when there are no bound checks in place and the excess data is copied onto the buffer. If this happens it usually leads to a **Segmentation Fault**. A Segmentation Fault happens when you try to access areas of memory  which you aren't supposed to access.

We can use this concept to crash the system, corrupt memory and execute arbitrary code with the same privileges as the program we are exploiting. There are different types of buffer overflow.  
For example in a stack based buffer overflow we can use the buffer to overwrite the return address. The return address point to where the control should be returned after execution. We can replace that with an address of a CPU instruction like our shellcode which can spawn a shell in our listener.  
Shellcode is a set of CPU instruction used as the payload to gain our shell.  
