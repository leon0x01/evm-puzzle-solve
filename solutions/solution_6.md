{
  "code": "60003556FDFDFDFDFDFD5B00",
  "askForValue": false,
  "askForData": true
}

let's decode above bytecode into opcode 

```
    [00]            PUSH1                       00
    [02]            CALLDATALOAD
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
**PUSH1**: Place 1 byte into on stack by incrementing all the other value indices. 
**CALLDATALOAD**: It takes thebyte offset in the calldata and return 32-byte value starting from the given offset of the calldata. after the calldata if calldata isn't of size 32 bytes then it append 0 at the end.

Now let's hope into the puzzl 6:
1. **PUSH1** takes 1 bytes data ie **00** into the top of the stack.
2. **CALLDATALOAD** always takes the offset data and derive it into 32 bytes and push it into the stack.
3. Need to pass such data in 32 bytes format where it breaks the linear execution ranging from 4-9.
4. Pass the **0x000000000000000000000000000000000000000000000000000000000000000a** ie 10 in decimal, calldataload takes the data and push into the stack.
5. JUMP takes the PC from the top most of the stack and reach to JUMPDEST
 