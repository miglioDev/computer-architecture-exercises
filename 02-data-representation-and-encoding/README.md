# Exercises – binary-representation-and-operations Basics

---

## Exercise 1 — Unsigned Integer Representation

Consider the unsigned decimal number **28** (base 10).

1. Explain how it is possible to determine (even without explicitly converting from base 10 to base 2) how many bits are required to represent the number.
2. Convert the number **28** from base 10 to base 2, showing the full procedure.
3. Given a **16-bit register**, explain how the unsigned number 28 can be stored in it, filling all the bits of the register.

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

**Result:** `28₁₀ = 11100₂`

---

### 3. Storing the Number in a 16-bit Register

Since the number is **unsigned**, it must be extended using **zero extension**.

Zero extension consists of adding leading zeros to the left of the binary number until the desired register width is reached.

Original 5-bit representation: `11100`

Extended to 16 bits: `0000000000011100`

**Result:** `28₁₀ = 0000000000011100₂  (16-bit unsigned representation)`

---

## Exercise 2 — UCS-2 Character Encoding and Base Conversion

Consider the following **32-bit sequence**, expressed in compact hexadecimal notation:

```
004100E9
```

1. Assume that these 4 bytes represent a sequence of characters encoded using **UCS-2** (each character is encoded using its Unicode code point on 16 bits). Determine which characters are represented. (Use the Unicode table limited to the first 256 code points.)
2. Convert the hexadecimal sequence from **base 16 to base 2**, showing the reasoning.
3. Assume now that the same **32-bit binary sequence** obtained in the previous step represents an **IEEE 754 single-precision floating-point number**.
   - Partition the bits into **sign**, **exponent (with bias 127)**, and **mantissa**.
   - State whether the number is **normalized** or **denormalized**.
   - Decode the number and express it in binary form and in the form `± b.mmmmmmm · 2^e`

### 1. Interpretation as UCS-2 Characters

**UCS-2** is a character encoding in which each character is stored using exactly **16 bits (2 bytes)**, with the 16-bit value corresponding directly to the Unicode code point.

The given sequence is `004100E9` (32 bits). Since each character occupies 16 bits:
```
32 bits ÷ 16 bits per character = 2 characters
```

**Step 1: Split the hexadecimal sequence into 16-bit blocks**

Each hexadecimal digit represents 4 bits, so 4 hex digits = 16 bits = 1 UCS-2 character:

```
0041   00E9
```

**Step 2: Convert each hexadecimal value to decimal**

- `0x0041` = **65₁₀**
- `0x00E9` = **233₁₀**

**Step 3: Look up the corresponding Unicode characters**

Using the Unicode table for the first 256 code points:

- Code point **65** → `A` (Latin capital letter A)
- Code point **233** → `é` (Latin small letter e with acute)

**Result:** The UCS-2 encoded sequence represents: `Aé`

---

### 2. Conversion from Base 16 to Base 2

Each **hexadecimal digit** corresponds to **4 binary bits**. The conversion is done digit by digit:

Hexadecimal: `0  0  4  1   0  0  E  9`

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

**Final binary sequence (32 bits):** `00000000 01000001 00000000 11101001`

---

### 3. Interpretation as IEEE 754 Floating-Point Number

**IEEE 754 Single-Precision Format:** 1 bit (sign) + 8 bits (exponent) + 23 bits (mantissa)

**Binary sequence:** `00000000010000010000000011101001`

**Partition the bits:** `0 | 00000000 | 10000010000000011101001`

- **Sign bit**: `0` → positive number
- **Exponent field**: `00000000` (value = 0)
- **Mantissa field**: `10000010000000011101001`

**Normalization:** In IEEE 754, when the exponent field is all zeros and mantissa ≠ 0, the number is **denormalized**.

For denormalized numbers:
- Hidden bit: `b = 0`
- Exponent: `e = 1 − 127 = −126`
- Mantissa: `0.10000010000000011101001₂`

**Final representation:** `+ 0.10000010000000011101001 · 2^(-126)`

---

## Exercise 3 – Character Encoding UTF-8, UCS-2 and IEEE 754 Interpretation

Consider the two-letter word **"Gò"**, where the second character is the lowercase letter **"ò"** (Latin small letter o with grave accent).

### 1. UTF-8 Encoding

**Character `G`:**
- Unicode code point: U+0047
- Range: U+0000 – U+007F → **1 byte** in UTF-8
- Hexadecimal: `47`

**Character `ò`:**
- Unicode code point: U+00F2
- Range: U+0080 – U+07FF → **2 bytes** in UTF-8
- UTF-8 two-byte format: `110xxxxx 10xxxxxx`
- Filling in the bits: `11000011 10110010`
- Hexadecimal: `C3 B2`

**Final UTF-8 Encoding:** `47 C3 B2`

---

### 2. UCS-2 and IEEE 754 Analysis

**UCS-2 Encoding:** Each character uses exactly 16 bits (2 bytes).

- Character `G` (U+0047): `0047`
- Character `ò` (U+00F2): `00F2`

**Final UCS-2 Encoding:** `00 47 00 F2` (or without spaces: `004700F2`)

