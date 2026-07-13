# ATmega128 Assembly – 1

**Author:** Zanyar Erkozan

---

## 📋 Overview

**ATmega128 microcontroller assembly programming** lab implemented and simulated in AVR Studio. Covers fundamental operations including arithmetic, SRAM data transfer, register manipulation, and SREG flag analysis.

---

## 🔧 Task 1 – Addition, SRAM Storage & SPH Register Write

### Code
```asm
.include "m128def.inc"   ; ATmega128 definitions
.cseg
.org 0x00

LDI R16, 0x2A            ; Load 0x2A into R16
LDI R17, 0x5C            ; Load 0x5C into R17
ADD R16, R17             ; R16 = 0x2A + 0x5C = 0x86
STS 0x0125, R16          ; Store result to SRAM[0x0125]
LDS R18, 0x0125          ; Load back from SRAM into R18
OUT SPH, R18             ; Write 0x86 to Stack Pointer High register
HERE: RJMP HERE          ; Infinite loop (halt)
```

### Result
- `R16 = 0x86` (sum of 0x2A + 0x5C)
- `SRAM[0x0125] = 0x86`
- `SPH = 0x86`

Demonstrates: **addition → SRAM store → SRAM load → I/O register write**

---

## 🔧 Task 2 – Subtraction & SREG Flag Analysis

### Code
```asm
.include "m128def.inc"
.cseg
.org 0x0000

EOR R1, R1               ; Clear R1
OUT SREG, R1             ; Initialize SREG = 0
LDI R16, 0x09            ; Load minuend
LDI R17, 0x05            ; Load subtrahend
SUB R16, R17             ; R16 = 0x09 - 0x05 = 0x04
IN  R20, SREG            ; Save SREG into R20
OUT SREG, R1             ; Clear SREG
NOP                      ; Halt for simulation
```

### SREG Flags After `0x09 - 0x05 = 0x04`

| Flag | Value | Reason |
|---|---|---|
| **C** (Carry) | 0 | No borrow (9 ≥ 5) |
| **H** (Half Carry) | 0 | No borrow between bit 3 and 4 |
| **Z** (Zero) | 0 | Result is not zero |
| **N** (Negative) | 0 | MSB of 0x04 is 0 |
| **V** (Overflow) | 0 | No signed overflow |

---

## 🛠️ How to Run

1. Open **AVR Studio** (or Microchip Studio)
2. Create a new AVR Assembler project targeting **ATmega128**
3. Paste the task code into the editor
4. Use the built-in simulator to step through and inspect registers/SRAM

---

## 📁 Files

| File | Description |
|---|---|
| `lab1zanyarerkozan2584985 (2).pdf` | Full lab report with screenshots and register/memory state tables |

---

## 📚 Concepts Demonstrated

- AVR assembly syntax and directives (`.include`, `.cseg`, `.org`)
- Immediate load (`LDI`) and register arithmetic (`ADD`, `SUB`)
- SRAM data storage and retrieval (`STS`, `LDS`)
- I/O register access (`OUT`, `IN`)
- SREG status flag analysis after arithmetic operations
- Infinite loop / program halt (`RJMP HERE`)
