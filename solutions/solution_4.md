{
  "code": "34381856FDFDFDFDFDFD5B00",
  "askForValue": true,
  "askForData": false
}


let's decode above bytecode into opcode

```
    [00]            CALLVALUE
    [01]            CODESIZE
    [02]            XOR
    [03]            JUMP
    [04]            REVERT
    [05]            REVERT
    [06]            REVERT
    [07]            REVERT
    [08]            REVERT
    [09]            REVERT
    [0a]            JUMPDEST
    [0b]            STOP
```

let's hop into the opcode

**CALLVALUE**: takes the value of the current call in wei and push on to the stack.
**CODESIZE**: takes the code size value in bytes and store into the stack, each instructions consists of 1 byte so in above opcode there's 12 instructions so it store 12 bytes in hex format ie. **c**

**XOR**: It pops the top most 2 value of stack and use the bitwise XOR operation and output is again pushed into the stack 

**JUMP**: It takes the counter presented in the stack and jump to that offset.

lets solve the puzzle 4

1. At first we need to pass such a value that jump to the JUMPDEST position offset 10 ie **0a**
2. Here **CODESIZE** contains the 12 instructions so it push value **c** in the stack.
3. **XOR** performs bitwise operation popping 2 value 12 and other value that is passed from CALLVALUE
4. To complete this operation we need to pass value 6 to get the JUMPDEST position ie 10/0a
```
        12     1100   // in XOR 1 ^ 1 = 0 | 0 ^ 0 = 0 | 1 ^ 0 = 1  | 0 ^ 1 = 1
        06     0110
  ------------------------ 
        10     1010
```
5. so, **JUMP** takes value 10 presented in stack and reached to **JUMPDEST**