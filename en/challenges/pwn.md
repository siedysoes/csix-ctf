# Binary Exploitation Challenges

Pwn challenges give you a program — often with a way to run it repeatedly — and ask you to make it do something it was not built to do, such as call a hidden function or read a flag file it refuses to touch. It matters because binary exploitation is where you learn how memory, calling conventions, and protections actually interact. Everything below happens inside the sandbox or service the event provides you.

## Why buffers overflow

Many C functions copy input into a fixed-size buffer with no length check (`gets`, `scanf("%s")`, `strcpy`). On the stack, local buffers sit below the bookkeeping data the function needs to return:

```
higher addresses
+----------------------+
| return address       |  <- where execution resumes after the function returns
+----------------------+
| saved RBP            |  <- caller's frame pointer
+----------------------+
| other locals         |
| buf[64]              |  <- your input lands here and keeps growing upward
+----------------------+
lower addresses
```

Input longer than `buf` walks past the end of the array, overwrites the saved frame pointer, and then the return address. Nothing stops it until the copy ends.

## The classic stack smash, conceptually

Walk through what an overwrite means, step by step:

1. The program reads more bytes than the buffer holds.
2. The extra bytes cover `saved RBP`, then the **return address**.
3. When the function executes `ret`, the CPU pops that overwritten value into the instruction pointer.
4. Execution therefore continues wherever the attacker chose — in a CTF, typically somewhere the binary already contains, like a `win()` function that prints the flag.

The CTF skill is measuring the exact distance to the return address (tools like a cyclic pattern make that safe and mechanical) and deciding where execution should go instead. This is a concept to understand on the challenge binary you were given — the goal is to reach the code path the author intended you to reach.

## Modern protections

| Protection | What it blocks | CTF-level concept (learning framing) |
|---|---|---|
| NX / DEP | Running shellcode planted on the stack | ret2win: reuse code the binary already has instead of executing your own |
| Stack canary | Silent return-address overwrites — a random value is checked when the function returns | The canary must be known or leaked first; many beginner challenges ship without one (verify with checksec) |
| ASLR | Hard-coded addresses of the stack and libraries | Leak an address, or brute-force only where the event allows unlimited local retries |
| PIE | Hard-coded addresses of the binary itself | Same idea: leak a base address, or use relative references; some challenges disable it |

`checksec` tells you which of these are on before you plan anything.

## The CTF pwn workflow

```
checksec --file=chall          # or: pwn checksec chall — which protections are on?
# open the binary in a decompiler (see the reversing guide)
# find the vulnerable function: gets, strcpy, sprintf with no bound, off-by-one reads
# plan: what SHOULD the program do? (e.g. reach a win() that exists in the binary)
# script the payload with pwntools, then test it against the provided instance
```

Plan before you type. Knowing the goal (which function to reach, which protection is missing) turns the payload into an engineering task instead of guesswork.

## A pwntools skeleton

```python
from pwn import *
# connect to the challenge instance provided by the event
p = remote("challenge.example", 1337)
# ... build payload for the given challenge ...
p.interactive()
```

Two safe habits worth building now:

- `cyclic(100)` generates a recognizable pattern; `cyclic_find()` turns a crashed value back into an offset. That is how you measure distance to the return address without guessing.
- Keep the script local and reproducible: one file per challenge, comments on what each field is for.

This is a template, not a working exploit — every challenge wires it differently.

## gdb basics

| Command | Does |
|---|---|
| `gdb ./chall` | Start the binary under the debugger |
| `break main` | Stop when `main` begins |
| `run < input.txt` | Run with prepared input |
| `info registers` | Show the current registers |
| `disassemble main` | Show the machine code of `main` |
| `x/32gx $rsp` | Dump 32 stack slots — watch the overwrite happen |
| `quit` | Exit |

With the remote-debugging extension in gdb (`gef`, `pwndbg`, or `gdb-peda` — verify which one you installed) the stack view becomes much more readable.

## Where to practice

picoCTF's pwn category is the usual first stop, OverTheWire's Narnia wargame is the classic stack-practice series (Bandit covers the Unix basics first — verify current wargame availability), and Hack The Box hosts pwn-style machines and challenges. Start where the challenge gives you the binary and a local copy to debug.

Only exploit instances you were handed: the remote endpoint in the task description, the Docker container the event ships, or a binary running in your own lab. Attacking any other host is out of scope for this skill — and out of bounds legally.

## See also

- [Reverse Engineering Challenges](reversing.md)
- [CTF Categories](../fundamentals/ctf-categories.md)
- [Practice Platforms](../practice/platforms.md)
- [The CTF Toolkit](../practice/toolkit.md)
