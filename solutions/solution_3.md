{
  "code": "3656FDFD5B00",
  "askForValue": false,
  "askForData": true
}

lets convert the above bytecode into opcode

```
[01]            CALLDATASIZE
[02]            JUMP
[03]            REVERT
[04]            REVERT
[05]            JUMPDEST
[06]            STOP
```

let hop into the opcode of above

**CALLDATASIZE**: push the byte size of the calldata into the top of the stack 

**JUMP**: JUMP takes the counter value present in the stack it is simply the value presented in the top of the stack and breaks the linear execution and reached to **JUMPDEST**;

lets solve the puzzle 3 

1. **CALLDATASIZE** push the byte size of the calldata.
2. Now, we need to pass the CALLDATASIZE ie represent 4 in offset to break the offset ranging 3-4 which throws **REVERT**
3. **00111100** represent **4** offset and push into the top of the stack
4. JUMP carry the value in the top most of the stack and jump to **JUMPDEST**