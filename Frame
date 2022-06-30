## The Stack

In the context of low-level systems, the stack is a special region of runtime memory dedicated to the storage of local function variables and sensitive control flow metadata.


## Stack Frame

The stack is typically structured as a linear sequence of memory allocations known as stack frames. Each time a function is called, the stack will automatically allocate a new stack frame.

As the function executes, it will use the given stack frame to store and operate upon its local variables. Once the function returns, this memory will automatically get released back to the stack.

## Function Prologue

The first few assembly instructions found at the start of any function are known as the function prologue.

```nasm
some_function:
    push    rbp
    mov     rbp, rsp
    sub     rsp, 0x20 
    ...
```

The function prologue is inserted by the compiler to 'allocate' a stack frame for the function body to use. When necessary, the prologue will also save any volatile caller state to the stack.

## Stack Pointer (RSP)

The first instruction of the function prologue is a `push`. This is a special instruction used to save data to the top of the stack.

```nasm
push    rbp
```

Internally, this instruction performs two operations:

```nasm
sub     rsp, 0x8           ; 1. allocate 8 bytes
mov     qword [rsp], rbp   ; 2. store rbp to stack
```

This form exposes the use of `rsp`, a special register known as the stack pointer. Whatever memory address is held within `rsp` is interpreted by the CPU as the top of the stack.


## Base Pointer (RBP)

The x86 register `rbp` is dedicated to holding the base pointer of the current stack frame. The base pointer is simply a bookend used in assembly to tell where the current stack frame ends.

Since the function prologue is used to create a _new_ stack frame, it first must save the old base pointer held by `rbp`. The quickest way to do this is by pushing the value onto the stack.

```nasm
push    rbp       ; save old base pointer
```

By saving the value in `rbp` onto the stack, the prologue is now free to overwrite the register with a new base pointer.

```nasm
mov     rbp, rsp  ; set a new base pointer
```

## Allocating a Stack Frame

Allocating memory on the stack is incredibly fast because it can be done in a single instruction. To allocate a new stack frame, the program simply subtracts from the stack pointer.

```nasm
sub     rsp, 0x30   ; allocate 0x30 bytes on stack
```

This is possible because the memory directly above the stack pointer (lower in memory) is guaranteed to be unallocated.


## Function Epilogue

At the end of every function, the compiler will insert a few instructions which make up the function epilogue.

```nasm
some_function:
    ...
    leave   
    retn
```

Opposite to the function prologue, the role of the epilogue is to release the current stack frame and return execution to the caller.

## Releasing a Stack Frame

The `leave` instruction is used almost exclusively by function epilogues to release the current stack frame.

```nasm
leave
```

This instruction is an alias for the following sequence:

```nasm
mov     rsp, rbp   ; 1. release the current stack frame
pop     rbp        ; 2. restore the old base pointer
```

Remember, `rbp` points at the 'bottom' of the current stack frame. By setting the stack pointer (eg, `rsp`) to the current base pointer, the first step will effectively collapse the stack frame to nothing.

## Popping off the Stack

Where the `push` instruction is normally used to add an element to the top of the stack, the `pop` instruction will remove the top most element, placing it into the given operand.

```nasm
pop     rbp
```

Like the `push` instruction, `pop` can be exploded out:

```nasm
mov     rbp, qword [rsp]   ; 1. copy top element into rbp
add     rsp, 0x8           ; 2. de-allocate 8 bytes
```

In this case, the function epilogue is restoring the base pointer to its previous value as saved by the function prologue.
