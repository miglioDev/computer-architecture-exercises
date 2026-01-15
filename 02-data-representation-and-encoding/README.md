# Exercise 1 — Unsigned Integer Representation

## Exercise Statement

Consider the unsigned decimal number **28** (base 10).

1. Explain how it is possible to determine (even without explicitly converting from base 10 to base 2) how many bits are required to represent the number.
2. Convert the number **28** from base 10 to base 2, showing the full procedure.
3. Given a **16-bit register**, explain how the unsigned number 28 can be stored in it, filling all the bits of the register.

---

## Solution

### 1. Number of Bits Required

For an **unsigned** integer represented with `n` bits, the range of representable values is:

```

0 to (2ⁿ − 1)

```

To determine how many bits are needed, we find the smallest value of `n` such that:

```

2ⁿ > 28

```

Evaluating powers of two:

- 2⁴ = 16  (not sufficient)
- 2⁵ = 32  (sufficient)

#### Result

The number **28** requires **5 bits** to be represented as an unsigned binary number.

---

### 2. Conversion from Base 10 to Base 2

The conversion is performed using **repeated division by 2**, keeping track of the remainders.

| Division | Quotient | Remainder |
|--------|----------|-----------|
| 28 ÷ 2 | 14 | 0 |
| 14 ÷ 2 | 7  | 0 |
| 7 ÷ 2  | 3  | 1 |
| 3 ÷ 2  | 1  | 1 |
| 1 ÷ 2  | 0  | 1 |

The binary representation is obtained by reading the remainders **from bottom to top**:

```

11100₂

```

#### Result

```

28₁₀ = 11100₂

```

---

### 3. Storing the Number in a 16-bit Register

Since the number is **unsigned**, it must be extended using **zero extension**.

Zero extension consists of adding leading zeros to the left of the binary number until the desired register width is reached.

Original 5-bit representation:

```

11100

```

Extended to 16 bits:

```

0000000000011100

```

#### Result

```

28₁₀ = 0000000000011100₂  (16-bit unsigned representation)

```

---

# Exercise 2 — UCS-2 Character Encoding and Base Conversion

## Exercise Statement

Consider the following **32-bit sequence**, expressed in compact hexadecimal notation:

```

004100E9

```

1. Assume that these 4 bytes represent a sequence of characters encoded using **UCS-2**
   (each character is encoded using its Unicode code point on 16 bits).
   Determine which characters are represented.
   (Use the Unicode table limited to the first 256 code points.)
2. Convert the hexadecimal sequence from **base 16 to base 2**, showing the reasoning.
3. Assume now that the same **32-bit binary sequence** obtained in the previous step
   represents an **IEEE 754 single-precision floating-point number**.
   - Partition the bits into **sign**, **exponent (with bias 127)**, and **mantissa**.
   - State whether the number is **normalized** or **denormalized**.
   - Decode the number and express it:
     - in binary form
     - in the form  
       `± b.mmmmmmm · 2^e`  
       where:
       - the sign is written using `+` or `−`
       - `b` is the hidden bit
       - the fractional part is the mantissa
       - `e` is the exponent derived from the exponent field

---

## Solution

### 1. Interpretation as UCS-2 Characters

#### What UCS-2 Encoding Is

- **UCS-2** is a character encoding in which:
  - **each character is stored using exactly 16 bits**, i.e. **2 bytes**
  - the 16-bit value corresponds **directly** to the Unicode code point of the character
- In this exercise:
  - the sequence is **32 bits long**
  - since each character occupies **16 bits**, the sequence represents:

```

32 bits ÷ 16 bits per character = 2 characters

```

This explains why a 32-bit (4-byte) hexadecimal sequence encodes **two characters** in UCS-2.

---

#### Step 1: Split the hexadecimal sequence into 16-bit blocks

The given sequence is:

```

004100E9

```

Each **hexadecimal digit represents 4 bits**, so:
- 4 hex digits = 16 bits = 1 UCS-2 character

We therefore split the sequence into two groups of 4 hex digits:

```

0041   00E9

```

Each group corresponds to **one Unicode code point**.

---

#### Step 2: Convert each hexadecimal value to decimal

- `0x0041` = **65₁₀**
- `0x00E9` = **233₁₀**

These decimal values are Unicode code points.

---

#### Step 3: Look up the corresponding Unicode characters

Using the Unicode table for the first 256 code points:

- Code point **65** → `A` (Latin capital letter A)
- Code point **233** → `é` (Latin small letter e with acute)

---

#### Result

The UCS-2 encoded sequence represents the two-character string:

```

Aé

```

---

### 2. Conversion from Base 16 to Base 2

#### Conversion Rule

- Each **hexadecimal digit** corresponds to **4 binary bits**
- The conversion is done digit by digit, without changing the order

---

#### Step-by-step conversion

Hexadecimal sequence:

```

0  0  4  1   0  0  E  9

```

Binary conversion:

```

0 → 0000
0 → 0000
4 → 0100
1 → 0001

0 → 0000
0 → 0000
E → 1110
9 → 1001

```

---

#### Final binary sequence (32 bits)

```

00000000 01000001 00000000 11101001

```

---

### 3. Interpretation as IEEE 754 Floating-Point Number

We now interpret the same **32-bit binary sequence** as a **single-precision IEEE 754 floating-point number**.

```

00000000010000010000000011101001

```

---

#### IEEE 754 Single-Precision Format

A 32-bit IEEE 754 floating-point number is divided as follows:

| Field | Number of bits |
|----|----|
| Sign | 1 |
| Exponent | 8 |
| Mantissa (fraction) | 23 |

---

#### Step 1: Partition the bits

```

0 | 00000000 | 10000010000000011101001

```

- **Sign bit**: `0`
- **Exponent field**: `00000000`
- **Mantissa field**: `10000010000000011101001`

---

#### Step 2: Determine Normalization

In the IEEE 754 single-precision standard, the interpretation of the exponent field is as follows:

- If the exponent field is **all zeros** → the number is **denormalized**
- If the exponent field is **neither all zeros nor all ones** → the number is **normalized**
- (If the exponent field is all ones, the value represents special cases such as ±∞ or NaN)

In this case, the exponent field is:

```

00000000

```

Therefore, the number is **denormalized**.

For **denormalized numbers**:
- the hidden bit is `b = 0`
- the exponent is fixed and computed as:

```

e = 1 − bias = 1 − 127 = −126

```

#### Step 3: Write the mantissa

Mantissa bits:

```

10000010000000011101001

```

As a binary fraction:

```

0.10000010000000011101001₂

```

---

#### Step 4: Determine the sign

- Sign bit `0` → **positive number**

---

#### Step 5: Final decoded representation

Binary scientific notation:

```

* 0.10000010000000011101001 · 2^(-126)

```

Where:
- `+` is the sign
- `b = 0` (denormalized number)
- the fractional part is the mantissa
- `e = −126`

---





