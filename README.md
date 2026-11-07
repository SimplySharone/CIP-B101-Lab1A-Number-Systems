# CIP-B101 – Lab 1A: Number Systems, ASCII, Timestamps and Data Representation

**International Cybersecurity and Digital Forensics Academy (ICDFA)**

---

## Student Information

| Field | Details |
|---|---|
| **Student Name** | Sharon Omowumi |
| **Course Code** | CIP-B101 |
| **Course Name** | Basic Computer Skills for Digital Forensics |
| **Laboratory** | Lab 1A |
| **Lab Title** | Number Systems, ASCII, Timestamps and Data Representation |
| **Platform** | Kali Linux – Oracle VirtualBox |
| **Programming Language** | Python 3 |
| **Submission Format** | PDF Laboratory Report |
| **Date** | November 2026 |

---

## 1. Introduction

### 1.1 Laboratory Overview

This laboratory introduces the foundational technical skills required for interpreting digital forensic evidence.

As a digital forensics trainee, it is important to understand how numerical values, encoded text, timestamps, cryptographic hashes and raw memory bytes are represented and interpreted within digital systems.

Digital evidence may be recovered from disk sectors, system logs, command-line outputs, file metadata, memory dumps and other digital sources.

This practical laboratory explores six areas of digital evidence interpretation:

1. Number systems and manual conversion.
2. ASCII, UTF-8 and hexadecimal evidence.
3. Unix epoch time and forensic timestamps.
4. Cryptographic hashing and the newline problem.
5. Endianness and memory representation.
6. Mini forensic interpretation case.

The exercises are performed using a Linux-based environment and appropriate command-line tools, including Python 3, `bc`, `xxd`, `date`, `md5sum` and `sha256sum`.

### 1.2 Learning Objectives

Upon completion of this laboratory, I should be able to:

- Differentiate decimal, binary and hexadecimal number systems.
- Perform manual conversions between different number systems.
- Validate numerical conversions using Linux shell arithmetic, `bc` and Python 3.
- Interpret ASCII and hexadecimal representations of digital evidence.
- Convert hexadecimal values into readable text.
- Convert Unix epoch timestamps and Mac Absolute Time values into UTC timestamps.
- Distinguish between local time and UTC.
- Calculate and compare MD5 and SHA-256 hashes.
- Explain how newline characters affect cryptographic hash values.
- Interpret raw hexadecimal bytes using little-endian and big-endian byte ordering.
- Analyse recovered forensic evidence and document findings professionally.

---

## 2. Laboratory Environment and Tools

### 2.1 Operating Environment

| Component | Configuration |
|---|---|
| Operating System | Kali Linux |
| Virtualisation Platform | Oracle VirtualBox |
| Terminal | Linux Terminal |
| Programming Language | Python 3 |
| Shell | Bash |
| Working Directory | `~/cip-b101-lab1a` |

### 2.2 Tools Used

| Tool | Purpose |
|---|---|
| `bash` | Execute Linux commands and arithmetic operations |
| `python3` | Validate conversions and perform timestamp calculations |
| `bc` | Perform numerical base conversions |
| `xxd` | Display hexadecimal representations of files |
| `date` | Convert Unix timestamps into readable dates |
| `md5sum` | Calculate MD5 hashes |
| `sha256sum` | Calculate SHA-256 hashes |

### 2.3 Environment Verification

The following commands are used to verify the laboratory environment:

```bash
whoami
hostname
cat /etc/os-release
python3 --version
```

Check the required utilities:

```bash
command -v bc
command -v xxd
command -v md5sum
command -v sha256sum
```

### 2.4 Create the Laboratory Working Directory

```bash
mkdir -p ~/cip-b101-lab1a
cd ~/cip-b101-lab1a
pwd
```

All practical exercises and generated evidence files are stored in this directory.

<img width="1317" height="954" alt="image" src="https://github.com/user-attachments/assets/058cd65b-a9b4-43f2-aa3f-d0b66999a555" />


- [ ] Screenshot 01 – Linux environment and working directory.

---

# 3. Task A – Number Systems and Manual Conversion

## 3.1 Introduction

Digital systems represent information using different number systems.

The three number systems examined in this exercise are:

- **Decimal (Base 10):** Uses digits 0–9.
- **Binary (Base 2):** Uses digits 0 and 1.
- **Hexadecimal (Base 16):** Uses digits 0–9 and letters A–F.

