# Iguana MAC Interpreter

An interpreter for Andrew S. Tanenbaum's MAC assembly.

## Example Assembly Code

The following example finds the largest of two values:

```arm
# Find the biggest between two values

.data
VAL1: .word 10
VAL2: .word 257
BIGGER: .word 0

.text
    LODD VAL1
    SUBD VAL2
    JPOS LABEL1

    LODD VAL2 
    STOD BIGGER
    JUMP PRINT_AND_END

LABEL1:
    LODD VAL1  
    STOD BIGGER   

PRINT_AND_END:
    LODD BIGGER
    PRINTLNAC
    HALT
```

output:
```
257
```

# Build Instructions

1. **Ensure that you have Rust and Cargo installed**:
   - Follow the installation guide on the official [Rust website](https://www.rust-lang.org/learn/get-started) if needed.

2. **Clone the repository**:
   - Use the following command to clone the repository:
     ```bash
     git clone https://github.com/joeCavZero/iguana-mac-interpreter.git
     ```

3. **Build the source code**:
   - Run the following command to build the project in release mode:
     ```bash
     cargo build --release
     ```

4. **Locate the generated binary**:
   - After the build is complete, the binary will be located at:
     ```bash
     ./target/release/iguana
     ```


---

# Iguana's MAC Operations Guide

## Constant Loading

- **LOCO X**  
  **Behavior**: Loads the constant value `X` into the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = X`

## Memory Operations

- **LODD X**  
  **Behavior**: Loads the value from the stack at address `X` into the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = M[X]`

- **STOD X**  
  **Behavior**: Stores the value in the accumulator (`ac`) into the stack at address `X`.  
  **Pseudo-behavior**: `M[X] = ac`

- **LODL X**  
  **Behavior**: Loads a value from the stack relative to the stack pointer (`sp`) into the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = M[sp - X]`

- **STOL X**  
  **Behavior**: Stores the value in the accumulator (`ac`) into the stack relative to the stack pointer (`sp`).  
  **Pseudo-behavior**: `M[sp - X] = ac`

- **PSHI**  
  **Behavior**: Pushes the value in the accumulator (`ac`) onto the stack.  
  **Pseudo-behavior**: `sp = sp - 1; M[sp] = ac`

- **POPI**  
  **Behavior**: Pops a value from the stack into the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = M[sp]; sp = sp + 1`

- **PUSH**  
  **Behavior**: Pushes the value in the accumulator (`ac`) onto the stack.  
  **Pseudo-behavior**: `sp = sp - 1; M[sp] = ac`

- **POP**  
  **Behavior**: Pops a value from the stack into the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = M[sp]; sp = sp + 1`

- **SWAP**  
  **Behavior**: Swaps the value in the accumulator (`ac`) with the value at the top of the stack.  
  **Pseudo-behavior**: `temp = ac; ac = M[sp]; M[sp] = temp`

- **INSP X**  
  **Behavior**: Increments the stack pointer (`sp`) by `X`.  
  **Pseudo-behavior**: `sp = sp + X`

- **DESP X**  
  **Behavior**: Decrements the stack pointer (`sp`) by `X`.  
  **Pseudo-behavior**: `sp = sp - X`

---

## Arithmetic Operations

- **ADDD X**  
  **Behavior**: Adds the value at address `X` in the stack to the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = ac + M[X]`

- **SUBD X**  
  **Behavior**: Subtracts the value at address `X` in the stack from the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = ac - M[X]`

- **ADDL X**  
  **Behavior**: Adds a value from the stack relative to the stack pointer (`sp`) to the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = ac + M[sp - X]`

- **SUBL X**  
  **Behavior**: Subtracts a value from the stack relative to the stack pointer (`sp`) from the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = ac - M[sp - X]`

- **MULD X**  
  **Behavior**: Multiplies the value in the accumulator (`ac`) by the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac * M[X]`

- **DIVD X**  
  **Behavior**: Divides the value in the accumulator (`ac`) by the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac / M[X]`

---

## Control Flow Operations

- **JPOS X**  
  **Behavior**: Jumps to address `X` if the accumulator (`ac`) is positive.  
  **Pseudo-behavior**: `if ac > 0: pc = X`

- **JZER X**  
  **Behavior**: Jumps to address `X` if the accumulator (`ac`) is zero.  
  **Pseudo-behavior**: `if ac == 0: pc = X`

- **JUMP X**  
  **Behavior**: Unconditionally jumps to address `X`.  
  **Pseudo-behavior**: `pc = X`

- **JNEG X**  
  **Behavior**: Jumps to address `X` if the accumulator (`ac`) is negative.  
  **Pseudo-behavior**: `if ac < 0: pc = X`

- **JNZE X**  
  **Behavior**: Jumps to address `X` if the accumulator (`ac`) is not zero.  
  **Pseudo-behavior**: `if ac != 0: pc = X`

- **CALL X**  
  **Behavior**: Calls a subroutine at address `X`, saving the return address on the stack.  
  **Pseudo-behavior**: `sp = sp - 1; M[sp] = pc + 1; pc = X`

- **RETN**  
  **Behavior**: Returns from a subroutine by popping the return address from the stack.  
  **Pseudo-behavior**: `pc = M[sp]; sp = sp + 1`

---

## Bitwise Operations

- **ANDI X**  
  **Behavior**: Performs a bitwise AND between the accumulator (`ac`) and the constant `X`.  
  **Pseudo-behavior**: `ac = ac & X`

- **ORI X**  
  **Behavior**: Performs a bitwise OR between the accumulator (`ac`) and the constant `X`.  
  **Pseudo-behavior**: `ac = ac | X`

- **XORI X**  
  **Behavior**: Performs a bitwise XOR between the accumulator (`ac`) and the constant `X`.  
  **Pseudo-behavior**: `ac = ac ^ X`

- **NOTI X**  
  **Behavior**: Performs a bitwise NOT on the constant `X` and stores the result in the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = ~X`

- **SHFLI X**  
  **Behavior**: Shifts the value in the accumulator (`ac`) left by `X` bits.  
  **Pseudo-behavior**: `ac = ac << X`

- **SHFRI X**  
  **Behavior**: Shifts the value in the accumulator (`ac`) right by `X` bits.  
  **Pseudo-behavior**: `ac = ac >> X`

- **ANDD X**  
  **Behavior**: Performs a bitwise AND between the accumulator (`ac`) and the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac & M[X]`

- **ORD X**  
  **Behavior**: Performs a bitwise OR between the accumulator (`ac`) and the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac | M[X]`

- **XORD X**  
  **Behavior**: Performs a bitwise XOR between the accumulator (`ac`) and the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac ^ M[X]`

- **NOTD X**  
  **Behavior**: Performs a bitwise NOT on the value at address `X` in the stack and stores the result in the accumulator (`ac`).  
  **Pseudo-behavior**: `ac = ~M[X]`

- **SHFRD X**  
  **Behavior**: Shifts the value in the accumulator (`ac`) right by the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac >> M[X]`

- **SHFLD X**  
  **Behavior**: Shifts the value in the accumulator (`ac`) left by the value at address `X` in the stack.  
  **Pseudo-behavior**: `ac = ac << M[X]`

---

## Debug Operations (Prints)

- **PRINTLNAC**  
  **Behavior**: Prints the value in the accumulator (`ac`) as a new line.  
  **Pseudo-behavior**: `print( ac + '\n' )`

- **PRINTAC**  
  **Behavior**: Prints the value in the accumulator (`ac`).  
  **Pseudo-behavior**: `print( ac )`

- **PRINTLNSPI X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`) as a new line.  
  **Pseudo-behavior**: `print( M[sp + X] + '\n' )`

- **PRINTLNSPD X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`) as a new line.  
  **Pseudo-behavior**: `print( M[sp - X] + '\n' )`

