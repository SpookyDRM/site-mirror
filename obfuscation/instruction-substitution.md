---
description: Replacing instructions with more complex equivalents.
---

# Instruction Substitution

Instruction Substitution is a rather simple type of obfuscation where instructions like `add` are transformed into something like:

```nasm
loop:
    inc    ecx
    mov    edx,eax
    inc    ecx
    mov    eax,eax
    inc    ecx
    xor    eax,ecx
    inc    ecx
    or     edx,ecx
    mov    ecx,edx
    sub    ecx,eax
    inc    ecx
    mov    eax,eax
    inc    ecx
    and    eax,ecx
    add    ecx,ecx
    inc    esp
    mov    eax,edx
    inc    esp
    mov    ecx,ecx
    inc    esp
    sub    eax,eax
    test   ecx,ecx
    jne    loop 
```

This type of obfuscation can not only be dealt with optimization passes, but also manually by, for example, first copying the decompiled result into an separate source-file and testing its functionality against various values and comparing the behavior of the code with known behavior of other operations. In this case, the obfuscated code represents an binary adder usually seen in electronics. We can now proceed by patching out all occurrences of that pattern with a simplified `add` instruction.

For example we may write a script like the following:

```python
import re

def find_pattern(target_path:str, pattern:bytes) -> list:
    with open(target_path, "rb") as f:
        data = f.read()
    r = re.compile(re.escape(pattern))
    return list(r.finditer(data))

def add_padding(pattern:bytes, patch:bytes) -> bytes:
    return patch + "\x90" * (len(pattern) - len(patch))

def patch_file(path:str, offsets:list, patch:bytes) -> None:
    with open(path, "rb") as f:
        data = bytearray(f.read())
    
    for off in offsets:
        data[off.start():off.start() + len(patch)] = patch
        
    with open(path, "wb") as f:
        f.write(data)

def patch_and_save(path:str, pattern:bytes, patch:bytes) -> None:
    offsets:list = find_pattern(path, pattern)
    patch_bytes = add_padding(pattern, patch)
    patch_file(path, offsets, patch_bytes)

pattern = ( 
    "\x41\x8B\xD0\x41\x8B\xC0\x41\x33\xC1\x41\x0B\xD1"
    "\x8B\xCA\x2B\xC8\x41\x8B\xC0\x41\x23\xC1\x03\xC9"
    "\x44\x8B\xC2\x44\x8B\xC9\x44\x2B\xC0\x85\xC9\x75"
    "\xDB" 
)
patch_str = "\x45\x01\xc8"
patch_and_save("path/to/file.exe", pattern, patch_str)
```

We could of course improve this script by assembling and disassembling the code however this example provides an quick and easy way to deal with instruction substitution.