Hexadecimal is particularly useful in digital forensics because it provides a compact representation of binary data.

## 3.2 Manual Conversion Exercises

### Exercise 1: Binary to Decimal

Convert:

`101010₂`

Manual calculation:

| Bit | Position | Calculation | Result |
|---|---:|---|---:|
| 1 | 5 | 1 × 2⁵ | 32 |
| 0 | 4 | 0 × 2⁴ | 0 |
| 1 | 3 | 1 × 2³ | 8 |
| 0 | 2 | 0 × 2² | 0 |
| 1 | 1 | 1 × 2¹ | 2 |
| 0 | 0 | 0 × 2⁰ | 0 |

Total:

32 + 8 + 2 = **42**

### Exercise 2: Hexadecimal to Decimal

Convert:

`AB2₁₆`

Hexadecimal values:

- A = 10
- B = 11
- 2 = 2

Manual calculation:

(10 × 16²) + (11 × 16¹) + (2 × 16⁰)

= 2560 + 176 + 2

= **2738**

### Exercise 3: Decimal to Binary

Convert:

`126₁₀`

Manual division by 2:

| Division | Quotient | Remainder |
|---|---:|---:|
| 126 ÷ 2 | 63 | 0 |
| 63 ÷ 2 | 31 | 1 |
| 31 ÷ 2 | 15 | 1 |
| 15 ÷ 2 | 7 | 1 |
| 7 ÷ 2 | 3 | 1 |
| 3 ÷ 2 | 1 | 1 |
| 1 ÷ 2 | 0 | 1 |

Reading the remainders from bottom to top:

**1111110₂**

### Exercise 4: Decimal to Hexadecimal

Convert:

`2738₁₀`

Manual division by 16:

| Division | Quotient | Remainder |
|---|---:|---:|
| 2738 ÷ 16 | 171 | 2 |
| 171 ÷ 16 | 10 | 11 (B) |
| 10 ÷ 16 | 0 | 10 (A) |

Reading the remainders from bottom to top:

**AB2₁₆**

### Exercise 5: Binary to Hexadecimal

Convert:

`101010110010₂`

Group the binary digits into sets of four from the right:

`1010 1011 0010`

| Binary | Hexadecimal |
|---|---|
| 1010 | A |
| 1011 | B |
| 0010 | 2 |

Final answer:

**AB2₁₆**

## 3.3 Validate Conversions Using Linux

### Binary to Decimal

```bash
echo $((2#101010))
```

### Hexadecimal to Decimal

```bash
echo $((16#AB2))
```

### Decimal to Binary Using BC

```bash
echo "obase=2;126" | bc
```

### Decimal to Hexadecimal Using printf

```bash
printf "%X\n" 2738
```

### Python Validation

```bash
python3 - <<'PY'
print(int('101010', 2))
print(int('AB2', 16))
print(bin(126))
print(hex(2738))
PY
```

## 3.4 Conversion Evidence Worksheet

| Evidence Value | Given Base | Manual Answer | Verification |
|---|---|---|---|
| 101010 | Binary | 42 | Pending |
| AB2 | Hexadecimal | 2738 | Pending |
| 126 | Decimal | 1111110₂ | Pending |
| 2738 | Decimal | AB2₁₆ | Pending |
| 101010110010 | Binary | AB2₁₆ | Pending |

<img width="1098" height="955" alt="image" src="https://github.com/user-attachments/assets/158d20f6-e284-4f25-a1f8-3ecc3983f9e5" />



- [ ] Screenshot 02 – Binary, hexadecimal and decimal conversion commands.


<img width="1048" height="962" alt="image" src="https://github.com/user-attachments/assets/da8f19d8-c09d-45dd-b7e8-8ad2783f033e" />

- [ ] Screenshot 03 – Python validation results.

## 3.5 Forensic Relevance

Understanding number systems is essential in digital forensics because investigators frequently encounter hexadecimal and binary values when examining file headers, disk sectors, memory dumps and other digital evidence.

Accurate conversion helps investigators interpret numerical values without confusing their underlying representation.

---

# 4. Task B – ASCII, UTF-8 and Hexadecimal Evidence

## 4.1 Introduction

Digital systems store information as bytes.

A byte consists of eight bits and may represent a character, part of a Unicode character, a numerical value or other binary information.

