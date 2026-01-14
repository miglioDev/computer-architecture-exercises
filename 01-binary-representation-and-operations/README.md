# Exercises – Number Systems and Memory Basics (with Solutions)

## Exercise 1  
Determine the **maximum decimal value** that can be represented using:
- (A) 5 bits  
- (B) 10 bits  
- (C) 16 bits  

### Solution
For an **unsigned** representation with `n` bits, the maximum value is obtained when all bits are set to 1:
\[
\text{max} = 2^n - 1
\]

- (A) \(2^5 - 1 = 31\)
- (B) \(2^{10} - 1 = 1023\)
- (C) \(2^{16} - 1 = 65535\)

---

## Exercise 2  
Convert the following **binary numbers to decimal**:
- 1011010  
- 11100011  
- 11001101.101  

### Solution
When converting from binary to decimal, we start **from the rightmost bit**, which has weight \(2^0\), then move left with increasing powers of two (\(2^1, 2^2, \dots\)).

- \(1011010_2 = 1·2^6 + 0·2^5 + 1·2^4 + 1·2^3 + 0·2^2 + 1·2^1 + 0·2^0\)  
  \[
  = 64 + 16 + 8 + 2 = 90
  \]

- \(11100011_2 = 1·2^7 + 1·2^6 + 1·2^5 + 0·2^4 + 0·2^3 + 0·2^2 + 1·2^1 + 1·2^0\)  
  \[
  = 128 + 64 + 32 + 2 + 1 = 227
  \]

For the **fractional part**, digits to the right of the binary point have **negative powers of two**:
\[
2^{-1}, 2^{-2}, 2^{-3}, \dots
\]

- \(11001101.101_2\)

Integer part:
\[
11001101_2 = 128 + 64 + 8 + 4 + 1 = 205
\]

Fractional part:
\[
.101_2 = 1·2^{-1} + 0·2^{-2} + 1·2^{-3} = \frac{1}{2} + \frac{1}{8}
\]

Final result:
\[
11001101.101_2 = 205.625
\]

---

## Exercise 3  
Calculate the **total number of bits** in a memory that contains:
- (A) 64 Kbit  
- (B) 128 Mbit  
- (C) 4 Gbit  

### Solution
In computer architecture:
- \(1K = 2^{10}\)
- \(1M = 2^{20}\)
- \(1G = 2^{30}\)

- (A) \(64 \times 2^{10} = 65\,536\) bits  
- (B) \(128 \times 2^{20} = 134\,217\,728\) bits  
- (C) \(4 \times 2^{30} = 4\,294\,967\,296\) bits  

---

## Exercise 4  
Convert the following **decimal numbers to binary**:
- 2024  
- 1492  
- 1984.625  

### Solution
To convert an **integer** from decimal to binary, repeatedly subtract the **largest power of 2** less than or equal to the number.

- \(2024_{10} = 11111101000_2\)

For \(1492_{10}\) :
\[
1492 = 1024 + 256 + 128 + 64 + 16 + 4
\]
\[
1492_{10} = 10111010100_2
\]

For the fractional number:

Integer part:
\[
1984_{10} = 1024 + 512 + 256 + 128 + 64
\]
\[
1984_{10} = 11111000000_2
\]

Fractional part:  
To convert fractions, repeatedly **multiply by the new base (2)**.  
Each time the result is ≥ 1, write `1` and subtract 1; otherwise write `0`.  
Stop when the fractional part becomes zero.

\[
0.625 \times 2 = 1.25 \rightarrow 1
\]
\[
0.25 \times 2 = 0.5 \rightarrow 0
\]
\[
0.5 \times 2 = 1.0 \rightarrow 1
\]

\[
0.625_{10} = 0.101_2
\]

Final result:
\[
1984.625_{10} = 11111000000.101_2
\]

---

## Exercise 5  
Each of the following numbers is expressed in a **different base**.  
Determine **which ones have the same decimal value**:
- (10221)₃  
- (2313)₄  
- (1441)₅  
- (3B4)₁₂  
- (2A9)₁₁  

### Solution
Each digit is multiplied by the corresponding **power of the base**, starting from the rightmost digit with power \(0\).

- \((10221)_3\)
\[
= 1·3^4 + 0·3^3 + 2·3^2 + 2·3^1 + 1·3^0 = 106
\]

