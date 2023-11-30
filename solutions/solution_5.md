{
  "code": "34800261010014600C57FDFD5B00FDFD",
  "askForValue": true,
  "askForData": false
}

lets convert above bytecode in opcode 

```
[00]        CALLVALUE
[01]        DUP1
[02]        MUL
[03]        PUSH2               0100
[06]        EQ
[07]        PUSH1               0C
[09]        JUMPI
[0a]        REVERT
[0b]        REVERT
[0c]        JUMPDEST
[0d]        STOP
[0e]        REVERT
[0f]        REVERT

```
**CALLVALUE** takes the value in wei and push into the stack

**DUP1**: takes the top value of the stack and duplicate it and again push into the stack

**MUL**: pops the top most 2 value from the stack ie top and top-1 and Multiply it and output is again pushed into the stack.

**PUSH2**: Push 2 byte item on stack. The new value is put on top of the stack, incrementing the other value indices.

**EQ**: compare the 2 top value presented in the stack, lets called it a: left side integer and b as right side integer 
if **a==b**: 1 if the left side is equal to the right side, 0 otherwise

**PUSH1**: place 1 byte item on stack

**JUMPI**: it takes the byte offset in the deployed code where execution will continue from. Must be a JUMPDEST instruction and the program counter will be altered with the new value only if this value is different from 0. Otherwise, the program counter is simply incremented and the next instruction will be executed.

let hope into the solution of puzzle 5 

1. Here to get into the JUMPDEST we must pass such value where it pass the **EQ** instruction that leads to push value 1 on to the stack.
2. here from 10 to 15 it represent into hex with a,b,c,d,e after that it start with 10 again that means 16 denote as 10 in hex.
3. On **EQ** if the top most two value is different then it push 0 on to the stack. Otherwise, it push 1.
4. Lets pass 16 in wei as a callvalue. Callvalue push it into the stack
5. DUP1 duplicate the top most value of stack and again push it into the stack
    ``` 
        | 10 | 16 in decimal => 10 in hex
        | 10 |
    ```
7. MUL instruction pops two value out of the stack 10 * 10 = 100 and push the 100 again into the top of the stack.
8. PUSH2 instruction simply push 2 byte value on the top of the stack.
9. As both top and top -1 value on the stack are same and From **EQ** instruction it pushes 1 into the stack.
10. Again PUSH1 instruction push the JUMPDEST offset ie **c** again into the stack.
    ```
    | c |                       | c |
    | 1 | works correctly       | 0 |  failed
    ```
11. JUMPI instruction works correctly if the top-1 value is **1** other wise it just increment the Program counter.