# RISC-V Notes

My concise notes with a practical approach to programming in RISCV

## 1 Foundations and First Basic Instruction

Registers store numbers. They are similar to variables in C.

Example:

```asm
t0 = 5
t1 = 10
```

But in RISC-V, you do NOT declare registers like in C.
The CPU already has them; we just use them.

```asm
li t0, 5
```

Common registers:

* `t0–t6` → temporary registers
* `s0–s11` → saved registers
* `a0–a7` → argument/function registers

---

### 1.1 `li` Load Immediate

`li` = Load Immediate

immediate values are numerical constants written directly within the assembly instructions

Syntax:

```asm
li destination, value
```

Example:

```asm
li t0, 7
```

Meaning:

```asm
t0 = 7
```

The register `t0` now contains the value `7`.

---

### 1.2 `addi` Add Immediate

Syntax:

```asm
addi destination, source, value
```

Example:

```asm
addi t0, t0, 3
```

Meaning:

```asm
t0 = t0 + 3
```

So the current value of `t0` is increased by `3`.

Example:

```asm
li t0, 5
addi t0, t0, 3
```

Execution:

* First line → `t0 = 5`
* Second line → `5 + 3 = 8`

Final value:

```asm
t0 = 8
```

---

### 1.3 `add` Add Two Registers

Syntax:

```asm
add destination, source1, source2
```

Example:

```asm
add t0, t1, t2
```

Meaning:

```asm
t0 = t1 + t2
```

Example:

```asm
li t1, 4
li t2, 6
add t0, t1, t2
```

Final value:

```asm
t0 = 10
```

---

### Sequential Execution and Comparison with C

The CPU executes one instruction at a time.

When reading assembly, it is usually better to simulate the program line by line instead of reading everything together.

Example:

```asm
li t0, 5
addi t0, t0, 3
addi t0, t0, 2
```

Execution:

* `t0 = 5`
* `t0 = 8`
* `t0 = 10`

---

C code:

```c
int x = 5;
x = x + 3;
x = x + 2;
```

RISC-V equivalent:

```asm
li t0, 5
addi t0, t0, 3
addi t0, t0, 2
```

Same logic, but at a lower level.

---

### Other Important Registers

* `zero` → always contains `0` and cannot be modified
* `t0–t6` → temporary values
* `s0–s11` → saved values
* `a0–a7` → arguments and function return values
* `ra` → return address
* `sp` → stack pointer

---

## 2 Comparisons and Branches

Normally, instructions are executed from top to bottom.

But now we introduce decisions, jumps, conditions, and skipping code.  
This is exactly how `if`, `else`, loops, and conditions work internally.

---

### 2.1 Branches and Labels

Branch instructions check if a condition is true.

- If the condition is true → the CPU jumps somewhere else
- If the condition is false → execution continues normally

To understand branches, we first need labels.

Labels are positions in code.

Examples:
```asm
START:
LOOP:
END:
```

A label is simply a name for a location in the program.

---

#### `beq` — Branch if Equal

Syntax:
```asm
beq reg1, reg2, LABEL
```

Meaning:
```asm
if reg1 == reg2
    jump to LABEL
```

Example:
```asm
li t0, 5
li t1, 5

beq t0, t1, END

addi t0, t0, 10

END:
addi t0, t0, 1
```

Execution:
- `t0 = 5`
- `t1 = 5`
- `5 == 5` is true
- The CPU jumps to `END`
- `addi t0, t0, 10` gets skipped
- Execution continues at `END`

Final result:
```asm
t0 = 6
```

---

#### `bne` — Branch if Not Equal

Syntax:
```asm
bne reg1, reg2, LABEL
```

Meaning:
```asm
if reg1 != reg2
    jump to LABEL
```

Example:
```asm
li t0, 2
li t1, 5

bne t0, t1, SKIP

addi t0, t0, 10

SKIP:
addi t0, t0, 1
```

Execution:
- `2 != 5` is true
- The jump happens
- `addi t0, t0, 10` gets skipped
- Execution continues at `SKIP`

Final result:
```asm
t0 = 3
```

---

#### `blt` — Branch if Less Than

Syntax:
```asm
blt reg1, reg2, LABEL
```

Meaning:
```asm
if reg1 < reg2
    jump to LABEL
```

Example:
```asm
li t0, 2
li t1, 5

blt t0, t1, SKIP

addi t0, t0, 100

SKIP:
addi t0, t0, 1
```

