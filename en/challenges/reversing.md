# Reverse Engineering Challenges

Reverse engineering (rev) challenges hand you a compiled program — often a small "crackme" — and ask you to recover a flag, a password, or the logic it hides. It matters because reading someone else's binary is a core skill: it teaches you how programs really behave when the source code is gone. Everything here applies to binaries given to you by an event, a platform, or your own lab.

## The CTF reversing workflow

```
file chall                     # 1. identify: ELF? PE? bytecode? archive in disguise?
strings chall | grep -i flag   # 2. cheap wins: embedded literals
./chall                        # 3. run it: what does it ask for?
ls                             # 4. check for given source, hints, or a flag file
ghidraRun                      # 5. import, auto-analyze, open the decompiler
                               # 6. find main, read the check, understand the compare
                               # 7. extract or derive the flag, submit it
```

Do the cheap steps first. Many beginner rev challenges are solved by `strings` alone, and running the binary tells you what the program even wants.

### The first 10 seconds

| Command | What it tells you |
|---|---|
| `file chall` | Format and architecture (ELF x86-64, PE, Java class, a ZIP with the wrong extension) |
| `strings chall \| grep -i flag` | Literal strings mentioning flag, key, password, correct, wrong |
| `./chall` | What it asks for and how it reacts to input (do this in a VM or sandbox you control) |
| `ls -la` + the challenge description | Whether source code, a `flag.txt`, or hints ship with the challenge |

### Strings first

Flags often leak as plain literals: the author prints `"Correct! flag{...}"` on success, and the compiler keeps the text in the binary. Grep broadly before touching a disassembler:

```bash
strings -n 6 chall | grep -i -E 'flag|key|pass|secret|correct'
strings -n 6 chall | grep -E 'flag\{|CTF\{'
```

If the flag is not sitting there, strings still reveals function names, error messages, and format strings that tell you where to look in the decompiler.

### Ghidra basics for CTF

Ghidra is the free option most write-ups assume (download it from the official Ghidra site; releases change, so verify the current version and its import steps). The CTF loop inside it:

1. **Import** the challenge binary (File > Import File) and accept the defaults.
2. **Run auto-analysis** when prompted (yes) — it builds function names and the cross-reference graph.
3. **Find `main`** — open the Symbol Tree, or use Search > Function by Name. If the binary is a wrapper, follow what it calls.
4. **Read the decompiled view** (Window > Decompiler) next to the listing — C-like pseudocode is where the check lives.
5. **Follow the check**: `strcmp`, `memcmp`, or a byte-comparison loop. The compared values are usually your password or the flag.

Alternatives: IDA Free, Cutter, or `objdump -d` for quick looks. Pick one and get fluent.

### Common challenge shapes

| Shape | What the check does | What you do |
|---|---|---|
| Password check | `strcmp(input, expected)` or a byte loop | Read the expected value straight from the decompiled code |
| Crackme | Derives a key from input and constants | Reproduce the derivation in a short script, feed it the flag |
| Bytecode / Python marshal | Logic lives in compiled bytecode, not machine code | Disassemble it (e.g. Python's built-in `dis`; third-party tools like `xdis` or `pycdc` — verify what fits your Python version) |
| Packed binary | An unpacker stub hides the real code | Unpacking intro: conceptually, the stub decrypts the real code as it runs, so you let it run in your lab and inspect the unpacked image — deep unpacking is a later skill |

### Reading a comparison

A typical decompiled password check looks almost like source:

```c
// decompiled main()
if (strcmp(input, "s3cr3t") == 0) {
    puts("Correct! flag{...}");
} else {
    puts("Wrong password");
}
```

A loop version compares byte by byte — the right-hand side of each `!=` is the answer, one character at a time:

```c
for (int i = 0; i < 16; i++)
    if (input[i] != expected[i]) return fail();
```

### Anti-debug in CTFs

Some challenges try to detect that you are analyzing them: a self-`ptrace` check on Linux, `IsDebuggerPresent` on Windows, timing checks using the CPU timestamp counter, or environment checks. In real software these are defensive; in a CTF they are just another puzzle to notice. If a check blocks your local copy of a provided binary, understanding it — and, on your own copy, skipping it to keep studying — is normal analysis practice. Patching exists, but the learning goal is understanding the code, not building an evasion toolkit.

### x86 essentials

| Piece | Meaning |
|---|---|
| `rax` / `eax` / `al` | Primary register; often holds return values |
| `rdi`, `rsi`, `rdx`, `rcx` | First four integer arguments (System V AMD64 ABI) |
| `rsp` / `rbp` | Stack pointer / frame pointer |
| `rip` | Instruction pointer (where execution continues) |
| `mov rax, [rbp-0x10]` | Load a local variable into a register |
| `cmp byte [rax], 0x41` | Compare a byte against `'A'` |
| `je` / `jne` | Jump if equal / not equal — the branch right after a check |

You do not need to write assembly to solve beginner rev challenges; you need to recognize these few patterns and follow the comparison.

### Practice pointers

Good starting grounds: crackmes.one (community crackmes, verify what is currently available), picoCTF's reverse-engineering challenges, and the OverTheWire wargames (Behemoth is their binary-leaning series — check the current wargame list before you start). Always read the challenge description first; it is part of the puzzle.

Only reverse binaries that were given to you — a competition task, a platform challenge, or a program in your own lab. Practicing on someone else's software without permission is not reverse engineering, it is unauthorized access.

## See also

- [CTF Categories](../fundamentals/ctf-categories.md)
- [Binary Exploitation Challenges](pwn.md)
- [Practice Platforms](../practice/platforms.md)
- [The CTF Toolkit](../practice/toolkit.md)
