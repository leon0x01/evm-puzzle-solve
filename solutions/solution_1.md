Let copy the bytes code and copy decode the bytes code into represented opcode.

```
    {
  "code": "3456FDFDFDFDFDFD5B00",
  "askForValue": true,
  "askForData": false
}

```

represented code 

```
[00]        CALLVALUE
[01]        JUMP
[02]        REVERT
[03]        REVERT
[04]        REVERT
[05]        REVERT
[06]        REVERT
[07]        REVERT
[08]        JUMPDEST
[09]        STOP

```
LET's look up **CALLVALUE** in [evm.codes](https://www.evm.codes/)

It takes values of the current call in wei and push into top of the stack.

Now, Let's look into **JUMP** opcode. 

JUMP opcode simply jump to the JUMP destination through the byte offset i.e Program Counter. It indicates which instruction will be executed next. 

**JUMP** takes a counter as input that is byte offset where execution will continue from and reached to **JUMPDEST**

Now looking to upabove opcode from offset [0-7] it throws the REVERT it works correctly if it reached to offset [8] so, as it take the value if we send the value ranging from 2-7 wei then it throws error saying:

```
Loading Solidity compiler v0.8.19...
Solidity compiler loaded
[Error] invalid JUMP at 9bebe83771f33ccc8f9372f4baaa9fcc42c1f65c5ff4b3416ca0f19aa8e7e282/9bbfed6889322e016e0a02ee459d306fc19545d8:1
[Error] invalid JUMP at 9bebe83771f33ccc8f9372f4baaa9fcc42c1f65c5ff4b3416ca0f19aa8e7e282/9bbfed6889322e016e0a02ee459d306fc19545d8:1

```

**JUMP takes => JUMP + COUNTER = JUMPDEST**

if we send the 8 wei it will push to the top of the stack and JUMP instruction takes the value of the top of stack and reached to the destination ie JUMPDEST. It jump breaks the linear execution raning 2-7 and reached the 8 offset. of JUMPDEST. solved_puzzle1.


