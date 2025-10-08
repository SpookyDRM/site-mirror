---
description: Replacing known instruction patterns with a jump to the protectors code.
---

# Stolen Bytes

placeholder

what are they? how do they work? how do we find 'em? how do we deal with 'em?

protectors only steal instructions of which they know the opcode size of, otherwise stealing half of an instruction would cause an exception.

```nasm
; Before -------------
push    rbp
mov     rbp, rsp
sub     rsp, 0x08
; Some Code ...
xor     rax, rax

; After --------------
jmp     stolen_bytes
; Some Code ...
xor     rax, rax

```
