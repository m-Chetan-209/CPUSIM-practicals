# Practical 5: Logical Operations: AND, OR, NOT, XOR, NOR, NAND

## Aim
To write an assembly program that performs AND, OR, NOT, XOR, NOR and NAND on two user-entered numbers.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
The Basic Computer provides only two logic instructions: `AND` (memory-reference, AC ← AC ∧ M[addr]) and `CMA` (register-reference, AC ← AC′). Since {AND, NOT} is functionally complete, every other operation can be built from them with Boolean algebra, applied to all 16 bits at once:

| Operation | Boolean identity used | Instruction sequence |
|---|---|---|
| A AND B | A·B | LDA A, AND B |
| A OR B | (A′·B′)′ (De Morgan) | LDA B, CMA, STA NB, LDA A, CMA, AND NB, CMA |
| NOT A | A′ | LDA A, CMA |
| A XOR B | (A + B)·(A·B)′ | LDA RAND, CMA, AND ROR |
| A NOR B | (A + B)′ | LDA ROR, CMA |
| A NAND B | (A·B)′ | LDA RAND, CMA |

Results are displayed as signed decimal numbers. Complementing a small positive number sets its sign bit, so NOT 12 = −13 (in two's complement, x′ = −x − 1).

## Program
File: [P05_LOGIC.a](P05_LOGIC.a)

Outputs appear in this order: AND, OR, NOT A, NOT B, XOR, NOR, NAND.

```
        INP             ; AC <- A
        STA A
        INP             ; AC <- B
        STA B
        LDA A           ; AC <- A
        AND B           ; AC <- A . B
        STA RAND
        OUT             ; output 1 : A AND B
        LDA B
        CMA             ; AC <- B'
        STA NB          ; NB <- B'
        LDA A
        CMA             ; AC <- A'
        STA NA          ; NA <- A'
        AND NB          ; AC <- A' . B'
        CMA             ; AC <- (A' . B')' = A + B
        STA ROR
        OUT             ; output 2 : A OR B
        LDA NA
        OUT             ; output 3 : NOT A
        LDA NB
        OUT             ; output 4 : NOT B
        LDA RAND
        CMA             ; AC <- (A . B)' = NAND
        STA RNAND
        AND ROR         ; AC <- (A + B) . (A . B)'
        STA RXOR
        OUT             ; output 5 : A XOR B
        LDA ROR
        CMA             ; AC <- (A + B)'
        STA RNOR
        OUT             ; output 6 : A NOR B
        LDA RNAND
        OUT             ; output 7 : A NAND B
        HLT
A:      .data 1 0
B:      .data 1 0
NA:     .data 1 0
NB:     .data 1 0
RAND:   .data 1 0
ROR:    .data 1 0
RXOR:   .data 1 0
RNOR:   .data 1 0
RNAND:  .data 1 0
```

The full file with all comments is in `P05_LOGIC.a`.

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/3889f322-61fd-481d-ad05-964a7aca3810" />

### Step 3 – Set the RAM pane to Hex

### Step 4 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/29197d53-3a7d-4350-ad3f-1329933ebdd2" />

### Step 5 – Run (Ctrl+R) and enter 12, then 10
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/887bf5f9-d8b1-49b3-bcf6-f69013fddeb8" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/e692269d-2d82-4542-9ff1-3a5afdb3f3e3" />


### Step 6 – Read the seven outputs in the console (scroll up to see all)
<img width="1920" height="1080" alt="real-5" src="https://github.com/user-attachments/assets/783181e0-70f4-45e6-92f7-598c7316cbfe" />



## Observations

Results for A = 12, B = 10:

| Operation | 16-bit result (binary) | Hex | Decimal output |
|---|---|---|---|
| A = 12 | 0000 0000 0000 1100 | 000C | 12 |
| B = 10 | 0000 0000 0000 1010 | 000A | 10 |
| A AND B | 0000 0000 0000 1000 | 0008 | 8 |
| A OR B | 0000 0000 0000 1110 | 000E | 14 |
| NOT A | 1111 1111 1111 0011 | FFF3 | -13 |
| NOT B | 1111 1111 1111 0101 | FFF5 | -11 |
| A XOR B | 0000 0000 0000 0110 | 0006 | 6 |
| A NOR B | 1111 1111 1111 0001 | FFF1 | -15 |
| A NAND B | 1111 1111 1111 0111 | FFF7 | -9 |

Sample runs:

| Input(s) typed | Output displayed |
|---|---|
| 12, 10 | 8, 14, -13, -11, 6, -15, -9 |
| 5, 3 | 1, 7, -6, -4, 6, -8, -2 |
| 255, 15 | 15, 255, -256, -16, 240, -256, -16 |

<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/bd90dfab-6a83-4947-a5e8-22bf46267e7a" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/8e87023f-0388-411a-86cc-69be8598c106" />
<img width="1920" height="1080" alt="real-5" src="https://github.com/user-attachments/assets/2746106d-8990-4f87-90ab-49d07e46d04d" />



## Result
All six logical operations were simulated using only `AND` and `CMA`. For A = 12 and B = 10 the outputs are AND = 8, OR = 14, NOT A = −13, NOT B = −11, XOR = 6, NOR = −15, NAND = −9.