ASCII provides a standard mapping between numerical values and characters.

UTF-8 is a variable-length character encoding that can represent a much wider range of characters.

## 4.2 Create Text Evidence

Create a file containing the word `Hello` without adding a newline:

```bash
echo -n "Hello" > evidence_text.txt
```

Display the file:

```bash
cat evidence_text.txt
```

Display its hexadecimal representation:

```bash
xxd evidence_text.txt
```

Display only the hexadecimal values:

```bash
xxd -p evidence_text.txt
```

Expected hexadecimal representation:

```text
48656c6c6f
```

## 4.3 Convert Hexadecimal to ASCII

Use `xxd` to convert hexadecimal values back into readable text:

```bash
printf "48656c6c6f" | xxd -r -p
```

Add a display newline:

```bash
printf "48656c6c6f" | xxd -r -p
printf "\n"
```

## 4.4 Python Conversion

```bash
python3 - <<'PY'
import binascii

text = b'Hello'

print('ASCII bytes:', text)
print('ASCII to hex:', binascii.hexlify(text).decode())
print('Hex to ASCII:', binascii.unhexlify('48656c6c6f').decode())
print('ord(H):', ord('H'))
print('chr(72):', chr(72))
PY
```

## 4.5 Student Exercise

Create a file containing the following text:

`Digital Forensics Begins With Bytes.`

Use your first name in the filename.

Example:

```bash
echo -n "Digital Forensics Begins With Bytes." > sharon_lab1a.txt
```

Display its contents:

```bash
cat sharon_lab1a.txt
```

Display the hexadecimal representation:

```bash
xxd sharon_lab1a.txt
```

Extract the plain hexadecimal values:

```bash
xxd -p sharon_lab1a.txt
```

Convert the hexadecimal values back into readable text:

```bash
xxd -p sharon_lab1a.txt | xxd -r -p
```

## 4.6 Understanding the Newline Character

When using `echo` without the `-n` option, Linux automatically adds a newline character.

The hexadecimal representation of a newline character is:

`0A`

Therefore:

- `echo` adds a newline character.
- `echo -n` does not add a newline character.
- The additional byte changes the underlying file contents.

## 4.7 Forensic Explanation

An examiner should not assume that every hexadecimal value represents readable text.

Hexadecimal values may represent:

- ASCII or UTF-8 text.
- Binary file structures.
- Image data.
- Compressed information.
- Encrypted content.
- Memory addresses or numerical values.

The interpretation depends on the context and structure of the evidence.

<img width="1054" height="967" alt="image" src="https://github.com/user-attachments/assets/841fd3d4-3ed7-4a09-b7cb-c0669e391e00" />

- [ ] Screenshot 04 – Text evidence creation and xxd output.

<img width="1057" height="955" alt="image" src="https://github.com/user-attachments/assets/7ed3c7ea-63f2-4acf-a848-a01de92ed8ad" />


- [ ] Screenshot 05 – Hexadecimal-to-ASCII conversion.

<img width="1022" height="955" alt="Screenshot 2026-11-07 evidence 6 lab 1A" src="https://github.com/user-attachments/assets/b4f4ed56-c734-4e6a-8a6f-afa7039232aa" />

- [ ] Screenshot 06 – Python conversion results.

---

# 5. Task C – Epoch Time and Forensic Timestamps

## 5.1 Introduction

Timestamps are important in digital investigations because they help establish when events occurred.

The Unix epoch begins on:

**1 January 1970 at 00:00:00 UTC.**

Unix timestamps represent the number of seconds elapsed since that starting point.

Forensic investigators must understand the difference between UTC and local time to avoid constructing inaccurate timelines.

## 5.2 Unix Epoch Conversion

Display the current Unix timestamp:

```bash
date +%s
```

Convert the supplied timestamp into UTC:

```bash
date -u -d @1643643491
```

Validate using Python:

```bash
python3 - <<'PY'
from datetime import datetime, timezone

print(datetime.fromtimestamp(1643643491, tz=timezone.utc))
PY
```

## 5.3 Compare Local Time and UTC

Display local time:

```bash
date
```

Display UTC:

```bash
date -u
```

Compare using Python:

```bash
python3 - <<'PY'
from datetime import datetime, timezone

print('Local:', datetime.now())
print('UTC  :', datetime.now(timezone.utc))
PY
```

