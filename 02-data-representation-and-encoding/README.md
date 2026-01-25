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
# Exercise 3 – Character Encoding UTF-8, UCS-2 and IEEE 754 Interpretation

## Problem Statement

Consider the two-letter word **“Gò”**, where the second character is the lowercase letter **“ò”** (Latin small letter o with grave accent).

Answer the following questions:

1. What is the UTF-8 encoding of this word?
2. What is the UCS-2 encoding of the same word (using 16-bit Unicode codes)?  
   Express the result as a sequence of hexadecimal digits.
3. Take the sequence of **8 hexadecimal digits** obtained in the previous point and interpret it as a **single-precision IEEE 754 floating-point number (32 bits)**.  
   State whether the number is **normalized** or **denormalized**, and justify your answer.

---

## Solution

The word is composed of two characters:

1. `G`
2. `ò`

---

## 1. UTF-8 Encoding

### Character `G`

- Unicode code point: U+0047
- Range: U+0000 – U+007F  
  → encoded using **1 byte** in UTF-8

Binary representation:

01000111


Hexadecimal representation:

47

---

### Character `ò`

- Unicode code point: U+00F2
- Range: U+0080 – U+07FF  
  → encoded using **2 bytes** in UTF-8

Binary representation of the Unicode code point:

11110010

UTF-8 two-byte format:

110xxxxx 10xxxxxx

Filling in the bits:

11000011 10110010

Hexadecimal representation:

C3 B2

---

### Final UTF-8 Encoding of the Word

In hexadecimal:

47 C3 B2

---

## 2. UCS-2 Encoding (16-bit Unicode)

In UCS-2, each character is encoded using exactly **16 bits (2 bytes)**.

### Character `G`

- Unicode: U+0047
- 16-bit representation:

0047

---

### Character `ò`

- Unicode: U+00F2
- 16-bit representation:

00F2

---

### Final UCS-2 Encoding of the Word

As a sequence of hexadecimal digits:


00 47 00 F2

or, without spaces:


004700F2


---

## 3. Interpretation as IEEE 754 Single Precision Floating-Point

We now interpret the 8 hexadecimal digits:


004700F2

as a **32-bit IEEE 754 single-precision floating-point number**.

---

### Conversion to Binary

Each hexadecimal digit corresponds to 4 bits:


0    0    4    7    0    0    F    2
0000 0000 0100 0111 0000 0000 1111 0010


Complete 32-bit binary sequence:

00000000010001110000000011110010

---

### IEEE 754 Field Separation

Single-precision format:
- 1 bit: sign (S)
- 8 bits: exponent (E)
- 23 bits: mantissa (M)

Splitting the bits:

| S | Exponent  | Mantissa                 |
|---|-----------|--------------------------|
| 0 | 00000000  | 1000111000000011110010   |

---

### Sign Bit

- S = 0  
  → the number is **positive**

---

### Exponent Analysis

Exponent bits:

00000000

- Exponent value = 0

According to IEEE 754:
- Exponent = 0 and mantissa ≠ 0  
  → the number is **denormalized (subnormal)**

---

### Mantissa Interpretation

For denormalized numbers:
- There is **no implicit leading 1**
- The significand has the form:

0.mantissa


Thus, the mantissa represents:

0.1000111000000011110010₂

The exponent used is:
```

1 − bias = 1 − 127 = −126

```

---

## Final Conclusion

- The UTF-8 encoding of the word **“Go”** (with `ò`) is:

47 C3 B2


- The UCS-2 encoding (16-bit Unicode, hexadecimal) is:

00 47 00 F2


- Interpreting `004700F2` as a 32-bit IEEE 754 floating-point number:
  - the sign is positive
  - the exponent is zero
  - the mantissa is non-zero

Therefore, the number is **denormalized**.

---

## Key Observation

A floating-point number in IEEE 754 single precision is **denormalized** when the exponent field is all zeros and the mantissa is not zero.  
Denormalized numbers are used to represent values very close to zero and allow for gradual underflow.

# Exercise 4: UCS-2 and Endianness

## Exercise 4a: U+03A9 (Ω – Greek Omega)

**Request:**
1. Write the Unicode code point in hexadecimal (16 bits)
2. Write the binary representation (16 bits)
3. Identify:
   - Most Significant Byte (MSB)
   - Least Significant Byte (LSB)
4. Show byte order in memory:
   - a) Big Endian
   - b) Little Endian
5. Does the Unicode value change?

**Solution:**
1. Hexadecimal: 0x03A9 the prefix 0x indicates that what follows is a hexadecimal code.
2. Binary: 0000 0011 1010 1001
3. Bytes: 
   - MSB (High byte = left most byte): 0x03
   - LSB (Low byte): 0xA9
