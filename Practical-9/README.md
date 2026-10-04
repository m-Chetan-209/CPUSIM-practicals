# Practical 9: Register-reference Instructions: CIR, CIL

## Aim
To simulate CIR and CIL and determine AC, E, PC, AR and IR in decimal after execution.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
CIR and CIL circulate (rotate) the 17-bit combination of E and AC by one position:

```
CIR (7080): E -> AC(15) -> AC(14) -> ... -> AC(0) -> E     (rotate right)
CIL (7040): E <- AC(15) <- AC(14) <- ... <- AC(0) <- E     (rotate left)
```

No bit is lost, so a CIR followed by a CIL restores the original AC and E. In CPU Sim each rotation uses a 1-bit scratch register TMP: the bit leaving AC is saved in TMP, AC is shifted, the old E enters the vacated bit, and TMP is copied into E.

| | E | AC |
|---|---|---|
| start | 0 | 0000 0000 0000 1001 = 9 |
| CIR | 1 | 0000 0000 0000 0100 = 4 (bit 0 of AC went to E) |
| CIR | 0 | 1000 0000 0000 0010 = 32770 (old E = 1 entered AC(15)) |
| CIL | 1 | 0000 0000 0000 0100 = 4 |
| CIL | 0 | 0000 0000 0000 1001 = 9 (original value restored) |

## Program
File: [P09_REGISTER_REF_CIR_CIL.a](P09_REGISTER_REF_CIR_CIL.a)

```
        LDA NUM         ; set-up: AC <- 9 = 0000 0000 0000 1001, E = 0
        CIR             ; 7080 : AC = 0000 0000 0000 0100 (4),       E = 1
        CIR             ; 7080 : AC = 1000 0000 0000 0010 (-32766),  E = 0
        CIL             ; 7040 : AC = 0000 0000 0000 0100 (4),       E = 1
        CIL             ; 7040 : AC = 0000 0000 0000 1001 (9),       E = 0
        HLT
NUM:    .data 1 9
```

## Procedure

### Step 1 – Open the machine

### Step 2 – Open the program
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/e2a50ece-089b-4979-a6a0-951bfc45d8df" />


### Step 3 – Assemble and load (Ctrl+2)
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/66a13210-2b32-46d7-9992-8498b013d186" />


### Step 4 – Enter debug mode (Ctrl+D)
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/7077f284-a23e-4c40-b2f0-bc1bb9969583" />

### Step 5 – Set the Registers pane to Unsigned Dec and the RAM pane to Hex

### Step 6 – Click Step by Instr 6 times and note AC, E, PC, AR and IR after each step


## Observations

Assembled program:

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction |
|---|---|---|---|---|
| 0 | 000 | 2006 | | LDA NUM |
| 1 | 001 | 7080 | | CIR |
| 2 | 002 | 7080 | | CIR |
| 3 | 003 | 7040 | | CIL |
| 4 | 004 | 7040 | | CIL |
| 5 | 005 | 7001 | | HLT |
| 6 | 006 | 0009 | NUM | .data 1 9 |

Register contents (decimal) after each instruction:

| Step | PC before | Instruction | IR (hex) | AC | E | PC | AR | IR (dec) |
|---|---|---|---|---|---|---|---|---|
| 1 | 0 | LDA NUM | 2006 | 9 | 0 | 1 | 6 | 8198 |
| 2 | 1 | CIR | 7080 | 4 | 1 | 2 | 128 | 28800 |
| 3 | 2 | CIR | 7080 | 32770 (-32766) | 0 | 3 | 128 | 28800 |
| 4 | 3 | CIL | 7040 | 4 | 1 | 4 | 64 | 28736 |
| 5 | 4 | CIL | 7040 | 9 | 0 | 5 | 64 | 28736 |
| 6 | 5 | HLT | 7001 | 9 | 0 | 6 | 1 | 28673 |


<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/9e880f5c-d3fd-4faa-aa6b-0bb376fa6851" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/eb8854b6-bd79-4a9b-b194-9ae689d6faa5" />
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/e3fb635e-38cf-4e92-9700-36e269511e21" />
<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/df88e000-38a5-44e4-859c-ab9a8041d51c" />
<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/092a7cb7-be48-4d2b-88d0-b9dcd12298d0" />


Final register contents after HLT:

| Register | Final value (decimal) |
|---|---|
| AC | 9 |
| E | 0 |
| PC | 6 |
| AR | 1 |
| IR | 28673 (7001 hex) |

## Result
CIR and CIL were simulated; two right rotations followed by two left rotations restored AC = 9. 
After execution: **AC = 9, E = 0, PC = 6, AR = 1, IR = 28673**.
