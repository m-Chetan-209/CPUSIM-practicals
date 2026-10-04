# Practical 11: Sum of Integers until Zero is Read

## Aim
To write an assembly program that reads integers and adds them until zero is read, then outputs the sum.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
Here the sentinel is 0. `SZA` skips the next instruction when AC = 0. Because a skip can only jump over one instruction, two branches are used: when AC ≠ 0 the `BUN ADDIT` executes and the number is added; when AC = 0 that branch is skipped and `BUN DONE` ends the loop. Negative numbers are added normally.

## Program
File: [P11_SUM_UNTIL_ZERO.a](P11_SUM_UNTIL_ZERO.a)

```
LOOP:   INP             ; AC <- next number
        SZA             ; if AC != 0 do not skip ...
        BUN ADDIT       ; ... so go and add it
        BUN DONE        ; AC = 0 (BUN ADDIT was skipped) : finish
ADDIT:  ADD SUM         ; AC <- AC + SUM
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
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/18ad308f-fe2d-438b-8267-98d8bb635bb7" />


### Step 3 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/d3138610-8f36-458b-b5b9-62ebe20e5a78" />


### Step 4 – Run (Ctrl+R) and enter 8, 12, -5 and 0
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/dda9e705-ce9b-42cf-a487-9aa1e01b2dcd" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/857e9af8-526d-4400-abd4-022ef1c2d288" />


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
|---|---|---|---|---|---|
| 0 | 000 | F800 | LOOP | INP | AC ← next number |
| 1 | 001 | 7004 | | SZA | if AC ≠ 0 do not skip |
| 2 | 002 | 4004 | | BUN ADDIT | go and add it |
| 3 | 003 | 4007 | | BUN DONE | AC = 0 (BUN ADDIT was skipped): finish |
| 4 | 004 | 100A | ADDIT | ADD SUM | AC ← AC + SUM |
| 5 | 005 | 300A | | STA SUM | SUM ← AC |
| 6 | 006 | 4000 | | BUN LOOP | read the next number |
| 7 | 007 | 200A | DONE | LDA SUM | AC ← SUM |
| 8 | 008 | F400 | | OUT | display the sum |
| 9 | 009 | 7001 | | HLT | |
| 10 | 00A | 0000 | SUM | .data 1 0 | running total |

Sample runs:

| Input(s) typed | Output displayed |
|---|---|
| 8, 12, -5, 0 | 15 |
| 0 | 0 |
| 100, 200, 300, 0 | 600 |


<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/a3c486d2-2286-4385-b9c6-8616cb65659a" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/f240255f-a4fc-418a-8eba-8f72814506fd" />


For 8, 12, −5, 0 the final AC and SUM are 15. E = 1 in the final screenshot because adding −5 (FFFB hex) to 20 produces a carry out of bit 15. In Dec mode a 1-bit register holding 1 is shown as −1.

## Result
The program adds integers until 0 is read and displays the sum: 8 + 12 + (−5) = **15**.