## 5.4 Mac Absolute Time

Mac Absolute Time, also known as Cocoa Core Data time, counts seconds from:

**1 January 2001 at 00:00:00 UTC.**

The difference between the Unix epoch and Mac Absolute Time epoch is:

`978307200 seconds`

To convert Mac Absolute Time into Unix time:

Unix timestamp = Mac Absolute Time + 978307200

### Python Conversion

```bash
python3 - <<'PY'
from datetime import datetime, timezone

unix_epoch = datetime(1970, 1, 1, tzinfo=timezone.utc)
mac_epoch = datetime(2001, 1, 1, tzinfo=timezone.utc)

print(int((mac_epoch - unix_epoch).total_seconds()))

mac_value = 725846400
unix_value = mac_value + 978307200

print(datetime.fromtimestamp(unix_value, tz=timezone.utc))
PY
```

## 5.5 Timestamp Worksheet

| Timestamp Value | Format | Readable UTC Result |
|---|---|---|
| 1643643491 | Unix seconds | Pending practical verification |
| 1675008832 | Unix seconds | Pending practical verification |
| 725846400 | Mac Absolute Time | Pending practical verification |
| Current time | Unix seconds | Pending practical verification |

### Additional Verification Commands

```bash
date -u -d @1675008832
```

```bash
date -u -d @1643643491
```

## 5.6 Forensic Explanation

UTC provides a consistent reference point for forensic timelines.

Local time depends on the geographical location and timezone configuration of the system.

Forensic investigators should record whether timestamps are presented in UTC or local time.

Failure to account for timezone differences can cause events to appear to have occurred earlier or later than they actually did.

<img width="1046" height="960" alt="image" src="https://github.com/user-attachments/assets/3ea3cef2-9a68-4de3-b266-e3743364b20d" />

- [ ] Screenshot 07 – Unix timestamp conversion.

<img width="1053" height="959" alt="image" src="https://github.com/user-attachments/assets/c0207b4c-3c3e-451f-a096-325fb82272a4" />

- [ ] Screenshot 08 – UTC and local-time comparison.

<img width="1053" height="961" alt="image" src="https://github.com/user-attachments/assets/79704e8b-3d19-43ce-82c5-f1cac34f71e7" />
<img width="1090" height="956" alt="image" src="https://github.com/user-attachments/assets/07e1bfd3-01bd-41ee-b49f-caf2fa475054" />


- [ ] Screenshot 09 – Mac Absolute Time conversion.

---

# 6. Task D – Hashing and the Newline Problem

## 6.1 Introduction

Cryptographic hashing is used to verify the integrity of digital evidence.

A hash function produces a fixed-length value from input data.

Even a small modification to the input data can produce a completely different hash value.

This makes hashing an important technique in digital forensic investigations.

The algorithms examined in this exercise are:

- MD5
- SHA-256

## 6.2 Create Two Similar Files

Create the first file with a newline:

```bash
echo "Hello" > hello_with_newline.txt
```

Create the second file without a newline:

```bash
echo -n "Hello" > hello_without_newline.txt
```

Display the contents:

```bash
cat hello_with_newline.txt
cat hello_without_newline.txt
```

## 6.3 Examine the Hexadecimal Representation

```bash
xxd hello_with_newline.txt
```

```bash
xxd hello_without_newline.txt
```

The first file contains an additional newline byte represented by `0A`.

## 6.4 Calculate MD5 Hashes

```bash
md5sum hello_with_newline.txt hello_without_newline.txt
```

## 6.5 Calculate SHA-256 Hashes

```bash
sha256sum hello_with_newline.txt hello_without_newline.txt
```

## 6.6 Student Exercise

Create two files containing your first name.

One file should include a newline, and the other should not.

Example:

```bash
echo "Sharon" > sharon_with_newline.txt
```

```bash
echo -n "Sharon" > sharon_without_newline.txt
```

Compare their bytes:

```bash
xxd sharon_with_newline.txt
xxd sharon_without_newline.txt
```

Calculate MD5:

```bash
md5sum sharon_with_newline.txt sharon_without_newline.txt
```

Calculate SHA-256:

```bash
sha256sum sharon_with_newline.txt sharon_without_newline.txt
```

## 6.7 Hash Comparison Worksheet

