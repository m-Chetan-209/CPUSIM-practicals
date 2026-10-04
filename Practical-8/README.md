# Practical 8: Register-reference Instructions: INC, SPA, SNA, SZE

## Aim
To simulate INC, SPA, SNA and SZE and determine AC, E, PC, AR and IR in decimal after execution.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory

| Symbol | Code (hex) | Micro-operation |
|---|---|---|
| INC | 7020 | AC ← AC + 1 |
| SPA | 7010 | if AC(15) = 0 (AC positive or zero) then PC ← PC + 1 |
| SNA | 7008 | if AC(15) = 1 (AC negative) then PC ← PC + 1 |
| SZE | 7002 | if E = 0 then PC ← PC + 1 |

A skip instruction increments PC once more when its condition is true, so the next instruction is not executed. In the program every skip instruction is followed by a `HLT` "trap": the program reaches its last instruction only if every skip works. AC starts at −2 so that both a negative and a non-negative value are tested.

## Program
File: [P08_REGISTER_REF_INC_SPA_SNA_SZE.a](P08_REGISTER_REF_INC_SPA_SNA_SZE.a)

```
        LDA NUM         ; set-up: AC <- -2
        INC             ; 7020 : AC <- AC + 1          (-2 -> -1)
        SNA             ; 7008 : AC < 0 (negative) -> skip next
        HLT             ;        (skipped)
        INC             ; 7020 : AC <- AC + 1          (-1 -> 0)
        SPA             ; 7010 : AC(15) = 0 (positive) -> skip next
        HLT             ;        (skipped)
        SZE             ; 7002 : E = 0 -> skip next
        HLT             ;        (skipped)
        INC             ; 7020 : AC <- AC + 1          (0 -> 1)
        HLT             ; 7001 : halt
NUM:    .data 1 -2
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/f084e6a7-064b-46ba-9373-6f67a86a2964" />

### Step 3 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/f5919a96-90b0-4572-81f2-0069669d1215" />


### Step 4 – Enter debug mode (Ctrl+D)
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/3a7428c5-51d8-4ad3-ab25-188a42b67773" />

### Step 5 – Set the Registers pane to Unsigned Dec and the RAM pane to Hex

### Step 6 – Click Step by Instr 8 times and note AC, E, PC, AR and IR after each step


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
|---|---|---|---|---|---|
| 0 | 000 | 200B | | LDA NUM | set-up: AC ← −2 |
| 1 | 001 | 7020 | | INC | AC ← AC + 1 (−2 → −1) |
| 2 | 002 | 7008 | | SNA | AC < 0 → skip next |
| 3 | 003 | 7001 | | HLT | (skipped) |
| 4 | 004 | 7020 | | INC | AC ← AC + 1 (−1 → 0) |
| 5 | 005 | 7010 | | SPA | AC(15) = 0 → skip next |
| 6 | 006 | 7001 | | HLT | (skipped) |
| 7 | 007 | 7002 | | SZE | E = 0 → skip next |
| 8 | 008 | 7001 | | HLT | (skipped) |
| 9 | 009 | 7020 | | INC | AC ← AC + 1 (0 → 1) |
| 10 | 00A | 7001 | | HLT | halt |
| 11 | 00B | FFFE | NUM | .data 1 -2 | |

Register contents (decimal) after each instruction:

| Step | PC before | Instruction | IR (hex) | AC | E | PC | AR | IR (dec) |
|---|---|---|---|---|---|---|---|---|
| 1 | 0 | LDA NUM | 200B | 65534 (-2) | 0 | 1 | 11 | 8203 |
| 2 | 1 | INC | 7020 | 65535 (-1) | 0 | 2 | 32 | 28704 |
| 3 | 2 | SNA | 7008 | 65535 (-1) | 0 | 4 | 8 | 28680 |
| 4 | 4 | INC | 7020 | 0 | 0 | 5 | 32 | 28704 |
| 5 | 5 | SPA | 7010 | 0 | 0 | 7 | 16 | 28688 |
| 6 | 7 | SZE | 7002 | 0 | 0 | 9 | 2 | 28674 |
| 7 | 9 | INC | 7020 | 1 | 0 | 10 | 32 | 28704 |
| 8 | 10 | HLT | 7001 | 1 | 0 | 11 | 1 | 28673 |

The instructions at addresses 3, 6 and 8 were never executed: the PC column goes 2 → 4, 5 → 7 and 7 → 9.

<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/f8ce25d9-d2dc-4c80-9714-f749f155dc58" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/7542294a-166a-4dc6-929f-be7615520954" />
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/64d25319-218f-486e-83c8-b3b816a29498" />
<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/76f195ef-f200-4795-8556-9fdd3f24c70c" />
<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/6ad905fb-819c-438b-af6b-57fe75b5ec11" />
<img width="1920" height="1080" alt="9" src="https://github.com/user-attachments/assets/382481ec-fe84-48c4-846b-8634dd693c3d" />
<img width="1920" height="1080" alt="10" src="https://github.com/user-attachments/assets/a946cf8b-b3c2-4883-bd53-c5bf08da559b" />
<img width="1920" height="1080" alt="11" src="https://github.com/user-attachments/assets/2e590036-de80-4e01-a7c3-2e6f2c62480b" />



Final register contents after HLT:

| Register | Final value (decimal) |
|---|---|
| AC | 1 |
| E | 0 |
| PC | 11 |
| AR | 1 |
| IR | 28673 (7001 hex) |

## Result
INC, SPA, SNA and SZE were simulated and every skip was verified. After execution: **AC = 1, E = 0, PC = 11, AR = 1, IR = 28673**.
