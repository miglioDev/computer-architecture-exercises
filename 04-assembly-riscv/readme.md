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

## 3. Loops

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
