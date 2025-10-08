---
description: The act of modifying the behavior of an compiled binary.
---

# Patching

Sometimes the crackmes' goal is to patch the given application, this type of crackme is usually referred to as an "_patchme_" and the goal is to re-enable specific menus and remove so called nags, which are pop-ups and other messages made to annoy the user. The easiest way to solve these is within an debugger like x64dbg or OllyDbg, since we can play around with the binaries behavior while its running. Another approach would be to patch the program in an reverse engineering platform such as IDA Pro or Binary Ninja, however, in my opinion it is more annoying to patch programs with an GUI.

TODO: add tutorial of patching a crackme from tuts4you
