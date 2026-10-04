# Practical 3: ADD Operation on Two User-entered Numbers

## Aim
To write an assembly program that reads two numbers entered by the user, adds them and displays the sum.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
`INP` reads an integer into AC. `STA A` saves it in memory because the next `INP` overwrites AC. `ADD A` is a memory-reference instruction: DR ← M[A], then AC ← AC + DR, and the carry out of bit 15 goes to E. `OUT` displays AC and `HLT` stops the machine.

Numbers are 16-bit two's complement, so the range is −32768 to +32767. A sum outside this range overflows and wraps around.

## Program
File: [P03_ADD.a](P03_ADD.a)

```
        INP         ; AC <- first number typed by the user
        STA A       ; M[A] <- AC (save first number)
        INP         ; AC <- second number
        ADD A       ; AC <- AC + M[A], E <- carry out
        STA SUM     ; M[SUM] <- AC (save the result)
        OUT         ; display AC (the sum)
        HLT         ; stop
A:      .data 1 0   ; first number
SUM:    .data 1 0   ; result
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="first" src="https://github.com/user-attachments/assets/4424661e-9976-476b-84f2-ace576322c34" />

### Step 3 – Set the RAM pane to Hex

### Step 4 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="second" src="https://github.com/user-attachments/assets/283570f7-393c-4bc6-9351-c55073573246" />

### Step 5 – Run (Ctrl+R) and enter the two numbers
<img width="1920" height="1080" alt="third" src="https://github.com/user-attachments/assets/f36bf756-a166-4e8d-922b-9c6aa74bfd7e" />


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction |
|---|---|---|---|---|
| 0 | 000 | F800 | | INP |
| 1 | 001 | 3007 | | STA A |
| 2 | 002 | F800 | | INP |
| 3 | 003 | 1007 | | ADD A |
| 4 | 004 | 3008 | | STA SUM |
| 5 | 005 | F400 | | OUT |
| 6 | 006 | 7001 | | HLT |
| 7 | 007 | 0000 | A | .data 1 0 |
| 8 | 008 | 0000 | SUM | .data 1 0 |

Sample runs:

| Input(s) typed | Output displayed |
|---|---|
| 25, 17 | 42 |
| -40, 15 | -25 |
| -1, 1 | 0 |
| 30000, 10000 | -25536 |

<img width="1920" height="1080" alt="third" src="https://github.com/user-attachments/assets/eae338fc-e59e-4f2c-aa46-ba1fa3039ab0" />
<img width="1920" height="1080" alt="fourth" src="https://github.com/user-attachments/assets/66725c69-70d7-4ffa-b315-8dcdb9316283" />


With inputs 25 and 17 the final registers are AC = 42, DR = 25, PC = 7, IR = 28673 (7001 hex = HLT) and E = 0. Memory holds A = 0019 hex and SUM = 002A hex. For −1 + 1 the result is 0 with E = 1 (carry out). For 30000 + 10000 the true sum 40000 does not fit in 16-bit two's complement, so the output is 40000 − 65536 = −25536 (overflow).

## Result
The program correctly adds two user-entered numbers; 25 + 17 = **42**.

**Q5. Which instructions in this program are memory-reference instructions?**
Ans. STA and ADD. INP, OUT and HLT use fixed codes F800, F400 and 7001.
