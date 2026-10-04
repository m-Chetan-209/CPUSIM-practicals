# Practical 7: Register-reference Instructions: CLA, CMA, CME, HLT

## Aim
To simulate the register-reference instructions CLA, CMA, CME and HLT and determine AC, E, PC, AR and IR in decimal after execution.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
Register-reference instructions have the code 7xxx: opcode 111 with I = 0. The low 12 bits select one operation on AC or E, executed with no memory access. Because the fetch routine always performs AR ← IR(0–11), AR ends up holding the low 12 bits of the instruction code (for example 800 hex = 2048 for CLA).

| Symbol | Code (hex) | Bit set in IR(0–11) | Micro-operation |
|---|---|---|---|
| CLA | 7800 | B11 | AC ← 0 |
| CMA | 7200 | B9 | AC ← AC′ |
| CME | 7100 | B8 | E ← E′ |
| HLT | 7001 | B0 | S ← 1 (halt) |

An `LDA NUM` (AC ← 25) is placed first so that the effect of CLA is visible.

## Program
File: [P07_REGISTER_REF_CLA_CMA_CME_HLT.a](P07_REGISTER_REF_CLA_CMA_CME_HLT.a)

```
        LDA NUM         ; set-up: AC <- 25 so that CLA has something to clear
        CLA             ; 7800 : AC <- 0
        CMA             ; 7200 : AC <- AC' (0000 -> FFFF = -1)
        CME             ; 7100 : E <- E' (0 -> 1)
        HLT             ; 7001 : S <- 1 (halt)
NUM:    .data 1 25
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/700de5c7-e41c-45e6-b43c-6f4f4c142800" />


### Step 3 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/9d93b5f6-0dd1-42c1-b44e-91675312c3ca" />


### Step 4 – Enter debug mode (Ctrl+D)
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/65a42103-3f1f-41bd-ae62-ad8d136c3a5c" />

### Step 5 – Set the Registers pane to Unsigned Dec and the RAM pane to Hex

### Step 6 – Click Step by Instr 5 times and note AC, E, PC, AR and IR after each step
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/dabce945-aab8-456e-8b0a-a98babdbfd9b" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/4133331a-362d-421d-9aca-e13a003b3c93" />
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/28278af0-eaeb-4e48-93e6-49a2a04acdd8" />
<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/e2aae9f0-c2df-4467-94a5-2ca5eeaf7962" />
<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/a1714293-18ab-4b4c-bfa8-c302e2bdf12c" />


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction |
|---|---|---|---|---|
| 0 | 000 | 2005 | | LDA NUM |
| 1 | 001 | 7800 | | CLA |
| 2 | 002 | 7200 | | CMA |
| 3 | 003 | 7100 | | CME |
| 4 | 004 | 7001 | | HLT |
| 5 | 005 | 0019 | NUM | .data 1 25 |

Register contents (decimal) after each instruction:

| Step | PC before | Instruction | IR (hex) | AC | E | PC | AR | IR (dec) |
|---|---|---|---|---|---|---|---|---|
| 1 | 0 | LDA NUM | 2005 | 25 | 0 | 1 | 5 | 8197 |
| 2 | 1 | CLA | 7800 | 0 | 0 | 2 | 2048 | 30720 |
| 3 | 2 | CMA | 7200 | 65535 (-1) | 0 | 3 | 512 | 29184 |
| 4 | 3 | CME | 7100 | 65535 (-1) | 1 | 4 | 256 | 28928 |
| 5 | 4 | HLT | 7001 | 65535 (-1) | 1 | 5 | 1 | 28673 |

Final register contents after HLT:

| Register | Decimal | Hex | Explanation |
|---|---|---|---|
| AC | 65535 (signed −1) | FFFF | CLA cleared it, CMA complemented all bits |
| E | 1 | 1 | CME complemented E from 0 to 1 |
| PC | 5 | 005 | Address after HLT (HLT is at address 4) |
| AR | 1 | 001 | IR(0–11) of HLT = 001 |
| IR | 28673 | 7001 | Code of HLT |

<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/20171bda-2bd3-46f2-b3e9-bd13ab115ea2" />


## Result
DR remains= 25
After execution: **AC = 65535 (−1), E = 1, PC = 5, AR = 1, IR = 28673, DR = 25**.
