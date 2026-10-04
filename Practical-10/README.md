# Practical 10: Sum of Integers until a Negative Number is Read

## Aim
To write an assembly program that reads integers and adds them until a negative non-zero number is read, then outputs the sum (not including the last number).

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
This is a sentinel-controlled loop: the negative number marks the end of the data. After each `INP`, `SPA` skips the exit branch when AC ≥ 0; for a negative number the skip does not happen and `BUN DONE` leaves the loop before the number is added. Zero counts as non-negative, so it is added (it does not change the sum).

```
        +--------------------------------------------+
        v                                            |
LOOP:  INP --> AC >= 0 ? --yes--> SUM = SUM + AC ----+
                  |
                  no
                  v
DONE:  OUT SUM --> HLT
```

## Program
File: [P10_SUM_UNTIL_NEGATIVE.a](P10_SUM_UNTIL_NEGATIVE.a)

```
LOOP:   INP             ; AC <- next number
        SPA             ; if AC >= 0 skip the exit branch
        BUN DONE        ; AC < 0 : leave the loop
        ADD SUM         ; AC <- AC + SUM
        STA SUM         ; SUM <- AC
        BUN LOOP        ; read the next number
DONE:   LDA SUM         ; AC <- SUM
        OUT             ; display the sum
        HLT
SUM:    .data 1 0       ; running total
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/0c8f5c5c-6ef8-4cf9-ac44-336d8854827f" />


### Step 3 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/b94957e6-8907-4b46-811d-c416e4e3fa18" />


### Step 4 – Run (Ctrl+R) and enter 4, 10, 0, 6 and finally -3
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/6865132b-151a-46cb-9c54-40d0ee795251" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/f0cdfce2-a9ea-45a8-8b10-048dcd95a845" />


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
|---|---|---|---|---|---|
| 0 | 000 | F800 | LOOP | INP | AC ← next number |
| 1 | 001 | 7010 | | SPA | if AC ≥ 0 skip the exit branch |
| 2 | 002 | 4006 | | BUN DONE | AC < 0: leave the loop |
| 3 | 003 | 1009 | | ADD SUM | AC ← AC + SUM |
| 4 | 004 | 3009 | | STA SUM | SUM ← AC |
| 5 | 005 | 4000 | | BUN LOOP | read the next number |
| 6 | 006 | 2009 | DONE | LDA SUM | AC ← SUM |
| 7 | 007 | F400 | | OUT | display the sum |
| 8 | 008 | 7001 | | HLT | |
| 9 | 009 | 0000 | SUM | .data 1 0 | running total |

Sample runs:

| Input(s) typed | Output displayed |
|---|---|
| 4, 10, 0, 6, -3 | 20 |
| -5 | 0 |
| 7, 8, -1 | 15 |

<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/00f9ac8c-286f-4030-99a7-8a170eb10e34" />
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/5fd2ab46-a648-4a18-a8bd-1fae7fbb9d81" />


For the inputs 4, 10, 0, 6, −3 the loop runs four times; SUM (address 9) holds 0014 hex = 20. If the first number is negative, the sum is 0.

## Result
The program adds integers until a negative number is read and displays the sum excluding it: 4 + 10 + 0 + 6 = **20**.
