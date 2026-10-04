# Practical 6: Memory-reference Instructions: ADD, LDA, STA, BUN, ISZ

## Aim
To write an assembly program that simulates the memory-reference instructions ADD, LDA, STA, BUN and ISZ.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
A memory-reference instruction has an opcode 0–6 and a 12-bit address. During fetch AR ← IR(0–11), so from the execute phase onwards AR holds the address of the operand (the effective address, since I = 0).

| Symbol | Code | Execute micro-operations |
|---|---|---|
| ADD | 1xxx | DR ← M[AR]; AC ← AC + DR, E ← Cout |
| LDA | 2xxx | DR ← M[AR]; AC ← DR |
| STA | 3xxx | M[AR] ← AC |
| BUN | 4xxx | PC ← AR |
| ISZ | 6xxx | DR ← M[AR]; DR ← DR + 1; M[AR] ← DR; if DR = 0 then PC ← PC + 1 |

The program multiplies X = 5 by N = 3 by repeated addition. CTR starts at −3. Each pass adds X to PROD and `ISZ CTR` increments CTR. While CTR is not zero, the next instruction `BUN LOOP` repeats the loop. When CTR becomes 0, ISZ skips the BUN and the program ends with PROD = 15.

## Program
File: [P06_MEMORY_REFERENCE.a](P06_MEMORY_REFERENCE.a)

```
LOOP:   LDA PROD        ; AC <- M[PROD]
        ADD X           ; AC <- AC + M[X]
        STA PROD        ; M[PROD] <- AC
        ISZ CTR         ; M[CTR] <- M[CTR] + 1; skip next if it became 0
        BUN LOOP        ; PC <- LOOP (repeat)
        LDA PROD        ; AC <- final product
        HLT
X:      .data 1 5       ; multiplicand
CTR:    .data 1 -3      ; -N (loop counter)
PROD:   .data 1 0       ; product
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/ce096f2a-698a-4da0-9881-4915b6e10e13" />


### Step 3 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/25aa48d4-eeef-4550-834e-d720cc3fda3c" />


### Step 4 – Enter debug mode (Ctrl+D)
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/74e928e4-77b3-4a52-b5c0-3ceb31d00984" />


### Step 5 – Set the Registers pane to Unsigned Dec and the RAM pane to Hex
<img width="1920" height="1080" alt="step5" src="https://github.com/user-attachments/assets/dab67eb3-0ffe-4641-8d7f-4a3edbbe5379" />


### Step 6 – Click Step by Instr 16 times and note the registers after each step
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/dd87cf82-f776-4287-87cb-9e8637cc0cf8" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/067e3f47-6aca-4ce3-ac2d-b43eff3f602a" />
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/e96b2db1-9f8c-4be7-be41-828b2224e9f0" />
<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/bd724c09-8b6d-4f59-973b-c3d71fde50f7" />



## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction |
|---|---|---|---|---|
| 0 | 000 | 2009 | LOOP | LDA PROD |
| 1 | 001 | 1007 | | ADD X |
| 2 | 002 | 3009 | | STA PROD |
| 3 | 003 | 6008 | | ISZ CTR |
| 4 | 004 | 4000 | | BUN LOOP |
| 5 | 005 | 2009 | | LDA PROD |
| 6 | 006 | 7001 | | HLT |
| 7 | 007 | 0005 | X | .data 1 5 |
| 8 | 008 | FFFD | CTR | .data 1 -3 |
| 9 | 009 | 0000 | PROD | .data 1 0 |

Register contents (decimal) after each instruction:

| Step | PC before | Instruction | IR (hex) | AC | DR | E | PC | AR | IR (dec) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 0 | LDA PROD | 2009 | 0 | 0 | 0 | 1 | 9 | 8201 |
| 2 | 1 | ADD X | 1007 | 5 | 5 | 0 | 2 | 7 | 4103 |
| 3 | 2 | STA PROD | 3009 | 5 | 5 | 0 | 3 | 9 | 12297 |
| 4 | 3 | ISZ CTR | 6008 | 5 | 65534 (-2) | 0 | 4 | 8 | 24584 |
| 5 | 4 | BUN LOOP | 4000 | 5 | 65534 (-2) | 0 | 0 | 0 | 16384 |
| 6 | 0 | LDA PROD | 2009 | 5 | 5 | 0 | 1 | 9 | 8201 |
| 7 | 1 | ADD X | 1007 | 10 | 5 | 0 | 2 | 7 | 4103 |
| 8 | 2 | STA PROD | 3009 | 10 | 5 | 0 | 3 | 9 | 12297 |
| 9 | 3 | ISZ CTR | 6008 | 10 | 65535 (-1) | 0 | 4 | 8 | 24584 |
| 10 | 4 | BUN LOOP | 4000 | 10 | 65535 (-1) | 0 | 0 | 0 | 16384 |
| 11 | 0 | LDA PROD | 2009 | 10 | 10 | 0 | 1 | 9 | 8201 |
| 12 | 1 | ADD X | 1007 | 15 | 5 | 0 | 2 | 7 | 4103 |
| 13 | 2 | STA PROD | 3009 | 15 | 5 | 0 | 3 | 9 | 12297 |
| 14 | 3 | ISZ CTR | 6008 | 15 | 0 | 0 | 5 | 8 | 24584 |
| 15 | 5 | LDA PROD | 2009 | 15 | 15 | 0 | 6 | 9 | 8201 |
| 16 | 6 | HLT | 7001 | 15 | 15 | 0 | 7 | 1 | 28673 |

<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/7daf912c-9b01-4861-9216-cc36ca5dc46d" />

Values in brackets are the signed interpretation of 16-bit numbers. Final memory: X = 5, CTR = 0, PROD = 15.

## Result
The memory-reference instructions were simulated: LDA, ADD and STA computed the running product, ISZ counted the passes and skipped the branch when the counter reached zero, and BUN formed the loop. 
Final AC = PROD = **15**.