| File | Newline Included? | MD5 Hash | SHA-256 Hash |
|---|---|---|---|
| sharon_with_newline.txt | Yes | Record actual result | Record actual result |
| sharon_without_newline.txt | No | Record actual result | Record actual result |

## 6.8 Forensic Explanation

The two files may display identical visible text, but their underlying bytes are different.

The additional newline character changes the file's content.

Because cryptographic hash functions operate on the actual input bytes, the MD5 and SHA-256 hash values will differ.

When an examiner needs to hash exactly the visible characters without adding a newline, `echo -n` can be used to create the file.

Hash verification is important when establishing whether evidence has remained unchanged during acquisition, examination and reporting.

<img width="1118" height="795" alt="image" src="https://github.com/user-attachments/assets/011222dc-87f3-4778-8b37-3267612d6981" />

- [ ] Screenshot 10 – File creation and hexadecimal comparison.

<img width="1053" height="796" alt="image" src="https://github.com/user-attachments/assets/79d3fa1a-eb38-47e5-95e1-182bd9248d97" />

- [ ] Screenshot 11 – MD5 hash comparison.

<img width="1086" height="812" alt="image" src="https://github.com/user-attachments/assets/ad593695-5ecb-452a-9b01-2223a9ff6262" />

- [ ] Screenshot 12 – SHA-256 hash comparison.

---

# 7. Task E – Endianness and Memory Representation

## 7.1 Introduction

Endianness describes the order in which bytes are arranged when representing multi-byte numerical values.

The two main byte-order formats are:

- **Little-endian:** The least significant byte is stored first.
- **Big-endian:** The most significant byte is stored first.

Understanding byte ordering is essential when examining raw memory, disk metadata and binary file structures.

## 7.2 Identify the System Byte Order

Execute:

```bash
python3 - <<'PY'
import sys

print('System byte order:', sys.byteorder)
PY
```

## 7.3 Interpret the Same Bytes in Two Ways

Use Python:

```bash
python3 - <<'PY'
raw = bytes.fromhex('78563412')

print('Raw bytes:', raw.hex())
print('Interpreted as little-endian:', int.from_bytes(raw, 'little'))
print('Interpreted as big-endian   :', int.from_bytes(raw, 'big'))
PY
```

## 7.4 Student Exercise

Interpret the following raw hexadecimal byte sequences:

| Raw Hex Bytes | Little-endian Decimal | Big-endian Decimal |
|---|---:|---:|
| 01000000 | Pending | Pending |
| FF000000 | Pending | Pending |
| 00000100 | Pending | Pending |
| 78563412 | Pending | Pending |

### Python Verification Script

```bash
python3 - <<'PY'
values = [
    '01000000',
    'FF000000',
    '00000100',
    '78563412'
]

for value in values:
    raw = bytes.fromhex(value)

    little = int.from_bytes(raw, 'little')
    big = int.from_bytes(raw, 'big')

    print(f'\nRaw bytes: {value}')
    print(f'Little-endian: {little}')
    print(f'Big-endian: {big}')
PY
```

## 7.5 Forensic Explanation

The same sequence of four bytes can represent different numerical values depending on the byte-order convention used.

An investigator must identify the expected byte order before interpreting binary evidence.

Incorrect byte-order interpretation can lead to incorrect conclusions about file metadata, memory values and other digital structures.

<img width="902" height="372" alt="image" src="https://github.com/user-attachments/assets/8579081f-ba18-4d53-bdd7-0c9ef1a5eedc" />

- [ ] Screenshot 13 – System byte-order identification.

<img width="1106" height="661" alt="image" src="https://github.com/user-attachments/assets/62e1d078-a38d-459c-88c2-90d3ea9ae960" />

- [ ] Screenshot 14 – Little-endian and big-endian conversion results.

---

# 8. Task F – Mini Forensic Interpretation Case

## 8.1 Case Introduction

This practical exercise presents five recovered evidence items from a fictional training investigation.

The objective is to decode and interpret each value using the techniques covered in Tasks A–E.

Each evidence item must be examined, interpreted and documented.

## 8.2 Evidence Item EVID-001

**Recovered Value:**

`48656c6c6f204943444641`

Task:

Convert the hexadecimal value into readable ASCII text.

Command:

```bash
echo "48656c6c6f204943444641" | xxd -r -p
```

Interpretation:

_To be completed after practical verification._