- **PRINTSPI X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`).  
  **Pseudo-behavior**: `print( M[sp + X] )`

- **PRINTSPD X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`).  
  **Pseudo-behavior**: `print( M[sp - X] )`

- **PRINTLNACCHAR**  
  **Behavior**: Prints the value in the accumulator (`ac`) as a character (new line).  
  **Pseudo-behavior**: `print( char(ac) + '\n' )`

- **PRINTACCHAR**  
  **Behavior**: Prints the value in the accumulator (`ac`) as a character.  
  **Pseudo-behavior**: `print( char(ac) )`

- **PRINTLNSPCHARI X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`) as a character (new line).  
  **Pseudo-behavior**: `print( char(M[sp + X]) + '\n' )`

- **PRINTLNSPCHARD X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`) as a character (new line).  
  **Pseudo-behavior**: `print( char(M[sp - X]) + '\n')`

- **PRINTSPCHARI X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`) as a character.  
  **Pseudo-behavior**: `print( char(M[sp + X]) )`

- **PRINTSPCHARD X**  
  **Behavior**: Prints the value at offset `X` from the stack pointer (`sp`) as a character.  
  **Pseudo-behavior**: `print( char(M[sp - X]) )`

---

## Custom Operations

- **HALT**  
  **Behavior**: Stops the execution of the program.  
  **Pseudo-behavior**: `halt`

- **SLEEPD X**  
  **Behavior**: Pauses execution for the number of seconds specified by the value at address `X` in the stack.  
  **Pseudo-behavior**: `sleep(M[X])`

- **SLEEPI X**  
  **Behavior**: Pauses execution for `X` seconds.  
  **Pseudo-behavior**: `sleep(X)`

