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

Until now we mostly used registers, but they're small and limited in number. Real programs need more space for arrays, strings, and large data structures, that's what memory (RAM) is for.

Registers hold values directly. Memory instead is accessed through **addresses**: a number that identifies a location in memory.

To move data between registers and memory, RISC-V uses **load** instructions (memory → register) and **store** instructions (register → memory).

---

### 4.1 The `offset(base_register)` Format

Every load/store instruction uses the same addressing format:

```asm
offset(base_register)
```

The CPU computes the real address like this:

```asm
address = base_register + offset
```

`base_register` usually holds a memory address (for example the start of an array), and `offset` is a constant added to it.

Example:
```asm
lw t0, 4(s0)
```
means: read memory at address `s0 + 4`, and put the result in `t0`.

This format is the same for every load and store instruction below, only the amount of data read/written changes (byte, halfword, or word).

---

### 4.2 `lw` and `sw` — Word Transfers (4 bytes)

In RV32, a **word is 4 bytes**. `lw` and `sw` move a full word.

```asm
lw destination, offset(base_register)   # memory -> register
sw source, offset(base_register)        # register -> memory
```

Example:
```asm
# s0 = 1000, and memory[1000] = 25

lw t0, 0(s0)     # t0 = 25   (read)
sw t0, 0(s0)     # memory[1000] = t0  (write)
```

Because a word is 4 bytes, consecutive words are 4 addresses apart:
```asm
0(s0) -> 1000
4(s0) -> 1004
8(s0) -> 1008
```

This is exactly how arrays are laid out in memory (see 4.4).

---

### 4.3 Byte and Halfword Transfers

Sometimes we don't need a full word, for example a `char` only takes 1 byte. RISC-V provides smaller load/store instructions:

| Instruction | Size | Direction | Notes |
|---|---|---|---|
| `lb` | 1 byte | memory → register | sign-extended |
| `lbu` | 1 byte | memory → register | zero-extended (unsigned) |
| `lh` | 2 bytes (halfword) | memory → register | sign-extended |
| `lhu` | 2 bytes (halfword) | memory → register | zero-extended (unsigned) |
| `sb` | 1 byte | register → memory | writes the low byte only |
| `sh` | 2 bytes (halfword) | register → memory | writes the low 2 bytes only |

Syntax is identical to `lw`/`sw`:
```asm
lb destination, offset(base_register)
sb source, offset(base_register)
```

**Why "sign-extended" vs "zero-extended"?**

A register is always 32 bits, but `lb`/`lh` only read 1 or 2 bytes from memory. The CPU must fill the remaining bits somehow:

- `lb` / `lh` → **sign-extended**: the remaining bits are filled with the sign bit (0 if positive, 1 if negative), so the signed value stays correct.
- `lbu` / `lhu` → **zero-extended**: the remaining bits are always filled with `0`, so the value is treated as unsigned (always positive).

Example: same byte, two different results:
```asm
# memory[0(s0)] contains the byte 0xFF (binary 11111111)

lb  t0, 0(s0)     # t0 = 0xFFFFFFFF  -> -1 as signed
lbu t1, 0(s0)     # t1 = 0x000000FF  ->  255 as unsigned
```

`lb` extends the sign bit across the whole register, `lbu` just pads with zeros, that's the entire difference.

Store example:
```asm
li t0, 65          # ASCII 'A'
sb t0, 0(s0)       # write only the lowest byte of t0 into memory[s0]
```

`sb`/`sh` never sign-extend or zero-extend: they only copy the low bits of the register into memory, so there's nothing to fill.

---

### 4.4 Arrays in Memory

Suppose we have, in C:
```c
int arr[3] = {10, 20, 30};
```

Each `int` in RV32 is a word (4 bytes), so consecutive elements are 4 bytes apart in memory:

| Address | Value |
|---|---|
| 1000 | 10 (`arr[0]`) |
| 1004 | 20 (`arr[1]`) |
| 1008 | 30 (`arr[2]`) |

If `s0 = 1000`, then `s0` is the **base address** of the array, it points to `arr[0]`. Any element can be reached with:
```asm
offset = index * 4
```

```asm
lw t0, 0(s0)     # t0 = arr[0] = 10
lw t0, 4(s0)     # t0 = arr[1] = 20
lw t0, 8(s0)     # t0 = arr[2] = 30
```