## 8.3 Evidence Item EVID-002

**Recovered Value:**

`01001000 01101001`

Task:

Interpret the binary values as ASCII characters.

Python command:

```bash
python3 - <<'PY'
binary = '01001000 01101001'

text = ''.join(
    chr(int(value, 2))
    for value in binary.split()
)

print('Decoded text:', text)
PY
```

Interpretation:

_To be completed after practical verification._

## 8.4 Evidence Item EVID-003

**Recovered Value:**

`1675008832`

Format:

Unix epoch timestamp.

Command:

```bash
date -u -d @1675008832
```

Python verification:

```bash
python3 - <<'PY'
from datetime import datetime, timezone

value = 1675008832

print(datetime.fromtimestamp(value, timezone.utc))
PY
```

Interpretation:

_To be completed after practical verification._

## 8.5 Evidence Item EVID-004

**Recovered Value:**

`725846400`

Format:

Mac Absolute Time.

Command:

```bash
python3 - <<'PY'
from datetime import datetime, timezone

mac_value = 725846400
unix_value = mac_value + 978307200

print(datetime.fromtimestamp(unix_value, timezone.utc))
PY
```

Interpretation:

_To be completed after practical verification._

## 8.6 Evidence Item EVID-005

**Recovered Value:**

`78563412`

Task:

Interpret the bytes using both little-endian and big-endian ordering.

Command:

```bash
python3 - <<'PY'
raw = bytes.fromhex('78563412')

print('Raw bytes:', raw.hex())
print('Little-endian:', int.from_bytes(raw, 'little'))
print('Big-endian:', int.from_bytes(raw, 'big'))
PY
```

Interpretation:

_To be completed after practical verification._

## 8.7 Mini-Case Evidence Summary

| Evidence Item | Recovered Value | Interpretation | Forensic Relevance |
|---|---|---|---|
| EVID-001 | `48656c6c6f204943444641` | Decodes from hexadecimal into the ASCII text **Hello ICDFA**. | Text and content interpretation |
| EVID-002 | `01001000 01101001` | Decodes from binary into the ASCII text **Hi**. | Binary-to-text interpretation |
| EVID-003 | `1675008832` | Unix timestamp representing **29 January 2023, 16:13:52 UTC**. | Digital forensic timeline analysis |
| EVID-004 | `725846400` | Mac Absolute Time representing **2 January 2024, 00:00:00 UTC**. | Mac forensic timestamp analysis |
| EVID-005 | `78563412` | Interpreted as **305419896** in little-endian format and **2018915346** in big-endian format. | Memory, binary data and byte-order interpretation |

## 8.8 Required Case Narrative

The recovered evidence values were examined using hexadecimal, binary, timestamp conversion and byte-order interpretation techniques.

**EVID-001 – Hexadecimal to ASCII**

The hexadecimal value `48656c6c6f204943444641` was converted into readable ASCII text using Python. The decoded result was **Hello ICDFA**. This demonstrates how hexadecimal values can represent readable text when interpreted using the appropriate character encoding.

**EVID-002 – Binary to ASCII**

The binary value `01001000 01101001` was converted into decimal character codes and then interpreted using ASCII. The result was **Hi**. This demonstrates how binary data can represent individual text characters.

**EVID-003 – Unix Timestamp**

The value `1675008832` was interpreted as a Unix timestamp. Using the Linux `date` command and Python's `datetime` module, it was converted into:

**29 January 2023, 16:13:52 UTC.**

This evidence is useful for establishing when an event occurred and placing it within a digital forensic timeline.

**EVID-004 – Mac Absolute Time**

The value `725846400` was interpreted as Mac Absolute Time, which counts seconds from 1 January 2001 at 00:00:00 UTC.

The value was converted into Unix time by adding the epoch difference of `978307200` seconds:

`725846400 + 978307200 = 1704153600`

The resulting UTC timestamp is:

**2 January 2024, 00:00:00 UTC.**

This conversion demonstrates the importance of identifying the correct timestamp format before interpreting digital evidence.

**EVID-005 – Endianness Interpretation**

The hexadecimal bytes `78563412` were interpreted using two different byte-order formats:

- **Little-endian:** `305419896`
- **Big-endian:** `2018915346`

