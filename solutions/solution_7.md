{
  "code": "36600080373660006000F03B600114601357FD5B00",
  "askForValue": false,
  "askForData": true
}

let's decode above bytecode into the opcode 

```
    [00]            CALLDATASIZE
    [01]            PUSH1                           00
    [03]            DUP1                            
    [04]            CALLDATACOPY
    [05]            CALLDATASIZE
    [06]            PUSH1                           00
    [08]            PUSH1                           00
    [0a]            CREATE
    [0b]            EXTCODESIZE
    [0c]            PUSH1                           01
    [0e]            EQ                              
    [0f]            PUSH1                           13
    [11]            JUMPI
    [12]            REVERT
    [13]            JUMPDEST
    [14]            STOP
```

let hop into the above opcode

**CALLDATASIZE**: It get the size of input data that is pass as the calldata.

**PUSH1**: push 1 byte size into the stack

**DUP1**: duplicate the 1 byte size value present on the top of stack and again push the same value into the stack

**CALLDATACOPY**: It copy the input data into the memory. 
       - destOffset: byte offset in the memory where the result will be copied.
       - offset: byte offset in the calldata to copy.
       - size: byte size to copy

**CREATE**: Create a new account with associated code. look **CREATE** instruction at [evm.codes](https://www.evm.codes/?fork=shanghai)

**EQ**: compare the 2 top value presented in the stack, lets called it a: left side integer and b as right side integer 

if **a==b**: 1 if the left side is equal to the right side, 0 otherwise.

**JUMPI**: it takes the byte offset in the deployed code where execution will continue from. Must be a JUMPDEST instruction and the program counter will be altered with the new value only if this value is different from 0. Otherwise, the program counter is simply incremented and the next instruction will be executed.

**EXTCODESIZE**: it takes 20-bytes address of the contract to query and return the byte size of the code.

Now let solve the puzzle_7:

1. Here to solve the problem in **EXTCODESIZE** it should return with 1 to make the conditionally jump to JUMPDEST.
2. First calldata value is passed with **0x60016000f3** 

```
    60 PUSH1         01
    60 PUSH1         00
    f3 RETURN

    it return 00 1byte data
```
3. Now **CALLDATASIZE** takes size bytes of passed data and push into the stack.
4. **PUSH1** pushed the 0 into the stack.
5. **DUP1** takes value presented into the top of the stack and pushes the same value to the top of the stack.
6. **CALLDATACOPY** takes 0 as destOffset in the memory and offset of the calldata to copy and byte size to copy.
7. it copy **60016000f3000000000000000000000000000000000000000000000000000000** into the memory by padding 0 at the end up to 32 byte
8. **CALLDATASIZE** push the size of the calldata into the stack ie. **5**
9. **PUSH1** pushes **00** into the stack.
10. again execute **PUSH1** and pushes **00** into the stack.
11. **CREATE** deploy the code and generate 20byte address and pushes into the stack.
12. **EXTCODESIZE** pushes the byte size of the code.
13. **EQ** compare top and top-1 value presented into the stack if true then push 1 into the stack otherwise 0.
14. **PUSH1** pushes the 1 byte ie 13 in hex into the stack
15. **JUMPI** execute only if the top-1 value of the stack is represented as 1.
16. Now it reach to **JUMPDEST** if it is true otherwise it revert.