Writing works the same way:
```asm
li t0, 99
sw t0, 4(s0)     # arr[1] = 99
```

Remember: `s0` holds the address of the array, not the array itself. For arrays of `char` (1 byte each) the same logic applies with `lb`/`sb` and `offset = index * 1`; for `short` (2 bytes) it's `lh`/`sh` with `offset = index * 2`.

---

**Example: Building an Array with a Loop**

Build `int arr[5] = {0, 1, 2, 3, 4}` starting at `s0 = 1000`:

```asm
li t0, 0      # current value
li t1, 5      # number of elements

LOOP:
sw t0, 0(s0)      # store value into array

addi s0, s0, 4    # move to next array element
addi t0, t0, 1    # next value

addi t1, t1, -1   # one element completed
bne t1, zero, LOOP
```

Each iteration writes one value, then advances `s0` by 4 (next word) and increments `t0` (next value), until `t1` reaches `0`.

Final memory:

| Address | Value |
|---|---|
| 1000 | 0 (`arr[0]`) |
| 1004 | 1 (`arr[1]`) |
| 1008 | 2 (`arr[2]`) |
| 1012 | 3 (`arr[3]`) |
| 1016 | 4 (`arr[4]`) |

This is a common pattern in assembly: one register holds the current value, one register points to the current array element, one register controls the loop, and the store instruction writes into memory while the pointer advances by the element size (4 for a word, 2 for a halfword, 1 for a byte).

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

Syntax:
```asm
la destination, symbol
```

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

### 5.5 `slli`

`slli` = Shift Left Logical Immediate

This instruction shifts the bits to the left.

Syntax:
```asm
slli destination, source, shift_amount
```

Example:
```asm
slli t0, t0, 1
```

Meaning:
```asm
t0 = t0 << 1
```

A left shift by 1 is similar to:
```asm
multiply by 2
```

Example:
```asm
li t0, 5

slli t0, t0, 1
```

Execution:
```asm
5 * 2 = 10
```

Now:
```asm
t0 = 10
```

Another example:
```asm
li t0, 5

slli t0, t0, 2
```

Execution:
```asm
5 * 4 = 20
```

Because:
```asm
left shift by 2 = multiply by 2²
```

So in general:
```asm
slli by n ≈ multiplication by 2ⁿ
```

---

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

### 6.2 The ABI (Application Binary Interface)

The ABI is a set of conventions that defines how routines interact with the execution environment: how parameters are passed, how values are returned, how registers are managed, and how the memory layout is organized.

---

### 6.3 Passing Parameters and Returning Values

In the RISC-V ILP32 ABI, parameters are passed through dedicated registers:
```asm
a0, a1, ..., a7
```

When a function is called, the first 8 arguments are already stored in these registers, in order.

**Return values** go in `a0`. If the returned value is 64 bits wide, the lower 32 bits go in `a0` and the upper 32 bits go in `a1`.

---

**More than 8 parameters**

If a function needs more than 8 arguments, the extra ones go on the stack. The caller must:
1. reserve stack space for the extra parameters (4 bytes each in RV32)
2. push them in reverse order (last parameter first)
3. only then jump to the routine

After the call, the caller is also responsible for freeing that stack space again.

Example: `sum10(a, b, c, d, e, f, g, h, i, j)`, returning the sum of all 10 arguments:

```asm
main:
li a0, 10       # 1st parameter
li a1, 20       # 2nd parameter
li a2, 30       # 3rd parameter
li a3, 40       # 4th parameter
li a4, 50       # 5th parameter
li a5, 60       # 6th parameter
li a6, 70       # 7th parameter
li a7, 80       # 8th parameter

addi sp, sp, -8 # reserve stack space for 2 extra parameters (4 bytes each)
li t1, 100
sw t1, 4(sp)    # push the 10th parameter
li t1, 90
sw t1, 0(sp)    # push the 9th parameter

jal sum10       # call sum10
addi sp, sp, 8  # deallocate the extra parameters
ret
```

Inside the routine, the first 8 arguments are already in `a0-a7`. The remaining ones are read from the stack, in the same order the caller pushed them:

```asm
sum10:
lw t1, 0(sp)    # 9th parameter
lw t2, 4(sp)    # 10th parameter

add a0, a0, a1
add a0, a0, a2
add a0, a0, a3
add a0, a0, a4
add a0, a0, a5
add a0, a0, a6
add a0, a0, a7
add a0, a0, t1
add a0, a0, t2   # a0 now holds the total
ret
```

