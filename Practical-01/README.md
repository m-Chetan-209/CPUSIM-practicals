# Practical 1: Create a Machine (Basic Computer Architecture)

## Aim
To create, in CPU Sim, a machine based on the Basic Computer architecture: its registers, memory, microinstructions, instruction fields and machine instructions.

## Tool
CPU Sim 4.0.11 (Java 8 with JavaFX)

## Machine
Machine file: [BasicComputer_new.cpu](BasicComputer_new.cpu)

## Theory
A CPU Sim machine is described at the register-transfer level by four kinds of objects:

| Object | Meaning | Dialog |
|---|---|---|
| Hardware modules | Registers, condition bits (single bits that can halt the machine or record a carry) and RAM | Modify → Hardware Modules (Ctrl+K) |
| Microinstructions | Elementary register-transfer operations such as `PC->AR`, `M[AR]->DR`, `AC+DR->AC`, a test-and-skip or a decode | Modify → Microinstructions (Ctrl+Shift+M) |
| Fetch sequence | The microinstructions executed at the start of every instruction cycle | Modify → Fetch Sequence (Ctrl+T) |
| Machine instructions | A name, an opcode, a format built from fields and an execute sequence of microinstructions ending with End | Modify → Machine Instructions (Ctrl+M) |

A control unit that runs a stored list of microinstructions for each instruction is a **microprogrammed control unit**, which is exactly what CPU Sim simulates. The Basic Computer is a 16-bit, single-accumulator, stored-program computer with a 4096-word memory, and every instruction is one 16-bit word.

## Procedure

### Step 1 – Start a new machine

### Step 2 – Create the registers
<img width="1920" height="1080" alt="registers" src="https://github.com/user-attachments/assets/47fd6c0f-6ad8-4d9b-a104-9e43606f7de1" />

### Step 3 – Create the condition bits and the RAM
<img width="1920" height="1080" alt="condition-bit" src="https://github.com/user-attachments/assets/4bd329ab-32aa-4dcf-9faf-6b85afe6b27a" />
<img width="1920" height="1080" alt="RAM-creation" src="https://github.com/user-attachments/assets/8fa0b35e-c16e-4a8e-b316-648d9a0f7368" />

### Step 4 – Create the microinstructions
<img width="1920" height="1080" alt="TransferRtoR" src="https://github.com/user-attachments/assets/732fa82a-1eee-4068-9de7-598fc843bbd3" />
<img width="1920" height="1080" alt="setCondbit" src="https://github.com/user-attachments/assets/f7ad53b5-cb23-4bc9-a488-09383d864c62" />
<img width="1920" height="1080" alt="decode" src="https://github.com/user-attachments/assets/eecd29a0-0445-4f28-b9ef-0c991ff7960b" />
<img width="1920" height="1080" alt="test" src="https://github.com/user-attachments/assets/4058daaa-cbd2-49a9-96be-aeb546fc49b0" />
<img width="1920" height="1080" alt="set" src="https://github.com/user-attachments/assets/6ff9a89e-e550-4ff0-938b-8bc354b0ec52" />
<img width="1920" height="1080" alt="shift" src="https://github.com/user-attachments/assets/1461965b-7611-4b87-96ad-4171f7adf367" />
<img width="1920" height="1080" alt="logcal" src="https://github.com/user-attachments/assets/ab7d2d1b-3058-44da-9430-bd667f54b818" />
<img width="1920" height="1080" alt="arithmetic" src="https://github.com/user-attachments/assets/210cce01-d812-45a9-b77b-b04906dd12a1" />
<img width="1920" height="1080" alt="increemnt" src="https://github.com/user-attachments/assets/fa38a653-5d49-4e34-aa7c-09488a3cd086" />
<img width="1920" height="1080" alt="memory-access" src="https://github.com/user-attachments/assets/4a81a8b8-8123-4e51-b7d5-fefd78baae8d" />
<img width="1920" height="1080" alt="io" src="https://github.com/user-attachments/assets/e00c8ed1-804f-44e5-b2fa-e27980809ffb" />


### Step 5 – Create the instruction fields
<img width="1920" height="1080" alt="fields" src="https://github.com/user-attachments/assets/b38d3862-f1de-4a01-9d94-b5f2c93a9979" />


### Step 6 – Create the machine instructions
<img width="1920" height="1080" alt="machine-instruc" src="https://github.com/user-attachments/assets/5807863a-4e13-46a0-831f-d54d6970ac6e" />


### Step 7 – Fetch sequence
<img width="1920" height="1080" alt="fetch-seq" src="https://github.com/user-attachments/assets/b1b907b6-41fd-40ba-acb9-a347d445c27d" />


## Observations
- The Hardware Modules dialog lists the **eight registers** (AC, DR, AR, PC, IR, E, TMP, S), the **two condition bits** (carry-E and halt-S) and the **4096-word RAM** (M, 16-bit cells).
- **33 microinstructions** and **20 machine instructions** are defined.
- The fetch sequence is: `PC->AR`, `M[AR]->IR`, `PC+1->PC`, `IR(0-11)->AR`, `decode-IR`.
- Loading the ADD program (`P03_ADD.a`) with Assemble & load (Ctrl+2) produces the expected machine code `F800 3007 F800 1007 …` in the RAM pane.
<img width="1920" height="1080" alt="Screenshot (2357)" src="https://github.com/user-attachments/assets/568b1146-9c74-42f9-8254-0c0669c3d3c7" />


## Result
A machine based on the Basic Computer architecture was created in CPU Sim and saved as `BasicComputer_new.cpu`