The values differ because little-endian interpretation places the least significant byte first, while big-endian interpretation places the most significant byte first. This is important when examining memory dumps, binary files and disk metadata because an incorrect byte-order assumption can lead to an incorrect interpretation of evidence.

### Overall Forensic Relevance

The evidence items demonstrate different approaches to interpreting recovered digital data:

- **Timeline analysis:** EVID-003 and EVID-004 provide timestamp information that can help establish the timing of digital events.
- **Text and content interpretation:** EVID-001 and EVID-002 demonstrate how hexadecimal and binary values can be decoded into readable text.
- **Memory and binary interpretation:** EVID-005 demonstrates why byte order must be considered when interpreting raw hexadecimal data.

These techniques help a digital forensic examiner interpret recovered information accurately and avoid incorrect conclusions when analysing digital evidence.

---

# 9. Final Reflection

**completed after all six practical tasks were done.**

In this laboratory, I developed practical skills in interpreting digital evidence through number-system conversions, hexadecimal analysis, timestamp interpretation, cryptographic hashing and byte-order analysis.

The exercises demonstrated that digital evidence must be interpreted carefully because the same information can be represented in different formats. Understanding binary, decimal and hexadecimal conversions helps investigators interpret raw digital values accurately.

The timestamp exercises highlighted the importance of distinguishing between UTC and local time when constructing forensic timelines. The hashing exercises demonstrated how even a small change, such as an additional newline character, can alter a file's hash value.

Finally, the endianness exercises showed that byte ordering affects the interpretation of numerical values stored in memory and digital structures.

These skills are important in digital forensics because accurate interpretation, validation and documentation help maintain the reliability and integrity of investigative findings.

---

# 10. Screenshot Evidence Register

All screenshots was generated from my own Kali Linux laboratory environment.

# 10. Screenshot Evidence Register

All screenshots must be generated from my own Kali Linux laboratory environment.

| Screenshot | Description | Status |
|---|---|---|
| 01 | Linux environment and working directory | Uploaded |
| 02 | Number-system conversion commands | Uploaded |
| 03 | Python conversion validation | Uploaded |
| 04 | Text evidence and xxd output | Uploaded |
| 05 | Hexadecimal-to-ASCII conversion | Uploaded |
| 06 | Python ASCII conversion | Uploaded |
| 07 | Unix timestamp conversion | Uploaded |
| 08 | UTC and local-time comparison | Uploaded |
| 09 | Mac Absolute Time conversion | Uploaded |
| 10 | File creation and newline comparison | Uploaded |
| 11 | MD5 hash comparison | Uploaded |
| 12 | SHA-256 hash comparison | Uploaded |
| 13 | System byte-order identification | Uploaded |
| 14 | Little-endian and big-endian results | Uploaded |

---

# 11. Conclusion

This laboratory provides a practical introduction to the representation and interpretation of digital evidence.

Through the six exercises, I will demonstrate my understanding of number systems, ASCII and hexadecimal encoding, timestamps, cryptographic hashing, byte ordering and forensic evidence interpretation.

The practical results, command outputs and screenshots will provide verifiable evidence of the procedures performed.

Accurate interpretation and documentation are essential responsibilities of a digital forensics examiner because investigative conclusions must be supported by reliable technical evidence.

---

## 12. References

1. ICDFA – Course slides: *Understand Number Systems and Timestamps*.
2. GNU Bash Manual – Shell Arithmetic: https://www.gnu.org/software/bash/manual/html_node/Shell-Arithmetic.html
3. Linuxize – Bash printf Command: https://linuxize.com/post/bash-printf-command/
4. Unicode – https://en.wikipedia.org/wiki/Unicode
5. Coordinated Universal Time – https://en.wikipedia.org/wiki/Coordinated_Universal_Time
6. Why MD5 Hashes Differ When Using Echo: https://stackoverflow.com/questions/5693360/why-is-an-md5-hash-created-by-python-different-from-one-created-using-echo-and-m
7. IAR – Migration Techniques for Different Endianness: https://www.iar.com/knowledge/learn/programming/migration-techniques-for-different-endianness/

---

**Course:** CIP-B101 – Basic Computer Skills for Digital Forensics  
**Laboratory:** Lab 1A – Number Systems, ASCII, Timestamps and Data Representation  
**Student:** Sharon Omowumi  
**Institution:** International Cybersecurity and Digital Forensics Academy (ICDFA)

**End of Laboratory README**
