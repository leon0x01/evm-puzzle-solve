{
  "code": "36600080373660006000F0600080808080945AF1600014601B57FD5B00",
  "askForValue": false,
  "askForData": true
}

let's convert above bytecode into opcode 

```
    [00]             CALLDATASIZE           
    [01]             PUSH1                              00
    [02]             DUP1
    [04]             CALLDATACOPY
    [05]             CALLDATASIZE
    [06]             PUSH1                              00
    [08]             PUSH1                              00
    [0a]             CREATE
    [0b]             PUSH1                              00
    [0d]             DUP1
    [0e]             DUP1
    [0f]             DUP1
    [10]             DUP1
    [11]             SWAP5
    [12]             GAS
    [13]             CALL
    [14]             PUSH1                              00
    [16]             EQ
    [17]             PUSH1                              18
    [19]             JUMPI
    [1a]             REVERT
    [1b]             JUMPDEST
    [1c]             STOP
```
let's hop into above opcode

**CALLDATASIZE** get the size of the calldata and push into the stack 
**PUSH1** place 1 byte item on stack 
**DUP1** Duplicate 1st stack item 
**CALLDATACOPY** copy the input data and takes the destOffset, offset and size of the stack and copy into the memory.
**CREATE** create the new account with associated code and the destination address is calculated as the rightmost 20 bytes
**SWAP5** Exchange 1st and 6th stack items
**GAS** Get the amount of available gas, including the correspoding reduction for the cost of this instruction.
**CALL** creates a new sub context and execute the code of the given account then resumes the current one.

Let's solve the evm puzzle 8

1. **CALL** return 0 if the code is reverted. Otherwise 0 
2. Here if we pass any data it will create the code and generate 20 byte address of the given code 
3. CALL opcde trigger successfully return 1 into the stack.
4. In **CALLDATACOPY** side the data is actually copy into the memory so while passing the valuefordata it must return revert opcode so that while call it also revert.
5. The main objective is JUMPI instruction as 1 so that it reaches to JUMPDEST.
6. In order to solve it, we must create the another contract which always revert 
7. To achieve that
  - saving something into memory 
  - return revert

```
  0x60fd60005360016000f3 

  [00]      PUSH1              fd
  [02]      PUSH1              00
  [04]      MSTORE3
  [05]      PUSH1              01
  [07]      PUSH1              00
  [09]      RETURN  
```

9. By trying out passing above value as data while CALL opcode trigger and call to the memory it revert and passed 0 into the stack EQ works correctly and JUMPI execute.
