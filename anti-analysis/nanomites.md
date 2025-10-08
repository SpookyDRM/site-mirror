---
description: >-
  Replacing jumps with exception-raising instructions so that the self-debugging
  process can resolve the jumps manually via a table.
---

# Nanomites

placeholder

what are they? how do they work? how do we deal with them?

Dynamic obfuscation, patch some jumps with an exception like a single step interrupt or a `ud2` instruction, protector then self-debugs or places an event handler and checks if the exception came from the patched place, if that is the case, check if the jump should be taken and redirect the control flow

TODO: rewrite the above and add an code example
