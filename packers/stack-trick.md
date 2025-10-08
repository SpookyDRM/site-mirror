---
description: >-
  The stack trick is an basic binary unpacking technique which involves setting
  a breakpoint on the stack after an push instruction.
---

# Stack Trick

If we open the packed executable in our debugger of choice and step over the `PUSHAD` instruction, and then place an "on access" breakpoint and then hit run we will land near our unconditional jump to the original entry point (OEP). After following that jump, we may freely dump the unpacked binary with Scylla and fix its import address table (IAT).

TODO: Add images and example (manually unpacking UPX)

Some packers that are vulnerable to this trick are:

* UPX
* MPRESS
