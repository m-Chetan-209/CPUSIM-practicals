# Practical 2: Create the Fetch Routine of the Instruction Cycle

## Aim
To create the fetch (and decode) routine of the instruction cycle and observe it one microinstruction at a time.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
`BasicComputer_new.cpu` – Mano's Basic Computer

## Theory
Every instruction cycle begins with the same fetch and decode phase. In Mano's Basic Computer it takes three clock pulses, T0, T1 and T2:

- T0: AR ← PC
- T1: IR ← M[AR], PC ← PC + 1
- T2: decode IR(12–14), AR ← IR(0–11), I ← IR(15)

In CPU Sim the fetch routine is a list of microinstructions that runs before every execute sequence:

| Order | CPU Sim microinstruction | Mano RTL | Time step |
|---|---|---|---|
| 1 | `PC->AR` | AR ← PC | T0 |
| 2 | `M[AR]->IR` | IR ← M[AR] | T1 |
| 3 | `PC+1->PC` | PC ← PC + 1 | T1 |
| 4 | `IR(0-11)->AR` | AR ← IR(0–11) | T2 |
| 5 | `decode-IR` | decode IR(12–14) | T2 |

A real processor performs IR ← M[AR] and PC ← PC + 1 on the same clock edge. CPU Sim executes microinstructions one after another, so they are listed separately.

## Procedure

### Step 1 – Open the machine
<img width="1920" height="1080" alt="open-text" src="https://github.com/user-attachments/assets/bed85043-857c-4161-adc5-7e1896528d5b" />

### Step 2 – Open the Fetch Sequence dialog
<img width="1920" height="1080" alt="fetch-dialog" src="https://github.com/user-attachments/assets/711c329a-45f3-433a-be28-ede16bbeacde" />


### Step 3 – Load the test program
<img width="1920" height="1080" alt="open-text" src="https://github.com/user-attachments/assets/9ceab709-4a5a-402c-a9e0-bc6e0d6dbab8" />


### Step 4 – Enter debug mode
<img width="1920" height="1080" alt="debug-mode-first-stepby" src="https://github.com/user-attachments/assets/9a562d5c-5180-4b15-aeb8-10e33a958c78" />

### Step 5 – Step through the fetch routine with Step by Micro


## Observations

| Micro-step | Microinstruction | AR | PC | IR |
|---|---|---|---|---|
| start | – | 0 | 0 | 0 |
| 1 | `PC->AR` | 0 | 0 | 0 |
| 2 | `M[AR]->IR` | 0 | 0 | 63488 (F800) |
| 3 | `PC+1->PC` | 0 | 1 | 63488 |
| 4 | `IR(0-11)->AR` | 2048 (800) | 1 | 63488 |
| 5 | `decode-IR` | 2048 | 1 | 63488 → INP |

### 1
<img width="1920" height="1080" alt="second-stepby" src="https://github.com/user-attachments/assets/248709aa-b2b9-49a6-b236-ce3c36087c36" />
### 2
<img width="1920" height="1080" alt="third" src="https://github.com/user-attachments/assets/19bc19ea-82d4-4f3f-a38b-63f20be6bcb3" />
### 3
<img width="1920" height="1080" alt="fourth" src="https://github.com/user-attachments/assets/e8a3b500-7ca8-4433-919a-c47ee817e655" />
### 4
<img width="1920" height="1080" alt="fifth" src="https://github.com/user-attachments/assets/665984d2-407b-4ace-88f9-138cc4f799a3" />
### 5
<img width="1920" height="1080" alt="sixth" src="https://github.com/user-attachments/assets/f0be4051-3ce1-450f-b109-484ca7f0fd66" />


## Result
The fetch routine `PC->AR`, `M[AR]->IR`, `PC+1->PC`, `IR(0-11)->AR`, `decode-IR` was created and verified by single-stepping the first instruction of a program.