- \((2313)_4\)
\[
= 2·4^3 + 3·4^2 + 1·4^1 + 3·4^0 = 183
\]

- \((1441)_5\)
\[
= 1·5^3 + 4·5^2 + 4·5^1 + 1·5^0 = 246
\]

- \((3B4)_{12}\)  
Here `B` represents the digit **11** 0–9, A (=10), B (=11):
\[
= 3·12^2 + 11·12^1 + 4·12^0
\]
\[
= 432 + 132 + 4 = 568
\]

- \((2A9)_{11}\)  
Here `A` represents the digit **10**:
\[
= 2·11^2 + 10·11^1 + 9·11^0
\]
\[
= 242 + 110 + 9 = 361
\]

### Result
**None of the given numbers have the same decimal value.**

---

# Conversion - Exercise 6: Base 4 → Base 8 (using Base 2 as intermediate)

Perform the following conversion using base 2 as an intermediate base:  

\[
(310.2)_4 \;\rightarrow\; ?_8
\]

---

## Step 1: Convert Base 4 → Base 2

Each digit in base 4 can be converted to **2 bits** in binary:

| Base 4 | Base 2 |
|--------|--------|
| 0      | 00     |
| 1      | 01     |
| 2      | 10     |
| 3      | 11     |

- Integer part: 310₄ → 3 1 0 → 11 01 00 → **110100₂**   
- Fractional part: .2₄ → 2 → 10 → **.10₂**   

**Result in binary:**  
\[
(310.2)_4 = 110100.10_2
\]

---

## Step 2: Convert Base 2 → Base 8

because:

\[
8 = 2^3
\]

So, to convert from binary to octal:

- **Integer part:** Group the binary digits into **groups of 3 bits**, starting from the decimal point and moving **to the left**.  

  Our integer part: `110100`  

  - Split into groups of 3: `110` `100`  
  - Convert each group to decimal (octal):  
    - `110₂ = 6₈`  
    - `100₂ = 4₈`  
  - Integer part in octal = **64₈**

- **Fractional part:** Again we have to group the binary digits into **3 bits**, starting **right after the decimal point** and moving **to the right**.  

  Our fractional part: `.10`  

  - Add trailing zeros (on the righ) if needed to complete a group of 3: `100`  
  - Convert each group to decimal (octal):  
    - `100₂ = 4₈`  
  - Fractional part in octal = **.4₈**

---

## Exercise 5  
Given a pair of binary integers, perform:
- Addition
- Subtraction
- Multiplication

---

## Numbers Used

- A = 101₂
- B = 10₂

(decimal check: 101₂ = 5, 10₂ = 2)

---

## A. Binary Addition

### Operation
101₂ + 10₂

### Step 1 – Align the bits

  101
+ 010
-----

### Step 2 – Add from right to left

- 1 + 0 = 1
- 0 + 1 = 1
- 1 + 0 = 1

No carry is generated.

### Result

  101
+ 010
-----
  111

### Final Result

101₂ + 10₂ = 111₂

(decimal check: 5 + 2 = 7 )

---

## B. Binary Subtraction

### Operation
101₂ − 10₂

### Step 1 – Align the bits

  101
- 010
-----

### Step 2 – Subtract from right to left

- 1 − 0 = 1
- 0 − 1 → not possible  
  → borrow 1 from the left bit  
  → in binary, a borrow always has value 1, following the same principles as decimal subtraction  
  → 10 − 1 = 1
- The leftmost bit becomes 0 after borrowing  
  → 0 − 0 = 0

### Result

  101
- 010
-----
  011

### Final Result

101₂ − 10₂ = 11₂

(decimal check: 5 − 2 = 3 )

---

## C. Binary Multiplication

### Operation
101₂ × 10₂

### Important Rules

- × 1 → copy the number
- × 0 → the result is 0
- each new row is shifted left by one position (equivalent to multiplying by 2)

### Step 1 – Write the operation

   101
×   10
------

### Step 2 – Multiply by the rightmost bit (0)

101 × 0 = 000

   000

### Step 3 – Multiply by the next bit (1)

101 × 1 = 101  
Shift left by one position:

  1010

### Step 4 – Add the partial results

   000
+ 1010
------
  1010

### Final Result

101₂ × 10₂ = 1010₂

(decimal check: 5 × 2 = 10 )

---