**IEEE 754 Single Precision Interpretation of `004700F2`:**

Binary sequence: `00000000010001110000000011110010`

| S | Exponent  | Mantissa                 |
|---|-----------|--------------------------|
| 0 | 00000000  | 1000111000000011110010   |

- **Sign:** `0` → positive
- **Exponent:** `00000000` (value = 0) with mantissa ≠ 0 → **denormalized**
- **Mantissa:** `0.1000111000000011110010₂`
- **Exponent used:** `1 − 127 = −126`

**Conclusion:** The number is **denormalized** because the exponent field is all zeros and the mantissa is non-zero. Denormalized numbers represent values very close to zero and allow for gradual underflow.

---

## Exercise 4: UCS-2 and Endianness

### Exercise 4a: U+03A9 (Ω – Greek Omega)

1. Hexadecimal (16 bits): `0x03A9`
2. Binary: `0000 0011 1010 1001`
3. Bytes:
   - MSB (High byte): `0x03`
   - LSB (Low byte): `0xA9`
4. Memory order:
   - Big Endian: `03 A9` (most significant byte first)
   - Little Endian: `A9 03` (least significant byte first)
5. **Unicode value remains the same** — Endianness only affects byte order in memory, not the character value

---

### Exercise 4b: U+20AC (€ – Euro Sign)

1. Hexadecimal (16 bits): `0x20AC`
2. Binary: `0010 0000 1010 1100`
3. Bytes:
   - MSB (High byte): `0x20`
   - LSB (Low byte): `0xAC`
4. Memory order:
   - Big Endian: `20 AC`
   - Little Endian: `AC 20`
5. **Unicode value remains the same** — Endianness changes only byte order in memory, not the code point value

---

## Exercise 5 – Character Encoding Identification

The Danish word **MØDE** (meaning *meeting*) has three different encodings:

| Encoding | Value |
|----------|-------|
| (a) | `4D C3 98 44 45` |
| (b) | `00 4D 00 D8 00 44 00 45` |
| (c) | `4D D8 44 45` |

**Unicode code points:** M = U+004D, Ø = U+00D8, D = U+0044, E = U+0045

### Identifying the Encodings

**Encoding (b) → UCS-2**

Pattern `00 XX 00 XX` repeated indicates UCS-2 (each character is exactly 2 bytes, 16-bit Unicode):
- `00 4D` → M
- `00 D8` → Ø
- `00 44` → D
- `00 45` → E

---

**Encoding (c) → Extended ASCII (Latin-1)**

Single-byte encoding: `4D D8 44 45`
- `4D` → M (standard ASCII)
- `D8` → Ø (value > 127, extended ASCII)
- `44` → D
- `45` → E

---

**Encoding (a) → UTF-8**

UTF-8 uses **byte prefixes**:
- `0xxxxxxx` = 1-byte ASCII
- `110xxxxx` = start of 2-byte sequence
- `10xxxxxx` = continuation byte

**Analysis:**
- `4D = 01001101` → starts with `0` → ASCII **M**
- `C3 = 11000011` → starts with `110` → first byte of 2-byte sequence
- `98 = 10011000` → starts with `10` → continuation byte
- `C3 98` together encode **Ø (U+00D8)**
- `44` → D
- `45` → E

**Mental Checklist:**
1. See `00 XX 00 XX` repeatedly? → UCS-2
2. See bytes starting with `10`? → UTF-8 is involved
3. UTF-8: check the **leading bits (prefix)**, remaining bits are payload
4. ASCII: values 0–127 only | Latin-1: 1 byte, includes characters beyond ASCII

---

## Exercise 6 – Conversion from ASCII to UTF-8 and UCS-2

**ASCII encoding (hexadecimal):** `4D 65 6E F9`

| Hex | Binary      | Character |
|-----|------------|-----------|
| 4D  | 0100 1101  | M |
| 65  | 0110 0101  | o |  
| 6E  | 0110 1110  | ù |
| F9  | 1111 1001  | q |

**String:** `"Moùq"`

### 1. UTF-8 Conversion

UTF-8 encoding depends on Unicode code point:
- U+0000 – U+007F → **1 byte**
- U+0080 – U+07FF → **2 bytes** (pattern: `110xxxxx 10xxxxxx`)

| Character | Unicode | UTF-8 (hex) |
|-----------|---------|-------------|
| M | U+004D | `4D` |
| o | U+006F | `6F` |
| ù | U+00F9 | `C3 B9` |
| q | U+0071 | `71` |

**UTF-8 result:** `4D 6F C3 B9 71`

---

### 2. UCS-2 Conversion

Each character is **16 bits fixed**, no prefixes.

**Big Endian (MSB first):**

| Character | Hex    |
|-----------|--------|
| M | 00 4D |
| o | 00 6F |
| ù | 00 F9 |
| q | 00 71 |

**UCS-2 Big Endian:** `00 4D 00 6F 00 F9 00 71`

**Little Endian (LSB first):**

| Character | Hex    |
|-----------|--------|
| M | 4D 00 |
| o | 6F 00 |
| ù | F9 00 |
| q | 71 00 |

**UCS-2 Little Endian:** `4D 00 6F 00 F9 00 71 00`