Execution:
- `2 < 5` is true
- The jump happens
- `addi t0, t0, 100` gets skipped

Final result:
```asm
t0 = 3
```

---

#### `bge` — Branch if Greater or Equal

Syntax:
```asm
bge reg1, reg2, LABEL
```

Meaning:
```asm
if reg1 >= reg2
    jump to LABEL
```

Example:
```asm
li t0, 7
li t1, 5

bge t0, t1, BIGGER

addi t0, t0, 100

BIGGER:
addi t0, t0, 1
```

Execution:
- `7 >= 5` is true
- The CPU jumps to `BIGGER`
- `addi t0, t0, 100` gets skipped

Final result:
```asm
t0 = 8
```

---

### 2.2 Signed vs Unsigned Numbers

We must remember that there are two ways to interpret the same binary number.

The same binary sequence can represent different values depending on whether it is signed or unsigned.

Example (C2 Two's complement):

| Binary | Signed Meaning | Unsigned Meaning |
|---|---|---|
| `11111111` | `-1` | `255` |
| `11111001` | `-7` | `249` |

- **Signed** numbers can represent negative values
- **Unsigned** numbers are always positive

This is important for the following branch instructions.

---

#### `bltu` — Branch if Less Than Unsigned

`bltu` compares numbers as **unsigned** values.

Syntax:
```asm
bltu reg1, reg2, LABEL
```

Meaning:
```asm
if reg1 < reg2 (unsigned comparison)
    jump to LABEL
```

Example:
```asm
li t0, -1
li t1, 5

bltu t0, t1, SMALL

addi t0, t0, 1

SMALL:
addi t1, t1, 1
```

Important:
- `-1` in binary is interpreted as a very large positive number in unsigned mode
- So unsigned comparison changes the result

As unsigned:
```asm
4294967295 > 5
```

So the jump does NOT happen.

Execution continues normally.

---

### 2.3 Note About Labels and Branches

Labels are only positions in code.

A label itself does not execute anything.

Example:
```asm
LOOP:
```

This line only marks a location.

The branch instruction either:
- jumps there
- or does not jump there

We do not "execute a label".

---

Branches are the foundation of:
- `if`
- `else`
- loops
- conditions
- function control flow

At low level, programs work mostly by:
- comparing values
- deciding where to jump next
- skipping or repeating instructions

---

## 3 Loops

In RISC-V, loops are very primitive to implement.

`while`, `for`, infinite loops, and repeated execution are all built using:
- labels
- jumps
- branches

---

### 3.1 Labels (Revisited)

To jump somewhere in the program, we use labels.

Example:
```asm
LOOP:
```

This creates a location called `LOOP`.

The CPU can now jump there.

Keep in mind:
- labels are NOT instructions
- they are only position markers
- `LOOP:` itself does not execute anything

Example:
```asm
li t0, 1

LOOP:
addi t0, t0, 2
```

Here:
- `LOOP:` does nothing
- only `addi` gets executed

---

### 3.2 The `j` Instruction

`j` = jump

Syntax:
```asm
j LABEL
```

Meaning:
```asm
go to LABEL immediately
```

This is an unconditional jump:
- no comparison
- no condition
- the CPU always jumps

Example:
```asm
li t0, 2

j END

addi t0, t0, 100

END:
addi t0, t0, 2
```

Execution:
- `t0 = 2`
- `j END` immediately jumps to `END`
- `addi t0, t0, 100` gets skipped
- `addi t0, t0, 2` gets executed

Final result:
```asm
t0 = 4
```

---

First Real Loop

Example:
```asm
li t0, 0

LOOP:
addi t0, t0, 1
j LOOP
```

This is an infinite loop.

Nothing stops it.

Execution:
- `t0` starts at `0`
- `addi` increases it by `1`
- `j LOOP` jumps back to the label
- the process repeats forever

So:
```asm
0 → 1 → 2 → 3 → 4 → ...
```

---

A real useful loop usually needs:
- repeated execution
- a stopping condition

In assembly:
- `j` is used to repeat
- branches are used to stop the loop

---

First Controlled Loop

Example:
```asm
li t0, 0
li t1, 3

LOOP:
addi t0, t0, 1

beq t0, t1, END

j LOOP

END:
```
---

Execution:

Initial values
```asm
t0 = 0
t1 = 3
```

First iteration
```asm
t0 = 1
```

Check:
```asm
1 == 3 → false
```

So:
```asm
j LOOP
```

The loop repeats.

---

Second iteration
```asm
t0 = 2
```

Check:
```asm
2 == 3 → false
```

Loop repeats again.

---

Third iteration
```asm
t0 = 3
```

Check:
```asm
3 == 3 → true
```

The branch jumps to `END`.

The loop stops.

Final result:
```asm
t0 = 3
```

---

Another Example

Example:
```asm
li t0, 10

LOOP:
addi t0, t0, -1

beq t0, zero, END

j LOOP

END:
```

What this loop does:
- starts from `10`
- subtracts `1` every iteration
- stops when `t0` becomes `0`

Execution:
```asm
10 → 9 → 8 → 7 → ... → 1 → 0
```

When `t0 == 0`:
```asm
beq t0, zero, END
```

becomes true, so the loop stops.

---

## 4. Memory, Addresses and Arrays

Until now we have mostly used registers, but they're small and limited in number, real programs need much more space for:
- arrays
- strings
- large data structures

That is why understanding memory (RAM) is important.

Registers directly hold values, but memory uses addresses.

An address is the location of data in memory.

---

### 4.1 `lw` — Load Word

`lw` = Load Word

This instruction reads memory into a register.

Syntax:
```asm
lw destination, offset(base_register)
```

Example:
```asm
lw t0, 0(s0)
```

Meaning:
```asm
read memory at address:
s0 + 0
```

Then copy the result into `t0`.

---

**Word Size**

In RV32:
```asm
1 word = 4 bytes
```

So:
```asm
lw
```

loads 4 bytes from memory.

---

Example

Suppose:
```asm
address 0x3E8 contains value 25
```

And:
```asm
s0 = 0x3E8
```

Code:
```asm
lw t0, 0(s0)
```

Address calculation:
```asm
0x3E8 + 0 = 0x3E8
```

Memory read:
```asm
memory[0x3E8] = 25
```

Now:
```asm
t0 = 25
```

So `lw` basically means:
1. go to memory
2. read data
3. copy data into a register

---

### 4.2 `sw` — Store Word 

`sw` = Store Word

This instruction writes a register value into memory.

Syntax:
```asm
sw source, offset(base_register)
```

Example:
```asm
sw t0, 0(s0)
```

Meaning:
```asm
store t0 into memory at:
s0 + 0
```

---

Example

Suppose:
```asm
t0 = 50
s0 = 1000
```

Code:
```asm
sw t0, 0(s0)
```

Address calculation:
```asm
1000 + 0 = 1000
```

Store operation:
```asm
memory[1000] = 50
```

Now memory contains:
```asm
address 1000 → value 50
```

---

### Direction of Data and Offset

`lw`
```asm
memory -> register
```

`sw`
```asm
register -> memory
```

Registers are used to work with data, while memory is used to store larger amounts of data.

---

Understanding the Offset

Example:
```asm
lw t0, 4(s0)
```

Meaning:
```asm
read from address:
s0 + 4
```

The offset is added to the base address.

So:
- `0(s0)` → first memory position
- `4(s0)` → next word
- `8(s0)` → next word again

Because:
```asm
1 word = 4 bytes
```

Example:
```asm
s0 = 1000
```

Then:
```asm
0(s0) -> 1000
4(s0) -> 1004
8(s0) -> 1008
```

This is exactly how arrays are accessed in memory.

---

### 4.3 Arrays in Memory

Suppose we have (in C):

```c
int arr[3] = {10, 20, 30};
```

Each integer in RV32 uses 4 bytes, so in memory it looks like this:

| Address | Value |
|---|---|
| 1000 | 10 |  arr[0]
| 1004 | 20 |  arr[1]
| 1008 | 30 |  arr[2]

Addresses increase by 4 because each integer is 4 bytes.

---

### Base Address

Suppose:
```asm
s0 = 1000
```

This means:
```asm
s0 points to arr[0]
```

So `s0` contains the base address of the array.

---

#### 4.3.1 Accessing Arrays

Accessing `arr[0]`

Code:
```asm
lw t0, 0(s0)
```

Address calculation:
```asm
1000 + 0 = 1000
```

Read:
```asm
10
```

---

Accessing `arr[1]`

Code:
```asm
lw t0, 4(s0)
```

Address calculation:
```asm
1000 + 4 = 1004
```

Read:
```asm
20
```

---

`arr[2]` would be:
```asm
lw t0, 8(s0)
```

Because for integer arrays:
```asm
offset = index * 4
```

---

#### 4.3.2 Writing into Arrays

Suppose:
```asm
t0 = 99
s0 = 1000
```

Code:
```asm
sw t0, 4(s0)
```

Address calculation:
```asm
1000 + 4 = 1004
```

This stores:
```asm
memory[1004] = 99
```

Now the array becomes:

| Address | Value |
|---|---|
| 1000 | 10 |  arr[0]
| 1004 | 99 |  arr[1]
| 1008 | 30 |  arr[2]

We need to keep in mind that:
```asm
s0 contains the address of the array,
not the array value itself
```

---

## 5. Program Structure, Directives and System Basics

Extra note:

In assembly we can leave comments in the code using:
```asm
# single line comment
```

or:
```c
/* multi line
   comment */
```

---

`x0` / `zero` is hardwired to `0` and cannot be modified.

Registers `a0–a7` are used for:
- function arguments
- return values

Registers `t0–t6` are caller-saved registers.
They can be overwritten by called functions.

If you only need a register temporarily inside a function, use `t0–t6`.

Registers `s0–s11` are callee-saved registers.
Called functions must preserve their values.

If you need a value to survive across function calls, use `s0–s11`.

---

### 5.1 Symbols and Directives

The developer or the compiler can create symbols.

A symbol is a name associated with:
- a numerical constant
- an address
- or a position in the program

One common directive is:
```asm
.set
```

Example:
```asm
.set max_temp, 95   # create the symbol max_temp with value 95

li t1, max_temp     # load the symbol value into t1
```

Labels that we saw before are also a special type of symbol because they mark positions in code.

---

Some common directives:

| Directive | Meaning |
|---|---|
| `.text` | code section |
| `.data` | initialized data section |
| `.bss` | uninitialized data section |
| `.globl` | make a symbol visible to the linker |
| `.equ` | associate a symbol with a numerical constant |
| `.set` | create a custom symbol |

---

#### `.data`

Here we can define initialized data like:
- strings
- arrays
- variables with known values

These values are ready when the program starts.

Example:
```asm
.data

x:     .byte 10          # 8-bit variable = 10
y:     .half 300         # 16-bit variable
z:     .word 546         # 32-bit variable

text:  .ascii "Hello\0"  # string with null terminator

array: .word 1,2,3,4,5  # array of 5 integers
```

---

#### `.globl`

`.globl` declares a symbol as global.

This means other files can access it during linking.

It is commonly used for:
- functions
- global variables

Example:
```asm
.globl main     # make main globally visible
.globl x        # make x visible to other files

.data
x: .word 10     # global initialized variable

.text
main:
la a0, x         # load x address
lw a1, 0(a0)     # read the value

ret
```

---

#### `.bss`

`.bss` is used for uninitialized data.

Variables declared here reserve memory space, but no initial value is stored in the executable.

This is useful for:
- buffers
- large arrays
- variables initialized later

Example:
```asm
.bss

buffer: .space 64     # reserve 64 bytes
array:  .space 40     # reserve space for 10 integers
```

In RV32:
```asm
10 integers * 4 bytes = 40 bytes
```

So `.space 40` reserves enough memory for 10 integers.

--- 

#### `.equ`

The directive `.equ` (equate) is used to define a symbol and assign a numerical constant to it.

Example:
```asm
.equ SIZE, 200
```

Every time the assembler meets `SIZE` in the code, the value will be interpreted as `200`.

This can make the code:
- more readable
- easier to maintain

The symbol created with `.equ` usually should not be redefined later.

This is similar to `#define` in C.

With `.equ` we do NOT physically allocate memory.

---

### 5.2 Input/Output (I/O) and System Calls

RV32I does not directly include input/output operations.

Usually the operating system provides an interface for:
- printing text
- reading input
- exiting programs
- and other services

These operations are executed using system calls through the instruction:
```asm
ecall
```

`ecall` means:
```asm
environment call
```

To execute a system call:
- register `a7` must contain the system call number
- arguments are usually stored in `a0`

---

Most common operations:

| `a7` | Function | Registers / Notes |
|---|---|---|
| `1` | print integer | `a0 = integer to print` |
| `4` | print string | `a0 = address of string` |
| `5` | read integer | result returned in `a0` |
| `8` | read string | `a0 = buffer address`, `a1 = max size` |
| `10` | exit program | terminate execution |
| `11` | print character | `a0 = character` |
| `12` | read character | result returned in `a0` |

---

Basic example:
```asm
.data
msg:
    .asciiz "Helloo!"    # string to print

.text

main:
    la a0, msg           # load string address into a0
    li a7, 4             # system call code for print_string
    ecall                # execute the call

    li a7, 10            # system call code for exit
    ecall                # terminate program
```

---

Another example: reading an integer from keyboard

```asm
.text

main:
    li a7, 5         # read integer
    ecall

    # returned value is now inside a0

    li a7, 1         # print integer
    ecall

    li a7, 10        # exit
    ecall
```

Execution:
- the program waits for keyboard input
- the integer is stored in `a0`
- the same value is printed back

---

#### System Call 10

To properly terminate a program in RV32I, it is necessary to invoke system call `10`.

Example:
```asm
li a7, 10
ecall
```

An exit value inside `a0` is optional, but it can be useful to indicate:
- success
- errors
- program status

---

### 5.3 `la` — Load Address

la = Load Address. la is a pseudo-instruction a shortcut for the assembler that translates it into real instructions

when we declare: 
```asm
.data
num1: .word 16
result: .word 0
```

num1 is an address, not a value. Before using lw, we need to know this address

```asm
la t0, num1        # t0 = address of num1
lw t1, 0(t0)       # t1 = value stored at memory address t0

la t2, result      # load address of 'result' in t2
sw t1, 0(t2)       # store t1 to the address pointed by t2
```

In this case la loads the address of num1 into register t0. 
In contrast, lw loads a value from memory.

### 5.4 `srai`

`srai` = Shift Right Arithmetic Immediate

This instruction shifts the bits to the right.

Syntax:
```asm
srai destination, source, shift_amount
```

Example:
```asm
srai t0, t0, 1
```

Meaning:
```asm
t0 = t0 >> 1 #shift bits to the right
```

Arithmetic right shift preserves the sign bit.

This is important for signed numbers because negative values remain negative after the shift.

A right arithmetic shift by 1 is similar to:
```asm
division by 2
```

Example:
```asm
li t0, 20

srai t0, t0, 1
```

Execution:
```asm
20 / 2 = 10
```

Now:
```asm
t0 = 10
```

Another example:
```asm
li t0, 40

srai t0, t0, 2
```

Execution:
```asm
40 / 4 = 10
```

Because:
```asm
right shift by 2 = divide by 2²
```

So in general:
```asm
srai by n ≈ division by 2ⁿ
```

## 6. Functions and Procedures in RISC-V

### 6.1 Types of Routine

- **Routine** → generic term that indicates a block of code that executes a task
- **Function** → a specific type of routine that accepts parameters and returns a value
- **Procedure** → another type of routine that accepts parameters but does not return values
- **Call site** → the position in the code where the routine gets called
- **Caller routine** → the routine that calls another routine
- **Callee routine** → the routine that gets invoked/called from the call site

In assembly, routines are defined by:
- a label
- a block of code

Invoking a routine consists of jumping to the entry point of the routine.

After the routine terminates, it is necessary to know the address to jump back to, so execution can continue after the call site.

Because of this, we need to save the return address to make sure the routine can return correctly to the caller.

The instruction:
```asm
jal
```

saves the return address inside the register:
```asm
ra
```

The return address is usually:
```asm
PC + 4
```

because it is the address of the next instruction after the call.

---

### ABI (Application Binary Interface)

The ABI is a set of conventions that defines how routines interact with the execution environment in a system.

It establishes rules for:
- how parameters are passed
- how values are returned
- how registers are managed
- how data is saved in memory
- how the memory layout is defined

---

In the RISC-V ILP32 ABI model, parameters are passed through dedicated registers:
```asm
a0, a1, ..., a7
```

So when a function gets called, the first 8 arguments are already stored in these registers in order.

If the function requires more than 8 arguments:
- the remaining arguments are placed on the stack

The caller must:
- reserve the necessary space on the stack
- insert the extra parameters in reverse order
- and only after that perform the routine jump

After the routine returns, the caller is also responsible for restoring the stack to its previous state.