The routine never touches `sp` to clean up the extra parameters — freeing that stack space is always the caller's job.

---

### 6.4 Passing by Value vs by Reference

- **By value** → the register/stack slot contains the actual value.
- **By reference** → the register/stack slot contains a memory address, and the routine reads/writes through that address.

Example in C, by value:
```c
int inc(int v) {
    return v + 1;
}
```
```asm
inc:
addi a0, a0, 1    # a0 = a0 + 1
ret
```

Same function, by reference:
```c
void inc(int* v) {
    *v = *v + 1;
}
```
```asm
inc:
lw a1, 0(a0)      # a1 = *v  (a0 holds an address, not a value)
addi a1, a1, 1    # a1 = a1 + 1
sw a1, 0(a0)      # *v = a1
ret
```

The difference: with pass-by-value, `a0` already contains the number. With pass-by-reference, `a0` contains the *address* of the number, so the routine must `lw` first to read it, and `sw` to write the result back. The caller doesn't need to read `a0` afterward, the value was already updated directly in memory.

---

### 6.5 Local and Global Variables

**Global variables** live in `.data` and are visible to every routine, exactly like a global variable in C:
```asm
.data
x: .word 0

.text
main:
lw a0, x           # read x
addi a0, a0, 1
ret
```

**Local variables** (declared inside a routine, like `tmp` below) should ideally live in registers - registers are much faster than memory.
```c
int foo() {
    int tmp = 5;
    return tmp + 1;
}
```
```asm
foo:
li a0, 5           # tmp = 5
addi a0, a0, 1     # tmp + 1
ret
```

Unlike C, RISC-V has no symbolic names for local variables, only labels and comments to keep track of what a register is being used for. A "local variable" in assembly is really just whatever register you decide to dedicate to it.

**When registers aren't enough**, local data must go on the stack instead:
- too many local variables to fit in the available registers
- the variable is an array or a struct (needs more than one word of space)
- the code needs the *address* of the variable (e.g. to pass it by reference)

The routine is responsible for allocating this space when it starts, and freeing it before it returns, by moving `sp`.

```asm
foo:
addi sp, sp, -4     # allocate 4 bytes for tmp
li t0, 5
sw t0, 0(sp)        # tmp = 5

lw t1, 0(sp)        # read tmp
addi t1, t1, 1      # tmp + 1

addi sp, sp, 4      # deallocate tmp
ret
```

**Arrays and structs on the stack** follow the same idea, just with more space reserved — one slot per element/field:

```c
int foo() {
    int arr[4] = {1, 2, 3, 4};
    return arr[2];
}
```
```asm
foo:
addi sp, sp, -16    # 4 elements * 4 bytes

li t0, 1
sw t0, 0(sp)        # arr[0] = 1
li t0, 2
sw t0, 4(sp)        # arr[1] = 2
li t0, 3
sw t0, 8(sp)        # arr[2] = 3
li t0, 4
sw t0, 12(sp)       # arr[3] = 4

lw a0, 8(sp)        # return arr[2]
addi sp, sp, 16     # deallocate
ret
```

```c
struct Point { int x; int y; };
int foo() {
    struct Point p = {3, 4};
    return p.x + p.y;
}
```
```asm
foo:
addi sp, sp, -8     # 2 fields * 4 bytes

li t0, 3
sw t0, 0(sp)        # p.x = 3
li t0, 4
sw t0, 4(sp)        # p.y = 4

lw t1, 0(sp)        # p.x
lw t2, 4(sp)        # p.y
add a0, t1, t2      # p.x + p.y

addi sp, sp, 8      # deallocate
ret
```

---

### 6.6 Caller-Saved vs Callee-Saved Registers

Registers get reused constantly for locals, temporaries, arguments, and return values, so before a routine overwrites one, its previous value might need to be saved somewhere safe first. The ABI decides *who* is responsible for that: the caller, or the callee.

| Register | Alias | Saved by |
|---|---|---|
| `x0` | `zero` | — |
| `x1` | `ra` | caller |
| `x2` | `sp` | callee |
| `x3` | `gp` | — |
| `x4` | `tp` | — |
| `x5–x7` | `t0–t2` | caller |
| `x8–x9` | `s0/fp, s1` | callee |
| `x10–x17` | `a0–a7` | caller |
| `x18–x27` | `s2–s11` | callee |
| `x28–x31` | `t3–t6` | caller |

