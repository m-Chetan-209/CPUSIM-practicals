# Practical 4: SUBTRACT Operation on Two User-entered Numbers

## Aim
To write an assembly program that reads two numbers A and B and displays A − B.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
The Basic Computer has no subtract instruction. Subtraction uses the two's complement: A − B = A + (B′ + 1). `CMA` forms the 1's complement B′ and `INC` adds 1, giving −B, which is then added to A with `ADD`.

```
A      = 18 = 0000 0000 0001 0010
B      = 50 = 0000 0000 0011 0010
B'          = 1111 1111 1100 1101  (CMA)
B' + 1      = 1111 1111 1100 1110  (INC) = -50
A + (-B)    = 1111 1111 1110 0000  = -32
```

After the addition, E = 1 means there was a carry out, i.e. A ≥ B when the numbers are treated as unsigned.

## Program
File: [P04_SUBTRACT.a](P04_SUBTRACT.a)

```
        INP         ; AC <- A (minuend)
        STA A       ; M[A] <- AC
        INP         ; AC <- B (subtrahend)
        CMA         ; AC <- AC' (1's complement of B)
        INC         ; AC <- AC + 1 (2's complement of B = -B)
        ADD A       ; AC <- A + (-B) = A - B
        STA DIFF    ; M[DIFF] <- AC
        OUT         ; display the difference
        HLT
A:      .data 1 0   ; minuend
DIFF:   .data 1 0   ; result
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/65ee3fa1-c3b3-4173-a095-7153f02304b5" />

### Step 3 – Set the RAM pane to Hex

### Step 4 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/962fd65c-baaa-4a70-9bf5-e8ecd429913e" />

### Step 5 – Run (Ctrl+R) and enter the minuend 18, then the subtrahend 50
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/c7bf9a2b-6b2d-4051-a949-5344a7966ce8" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/8e478544-579a-40d5-85be-663bd33bf2c1" />


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction |
|---|---|---|---|---|
| 0 | 000 | F800 | | INP |
| 1 | 001 | 3009 | | STA A |
| 2 | 002 | F800 | | INP |
| 3 | 003 | 7200 | | CMA |
| 4 | 004 | 7020 | | INC |
| 5 | 005 | 1009 | | ADD A |
| 6 | 006 | 300A | | STA DIFF |
| 7 | 007 | F400 | | OUT |
| 8 | 008 | 7001 | | HLT |
| 9 | 009 | 0000 | A | .data 1 0 |
| 10 | 00A | 0000 | DIFF | .data 1 0 |

Sample runs:

| Input(s) typed | Output displayed |
|---|---|
| 50, 18 | 32 |
| 18, 50 | -32 |
| -7, -7 | 0 |
| 0, 1 | -1 |


<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/54ddacf5-cd26-40f6-addb-e526207c32e5" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/7792f092-5ec8-4348-bd55-5c5f51357dfc" />


For 18 − 50 the final AC is −32 (FFE0 hex), DR = 18 and PC = 9. For −7 − (−7) the output is 0.

## Result
The program subtracts two user-entered numbers using the 2's complement; 18 − 50 = **−32** and 50 − 18 = **32**.
