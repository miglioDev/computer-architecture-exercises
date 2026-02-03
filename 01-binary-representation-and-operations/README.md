# Exercises – binary-representation-and-operations Basics (with Solutions)

## Exercise 1  
Determine the **maximum decimal value** that can be represented using:
- (A) 5 bits  
- (B) 10 bits  
- (C) 16 bits  

### Solution
For an **unsigned** representation with `n` bits, the maximum value is obtained when all bits are set to 1:
  max = 2^n - 1

- (A) 2^5 - 1 = 31
- (B) 2^10 - 1 = 1023
- (C) 2^16 - 1 = 65535

---

## Exercise 2  
Convert the following **binary numbers to decimal**:
- 1011010  
- 11100011  
- 11001101.101  

### Solution
## Binary to Decimal Conversion

When converting from binary to decimal, we start **from the rightmost bit**, which has weight `2^0`,
then move left with increasing powers of two (`2^1`, `2^2`, ...).

### Example 1

    1011010₂
    = 1·2^6 + 0·2^5 + 1·2^4 + 1·2^3 + 0·2^2 + 1·2^1 + 0·2^0
    = 64 + 16 + 8 + 2
    = 90

### Example 2

    11100011₂
    = 1·2^7 + 1·2^6 + 1·2^5 + 0·2^4 + 0·2^3 + 0·2^2 + 1·2^1 + 1·2^0
    = 128 + 64 + 32 + 2 + 1
    = 227

---

## Binary Fractions

For the **fractional part**, digits to the right of the binary point have **negative powers of two**:

    2^-1, 2^-2, 2^-3, ...

### Example

    11001101.101₂

**Integer part**

    11001101₂
    = 128 + 64 + 8 + 4 + 1
    = 205

**Fractional part**

    .101₂
    = 1·2^-1 + 0·2^-2 + 1·2^-3
    = 1/2 + 1/8

**Final result**

    11001101.101₂ = 205.625

---

## Exercise 3  
Calculate the **total number of bits** in a memory that contains:
- (A) 64 Kbit  
- (B) 128 Mbit  
- (C) 4 Gbit  

### Solution
In computer architecture:
    1 K = 2^10
    1 M = 2^20
    1 G = 2^30

- (A) 64 × 2^10 = 65,536 bits
- (B) 128 × 2^20 = 134,217,728 bits
- (C) 4 × 2^30 = 4,294,967,296 bits

---

## Exercise 4  
Convert the following **decimal numbers to binary**:
- 2024  
- 1492  
- 1984.625  

### Solution
To convert an **integer** from decimal to binary, repeatedly subtract the **largest power of 2** less than or equal to the number.

-    2024₁₀ = 11111101000₂

For 1492₁₀ :
1492 = 1024 + 256 + 128 + 64 + 16 + 4
    1492₁₀ = 10111010100₂

For the fractional number:

Integer part:
    1984₁₀ = 1024 + 512 + 256 + 128 + 64
    1984₁₀ = 11111000000₂

Fractional part:  
To convert fractions, repeatedly **multiply by the new base (2)**.  
Each time the result is ≥ 1, write `1` and subtract 1; otherwise write `0`.  
Stop when the fractional part becomes zero.

    0.625 × 2 = 1.25 → 1
    0.25  × 2 = 0.5  → 0
    0.5   × 2 = 1.0  → 1

    0.625₁₀ = 0.101₂

Final result:

    1984.625₁₀ = 11111000000.101₂

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

    (10221)₃
    = 1·3^4 + 0·3^3 + 2·3^2 + 2·3^1 + 1·3^0
    = 106

    (2313)₄
    = 2·4^3 + 3·4^2 + 1·4^1 + 3·4^0
    = 183

    (1441)₅
    = 1·5^3 + 4·5^2 + 4·5^1 + 1·5^0
    = 246


  (3B4)₁₂
Here `B` represents the digit **11** 0–9, A (=10), B (=11):

    = 3·12^2 + 11·12^1 + 4·12^0
    = 432 + 132 + 4
    = 568


    (2A9)₁₁
Here `A` represents the digit **10**:
    = 2·11^2 + 10·11^1 + 9·11^0
    = 242 + 110 + 9
    = 361

### Result
**None of the given numbers have the same decimal value.**

---

# Conversion - Exercise 6: Base 4 → Base 8 (using Base 2 as intermediate)

Perform the following conversion using base 2 as an intermediate base:  

    (310.2)₄ → ?₈

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

    (310.2)₄ = 110100.10₂

---

## Step 2: Convert Base 2 → Base 8

In base 8, each digit corresponds to **3 binary bits**, because:

    8 = 2^3

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
## Exercise 6: 6-bit Two's Complement Addition

### Problem Statement

Consider the following two numbers, represented in **6-bit two's complement**:

m1 = 100110
m2 = 001011

Tasks:

1. Determine whether the signs of the two numbers are **concordant** (same) or **discordant** (different).  
2. Calculate the sum of the two numbers (also represented in 6-bit two's complement) and discuss whether an **overflow** occurs.

---

### Solution

#### Step 1: Determine the sign of each number

- `m1 = 100110` → Most Significant Bit (MSB) = `1` → Negative  
- `m2 = 001011` → MSB = `0` → Positive  

**Conclusion:** The signs are **discordant**.

---

#### Step 2: Binary addition

Add the two numbers **bit by bit**, including carries:


100110  (m1)

* 001011  (m2)

---

110001  (6-bit sum)


Since the numbers have **discordant signs**, there is **no risk of overflow** in two's complement addition.

---

#### Step 3: Interpret the result

- The sum in 6-bit two's complement is `110001`.  
- MSB = `1` → Negative number.  

**Final Result:** `110001` (represents `-15` in decimal)  
**Overflow:** No  

---

**Key Notes:**

- In two's complement, **overflow occurs only when adding two numbers with the same sign** that produce a result with a different sign.  
- When adding numbers with **discordant signs**, overflow cannot occur.  
- The result is always represented in the same bit width, no extra manipulation is required.

## Exercise 7: Number Base Conversion and IEEE 754 Representation

### Problem Statement

Consider the following fractional number in base 10:

**4.3125**

1. Find the representation of this number in **base 2**.
2. Convert the obtained binary number into **base 16**.
3. Represent the following binary number in **IEEE 754 single-precision (32-bit) floating-point format**, explaining all steps:

**-110.11011101₂**

---

### Solution

#### 1. Conversion of 4.3125 from Base 10 to Base 2

We separate the number into its integer and fractional parts.

#### Integer Part

4 (base 10) = 100 (base 2)

#### Fractional Part

We repeatedly multiply the fractional part by 2:

| Step | Value × 2 | Integer Part |
|------|-----------|--------------|
| 0.3125 | 0.625 | 0 |
| 0.625 | 1.25 | 1 |
| 0.25 | 0.5 | 0 |
| 0.5 | 1.0 | 1 |

Reading the integer parts from top to bottom:

0.3125 (base 10) = 0.0101 (base 2)

#### Final Binary Representation

4.3125 (base 10) = 100.0101 (base 2)

---

### 2. Conversion from Base 2 to Base 16

We group the binary digits in groups of four, starting from the binary point:

100.0101 (base 2) = 0100.0101 (base 2)

| Binary | Hexadecimal |
|--------|-------------|
| 0100 | 4 |
| 0101 | 5 |

4.3125 (base 10) = 4.5 (base 16)

---

### 3. IEEE 754 Single-Precision Representation

Given binary number:

-110.11011101 (base 2)

#### 3.1 Sign Bit

The number is negative:

Sign bit S = 1

---

#### 3.2 Normalization

We normalize the number to the form:

1.mantissa × 2^e

-110.11011101 (base 2) = -1.1011011101 (base 2) × 2^2

So the real exponent is:

e = 2

---

#### 3.3 Exponent with Bias

For IEEE 754 single precision:

Bias = 127

Stored exponent:

E = e + bias = 2 + 127 = 129

129 (base 10) = 10000001 (base 2)

---

#### 3.4 Mantissa

The mantissa consists of the bits following the leading 1:

1011011101

We pad with zeros to reach 23 bits:

10110111010000000000000

---

### 4. Final IEEE 754 Representation (32 bits)

| Field | Bits |
|-------|------|
| Sign | 1 |
| Exponent | 10000001 |
| Mantissa | 10110111010000000000000 |

Final 32-bit binary representation:

11000000110110111010000000000000

---

## Final Results

- 4.3125 (base 10) = 100.0101 (base 2) = 4.5 (base 16)
- IEEE 754 single-precision representation:

11000000110110111010000000000000

---

## Question 8  
Given an 8-bit binary number, write the general formulas to compute the minimum and maximum representable values for:
- unsigned integers,
- sign-and-magnitude representation,
- two’s complement representation.

Briefly explain why the ranges are different.

## Explanation  

An 8-bit binary number provides 2^8 = 256 distinct bit patterns.  
The representable range depends on how these patterns are interpreted.

### Unsigned integers  
All 8 bits represent the magnitude.

- Minimum value:  
  min = 0
- Maximum value:  
  max = 2^8 - 1 = 255

Range: [0, 255]

### Sign and magnitude  
The most significant bit (MSB) represents the sign (0 = positive, 1 = negative).  
The remaining 7 bits represent the magnitude.

- Maximum value:  
  max = +(2^7 - 1) = +127
- Minimum value:  
  min = -(2^7 - 1) = -127

Range: [-127, +127]

This representation has two encodings for zero (+0 and -0), wasting one bit pattern.

### Two’s complement  
The MSB has negative weight (-2^7), while the remaining bits have positive weights.

- Maximum value:  
  max = 2^7 - 1 = +127
- Minimum value:  
  min = -2^7 = -128

Range: [-128, +127]

### Why the ranges differ  
The ranges differ because the most significant bit is interpreted differently:
- as a value bit in unsigned representation,
- as a sign indicator in sign-and-magnitude,
- as a negative-weight bit in two’s complement.

Two’s complement uses all bit patterns efficiently and allows arithmetic operations to be performed without special cases.

## Question 9:
Consider the following decimal numbers: -130 and -128.  
For each number, state whether it can be represented exactly using 8 bits.  
If it is not representable, write "overflow" and briefly justify why.  
If it is representable, provide its 8-bit binary representation.  

### Explanation  

The representable range for signed 8-bit integers in two’s complement is:
- minimum: -128
- maximum: +127

### Number: -130  
- Not representable on 8 bits.
- Reason: -130 is smaller than the minimum value (-128) allowed by 8-bit two’s complement.

Result: **overflow**

### Number: -128  
- Representable on 8 bits.
- It corresponds to the minimum value in 8-bit two’s complement.
- While with sign and magnitute it is not representable **overflow**

Binary representation (8 bits):
-128 → 10000000