4. Memory order:
   - Big Endian: 03 A9 (most significant byte first)
   - Little Endian: A9 03 (least significant byte first)
5. Unicode value remains the same 
   - Endianness only affects byte order in memory, not the character value

---

## Exercise 4b: U+20AC (€ – Euro Sign)

**Request:**
 the same as before (1,2,3,4,5)

**Solution:**
1. Hexadecimal: 0x20AC
2. Binary: 0010 0000 1010 1100
3. Bytes:
   - MSB (High byte = left most ): 0x20
   - LSB (Low byte): 0xAC
4. Memory order:
   - Big Endian: 20 AC
   - Little Endian: AC 20
5. Unicode value remains the same  
   - Endianness changes only byte order in memory, not the code point value


# Exercise 5 – Character Encoding Identification

## Exercise description

The Danish word **MØDE** (meaning *meeting*) is written using **uppercase letters only**.

Three different encodings of the word are given below.  
Each encoding is expressed in **hexadecimal notation**, where **each pair of hex digits corresponds to one byte**.

One encoding is:
- **UTF-8**
- one is **Extended ASCII (Latin-1 / ISO-8859-1)**
- one is **UCS-2** (2-byte Unicode encoding for the Basic Multilingual Plane)

### Task

Identify which encoding is which and explain **how they can be recognized**, paying special attention to **UTF-8 byte prefixes**.

---

## Given encodings

### Encoding (a)
```

4D C3 98 44 45

```

### Encoding (b)
```

00 4D 00 D8 00 44 00 45

```

### Encoding (c)
```

4D D8 44 45

```

---

## Unicode code points of the characters

| Character | Unicode |
|---------|---------|
| M | U+004D |
| Ø | U+00D8 |
| D | U+0044 |
| E | U+0045 |

---

## Solution and explanation

---

## Encoding (b) → UCS-2

```

00 4D 00 D8 00 44 00 45

```

### Why this is UCS-2

- Each character is encoded using **exactly 2 bytes**
- The format is `00 XX` for characters in the ASCII / Latin-1 range
- This is a direct 16-bit representation of Unicode code points (BMP)

### Decoding

| Bytes | Unicode | Character |
|------|---------|-----------|
| 00 4D | U+004D | M |
| 00 D8 | U+00D8 | Ø |
| 00 44 | U+0044 | D |
| 00 45 | U+0045 | E |

**Conclusion:**  
Encoding (b) is **UCS-2**

---

## Encoding (c) → Extended ASCII (Latin-1)

```

4D D8 44 45

```

### Important clarification

All encodings (ASCII, Latin-1, UTF-8) are made of **8-bit bytes**.  
What matters is **the value of the byte**, not the fact that it has 8 bits.

### Analysis

- `4D` → M (standard ASCII)
- `D8` → value > `7F` (127)

Pure ASCII only supports values from `00` to `7F`.  
In **Latin-1**, the byte `D8` represents the character **Ø**.

### Decoding

| Byte | Character |
|----|-----------|
| 4D | M |
| D8 | Ø |
| 44 | D |
| 45 | E |

**Conclusion:**  
Encoding (c) is **Extended ASCII (Latin-1)**

---

## Encoding (a) → UTF-8

```

4D C3 98 44 45

```

### UTF-8 prefix rules (key concept)

UTF-8 uses **byte prefixes**:

| Prefix | Meaning |
|------|---------|
| `0xxxxxxx` | 1-byte ASCII |
| `110xxxxx` | start of 2-byte sequence |
| `10xxxxxx` | continuation byte |
| `1110xxxx` | start of 3-byte sequence |

---

### Byte-by-byte analysis

#### Byte 1: `4D`
```

4D = 01001101

```
- Starts with `0`
- ASCII character → **M**

#### Bytes 2–3: `C3 98`

```

C3 = 11000011  → starts with 110 → first byte of 2-byte UTF-8 sequence
98 = 10011000  → starts with 10  → continuation byte

```

This matches the UTF-8 pattern:

```

110xxxxx 10xxxxxx

```

These two bytes together encode **Ø (U+00D8)** in UTF-8.

#### Remaining bytes

- `44` → D
- `45` → E

---

## Final identification

| Encoding | Type |
|-------|------|
| (a) `4D C3 98 44 45` | **UTF-8** |
| (b) `00 4D 00 D8 00 44 00 45` | **UCS-2** |
| (c) `4D D8 44 45` | **Extended ASCII (Latin-1)** |

---

## Exam-oriented mental checklist

1. **Do you see `00 XX 00 XX` repeatedly?** → UCS-2  
2. **Do you see bytes starting with `10`?** → UTF-8 is involved  
3. **UTF-8** → only check **the leading bits (prefix), the remaining bits are just payload
4. **ASCII** → values **0–127 only** Latin-1 → **1 byte**, includes characters beyond ASCII

---


