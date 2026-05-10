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

### 1.1 : `li` Load Immediate

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