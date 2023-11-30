{
  "code": "34380356FDFD5B00FDFD",
  "askForValue": true,
  "askForData": false
}

let's decode the bytecode into opcode 

```
    [00]        CALLVALUE
    [01]        CODESIZE
    [02]        SUB
    [03]        JUMP
    [04]        REVERT
    [05]        REVERT
    [06]        JUMPDEST
    [07]        STOP
    [08]        REVERT
    [09]        REVERT
```

let's hop into the opcode one by one 

**CALLVALUE** : It simple take the value of the current call in wei and push into the top of the stack

**CODESIZE**: As each instruction occupies the one bytes. CODESIZE simple store the code bytes size and push on to the stack. As in this puzzle it contains 10 instruction in total so that it contain 10 bytes and push hex represent value of 10 ie **a** into the top of the stack.

**SUB**: SUB instruction subtracts the two value from the stack and suppose a is top of the stack and b is top-1 of the stack it pops the value from it and a: first interger value and b is integer value to substract to the first.

**a-b: integer result of the substraction modulo 2^256**

**JUMP**: JUMP takes the counter value present in the stack it is simply the value presented in the top of the stack and breaks the linear execution and reached to **JUMPDEST**;

**REVERT**: STOP the current context execution and also return the unused gas the caller. It also reverts the gas refund to its value before the current context. 

If the execution is stopped with **REVERT**. the value 0 is pushed on the stack of the calling context which continues to execute normally.

Now lets solve the puzzle 2:

1. First we need to pass such value in wei on subtracting it should break the execution of offset [4-5] condition of **REVERT**.
2. caller pass the value 4 in wei CALLVALUE takes it and push on to stack.
3. **CODESIZE** pushes the size of code in bytes and push it into top of the stack as of now it 10 ie a in HEX format.
4. **SUB** pops two value out of the stack as top of stack is 10 and top-1 is 4, ie. **10-4=6** and 6 is push in to the stack.
5. **JUMP** takes counter that is value top of the stack and reach to offset of the that destination **JUMPDEST**
6. here it breaks the execution of 4-5 which throws the **REVERT** on execution