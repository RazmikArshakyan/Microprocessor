# Microprocessor Project

## Introduction

The objective of this mini-project is to develop a simple and almost complete microprocessor with a focus on a small number of registers and a limited instruction set. Memory management will not be carried out initially. The project begins with creating simple multiplexers, adders, and decoders, progressing towards building the complete microprocessor circuit.

## ALU (Arithmetic Logic Unit)

### Operation Codes and Results

| Opcode | Result     |
|--------|------------|
| 00     | A + B      |
| 01     | A - B      |
| 10     | A and B    |
| 11     | B          |

## RegBank

RegBank is responsible for storing all registers and facilitating operations. It consists of 8 registers of 8 bits each. The outputs OUT0 and OUT1 correspond to the values stored in the index registers N0 and N1. Writing to this bank is possible when the input signal WR is set to 1.

## CORE0

CORE0 performs the basic actions of an execution core:

- Reads operands from the input ports N0 and N1 in the register bank or calculates the second operand from N1.
- Calculates the result of the operation identified by the input port OP using the ALU and operands.
- Records the result in the register bank for register N0 when the signal WR is set to 1 on the next rising edge of the clock CLK.

**Debugging Outputs:**
- A: Value (8-bit) of the first operand supplied to the ALU.
- B: Value (8-bit) of the second operand supplied to the ALU.
- R: Value (8-bit) of the result of the ALU.

### Operation Example

To illustrate the functionality of CORE0, consider the following instructions:

| Instruction | OP  | N0 | N1 | A         | B         | R         |
|-------------|-----|----|----|-----------|-----------|-----------|
| MOV         | 11  | 000| 100| 00000000  | 00000100  | 00000100  |
| MOV         | 11  | 010| 011| 00000000  | 00000011  | 00000011  |
| ADD         | 00  | 000| 010| 00000100  | 00000011  | 00000111  |
| MOV         | 11  | 111| 111| 00000000  | 00000111  | 00000111  |
| SUB         | 01  | 000| 111| 00000111  | 00000111  | 00000000  |

## Testing Instructions

To test the circuit:

1. Set INIT to 1.
2. Set INIT to 0.
3. Position OP, N0, and N1.
4. Check the values on A, B, R.
5. Set CLK to 1 (to save the result).
6. Set CLK to 0.

## Sequencer

The sequencer allows scrolling through the two phases of execution:

- Phase 1: Loading the instruction (IWR = 1).
- Phase 2: Execution of the instruction (BWR = 1).

With each clock pulse, it transitions between phases using a serial/shift register.

## PC (Program Counter)

The PC (Program Counter) provides the address of the instruction to be loaded and executed. It increments with each instruction reading, acting as a counter with 4 D flip-flops.

## Core 1

Core 1 combines CORE0 with a sequencer, PC, a parallel register IW (Word Instruction), and memory to create a complete microprocessor. The sequencer controls phases of instruction loading and execution.

### Operation Cycle

1. In state 1, IWR is set to 1 to read an instruction.
2. In state 2, BWR is set to 1 to perform the calculation.
3. The cycle repeats as the sequencer transitions between states.