**Caller-saved** (`a0–a7`, `t0–t6`, `ra`): the callee is free to overwrite these without asking permission. If the caller needs their value after the call, the caller must save them before `jal` and restore them after.

```asm
main:
addi sp, sp, -12
sw a0, 0(sp)        # save a0, a1, a2 before the call
sw a1, 4(sp)
sw a2, 8(sp)

jal sum              # sum is free to overwrite a0-a2 internally

lw a0, 0(sp)         # restore them afterward
lw a1, 4(sp)
lw a2, 8(sp)
addi sp, sp, 12
```

Stack access is relatively slow, so in practice only the registers actually needed afterward get saved — not all of them by default.

**Callee-saved** (`s0–s11`, `sp`): the opposite rule. If the callee wants to use these registers, it must save their original value first and restore it before returning, the caller can trust they'll come back unchanged.

```asm
add:
addi sp, sp, -16
sw s0, 0(sp)         # save s0-s3 before using them
sw s1, 4(sp)
sw s2, 8(sp)
sw s3, 12(sp)

add a0, a0, a1        # ... use s0-s3 for something here ...

lw s0, 0(sp)          # restore before returning
lw s1, 4(sp)
lw s2, 8(sp)
lw s3, 12(sp)
addi sp, sp, 16
ret
```

`gp` and `tp` are reserved for global-variable access and thread management — routines should never modify them at all.

---

### 6.7 Why `ra` Must Be Preserved on Nested Calls

`ra` is caller-saved, but there's a case where getting this wrong actually breaks the program: **nested calls**. Every `jal` overwrites `ra` with a new return address, so if a routine calls another routine while it still needs its own return address, it must save `ra` first.

Example: `main` calls `sum2`, and `sum2` calls `sum1` to do part of the work.

```c
int sum1(int x, int y) { return x + y; }
int sum2(int a, int b, int c) {
    int result = sum1(a, b);
    return result + c;
}
```
```asm
main:
li a0, 3
li a1, 4
li a2, 5
jal sum2
sum2:
addi sp, sp, -4
sw ra, 0(sp)        # save ra BEFORE calling sum1, or it gets overwritten

jal sum1            # this call overwrites ra

lw ra, 0(sp)        # restore ra so sum2 can still return to main
addi sp, sp, 4
add a0, a0, a2
ret
sum1:
add a0, a0, a1
ret
```

Without saving `ra` before `jal sum1`, `sum2` would lose its own return address and end up returning to the wrong place.

The same problem shows up in **recursion**: every recursive call overwrites `ra` again, so each call level must save its own `ra` on the stack before recursing, and restore it right before returning.

```c
int multiply(int x, int y) {
    if (y == 0) return 0;
    return x + multiply(x, y - 1);
}
```
```asm
multiply:
addi sp, sp, -4
sw ra, 0(sp)        # save THIS call's return address
mv s0, a0           # s0 = x (callee-saved, survives the recursive call)

beq a1, zero, base_case
addi a1, a1, -1
jal multiply        # a0 = multiply(x, y-1) -- overwrites ra again
add a0, a0, s0       # a0 = result + x
j end

base_case:
li a0, 0

end:
lw ra, 0(sp)        # restore THIS call's return address
addi sp, sp, 4
ret
```

Each recursive call has its own stack slot for `ra` (a new one is allocated on every call), so unwinding the recursion returns correctly through every level.

---

### 6.8 Worked Exercises

**Even or odd** return `0` if the input is even, `1` if odd:

```asm
check_parity:
andi a0, a0, 1     # keep only the lowest bit: 0 = even, 1 = odd
ret
```

The lowest bit of a binary number tells you its parity directly, no branching needed.

---

**Multiplication without `mul`** (RV32I has no multiply instruction), repeated addition:

```asm
# a0 = factor1, a1 = factor2 -> result in a0

moltiplica:
li t0, 0             # accumulator

loop_moltiplica:
beq a1, zero, exit_moltiplica
add t0, t0, a0        # accumulator += factor1
addi a1, a1, -1       # one multiplication "step" done
j loop_moltiplica

exit_moltiplica:
mv a0, t0
ret
```

Execution for `5 * 4`: the loop adds `5` to the accumulator `4` times (`0 → 5 → 10 → 15 → 20`), decrementing `a1` each time until it reaches `0`.

