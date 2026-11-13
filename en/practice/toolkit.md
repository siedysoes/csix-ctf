# The CTF Toolkit

> Part of the **CSIX CTF** practice & resources section — [EN/ID bilingual learning base](../../README.md).
> Companion to [Practice Platforms](platforms.md) and [Writing Write-ups](writeups.md).
> **Educational framing: every tool below is for the challenges you're given — your own lab or a platform's provided instances only.**

You don't need 100 tools. You need the right *small* set, installed before the first challenge, and a mental map of which tool answers which question.

> ⚠️ **Verify before installing:** install methods, package names, and tool availability change frequently (especially on Linux distros and via `pip`/`gem`). Check the official site or your package manager for current instructions — treat every "how to get" cell as a pointer, not a guarantee.

## The toolkit by category

### General — every category's foundation

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **Python 3 + pip** | The scripting language of CTF: parsing, one-off automation, crypto math | Preinstalled on most Linux/macOS; `apt install python3 python3-pip`, `brew install python3`, or python.org on Windows (ideally via WSL2) |
| **CyberChef** | Drag-and-drop encoding/decoding: base64, hex, XOR, hashes, compression, regex — the "what if I just try every decoding" machine | Free web app at cyberchef.org (no install); self-hostable from the official GitHub repo (gchq/CyberChef) |
| **gdb** | The GNU debugger: step through a program, inspect memory and registers — essential for pwn | `apt install gdb`, `brew install gdb` (macOS may need codesigning steps — verify); often preinstalled in Kali |
| **git** | Cloning tools, tracking your own write-ups and scripts | `apt install git`, `brew install git`, git-scm.com, preinstalled in most dev environments |

### Web — breaking and inspecting web apps

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **Burp Suite Community** | Intercepts and edits HTTP requests/responses between your browser and the server — the core workflow of web challenges | Free Community edition from portswigger.net (account required to download; verify current terms) |
| **curl** | Send arbitrary HTTP requests from the terminal; scriptable, precise, no browser noise | Preinstalled on Linux/macOS/WSL; `apt install curl` otherwise |
| **Browser dev tools** | Inspect cookies, storage, requests, and client-side code (Elements/Network tabs) | Built into every modern browser — free |
| **sqlmap** | Automates detecting and exploiting SQL injection ⚠️ **lab-only** — point it ONLY at challenges/your own WebGoat instance, never at a site you don't own | sqlmap.org or `apt install sqlmap` — **legal only against authorized targets** |

### Crypto — ciphers, hashes, RSA

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **CyberChef** | First stop for encoding puzzles: base64, hex, rot13, XOR, frequency analysis recipes | Web app (see General) |
| **pycryptodome** | Python library for AES, RSA, hashes — implement attack code instead of hand-rolling crypto | `pip install pycryptodome` |
| **RsaCtfTool** | Attempts common RSA attacks (small e, common moduli, Wiener, etc.) — a *mention-level* tool: check its GitHub for current install steps and Python requirements, as dependencies change and the project may lag | From its official GitHub repository — **verify current install instructions** |
| **hashid** | Identifies which hash *algorithm* produced a hash string — step one before attempting to crack it | `pip install hashid` or `apt install hashid` (verify package availability) |

### Reverse engineering — what does this binary do?

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **Ghidra** | Free, open-source disassembler/decompiler (from the NSA) — reads ELF/PE and shows pseudo-C code; the standard free RE suite | Free download from the official Ghidra site (ghidra-sre.org / GitHub NationalSecurityAgency/ghidra); requires a JDK |
| **file** | Identifies file type before you touch it (ELF, zip, pcap…) — always step 1 | Preinstalled on Linux/macOS/WSL (`apt install file`) |
| **strings** | Extracts readable text from any binary — often finds embedded flags in beginner RE | Preinstalled with binutils (`apt install binutils`) |
| **radare2 / rizin** | Terminal-based disassemblers — lighter/faster than Ghidra, scriptable, great for small crackmes | rizin via `apt install rizin` or its GitHub releases; radare2 via its official repo/install script — **verify current methods** (package names change) |
| **ltrace / strace** | Watch a program's library calls / system calls at runtime — the fastest way to see what a binary *does* when executed | `apt install ltrace strace` (often preinstalled in Kali) |

### Pwn — exploiting programs

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **pwntools** | Python framework for CTF pwn: packing addresses, talking to remote services, ROP helpers | `pip install pwntools` (on Linux you may need Python dev headers first, e.g. `apt install python3-dev` — verify) |
| **checksec** | Shows a binary's protections (NX, PIE, stack canary, RELRO) — tells you which exploit strategy is even possible | Ships with pwntools (`pwn checksec <file>`); standalone scripts exist on GitHub — verify current |
| **GDB + GEF or pwndbg** | Interactive debugging with a *CTF-friendly* plugin: readable registers, stack, and memory views | `apt install gdb`, then install GEF (github.com/hugsy/gef) or pwndbg (github.com/pwndbg/pwndbg) via their current install commands — verify |

### Forensics — finding what's hidden

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **binwalk** | Scans files for embedded payloads/archives — "is there another file inside this file?" | `apt install binwalk` (note: pip and distro versions can differ — verify which you're getting) |
| **foremost** | Carves embedded files (images, docs) out of a larger file or disk image | `apt install foremost` |
| **exiftool** | Reads image/document metadata: camera, author, GPS, timestamps — favorite hiding spot for clues | `apt install libimage-exiftool-perl` (package name varies by distro — verify) |
| **steghide** | Extracts data hidden in images/audio with a passphrase (often the passphrase *is* the clue) | `apt install steghide` |
| **zsteg** | Finds LSB/hidden data in PNG/BMP images — the standard image-stego scanner | Ruby gem: `gem install zsteg` (requires Ruby — verify) |
| **Wireshark** | GUI packet analyzer: inspect captured network traffic, follow TCP streams, filter protocols | Free from wireshark.org; `apt install wireshark` (may need extra group setup — verify) |
| **Volatility** | Memory forensics: extract processes, connections, and secrets from RAM dumps | Volatility 3 from its official GitHub (verify install: often `pip install volatility3` or from source); older Volatility 2 still appears in older write-ups — check which the challenge expects |

### OSINT — public-data reconnaissance

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **theHarvester** | Gathers emails, subdomains, and names from public search sources | `apt install theharvester` or from its GitHub repo — verify |
| **Google dorks** | Search operators (`site:`, `filetype:`, `intitle:`) that surface indexed-but-obscure pages | Free — just a search engine; learn the operator syntax |
| **TinEye / reverse image search** | Find where an image appeared online, or find higher-resolution/original versions | Web service at tineye.com (and other engines' reverse-search features) — free tiers vary, verify |

⚠️ **OSINT rule:** public does not mean fair game for *harassment*. In CTFs, use only the challenge's stated scope; outside CTFs, respect privacy and local law.

### Misc — grab bag

| Tool | What it is / why you need it | How to get it (verify current) |
|---|---|---|
| **zbarimg** | Decodes QR codes and barcodes from image files — common in beginner misc/forensics | `apt install zbar-tools` (provides `zbarimg`) |
| **Python one-liners** | `python3 -c "..."` for quick math, conversions, and file surgery — often faster than hunting for a tool | Built in with Python 3 |
| **hashcat / john** | Password-hash crackers ⚠️ **lab-only** — use on challenge files and your own test hashes, never on data you aren't authorized to crack | `apt install hashcat john` — **legal only against authorized targets** |

## Which tool for which question

When you open a challenge and don't know where to start, find your *question* in this table:

| Your question | Start with | Then |
|---|---|---|
| "What kind of file is this?" | `file` | `binwalk` (embedded?), `strings` (readable text?) |
| "Is there hidden readable text?" | `strings \| grep -i flag` | CyberChef (encoding?) |
| "Is another file hidden inside this?" | `binwalk` | `foremost` to carve it out |
| "What's in this file's metadata?" | `exiftool` | Hex editor / manual inspection |
| "Is there data hidden *in* this image?" | `zsteg` (PNG/BMP) → `steghide` (passphrase?) | Visual inspection layer by layer |
| "What does this QR/barcode say?" | `zbarimg` | — |
| "Is this hash weak/what algorithm is it?" | `hashid` | john/hashcat (**lab-only**) |
| "Is this encoding or encryption?" | CyberChef (try base64/hex/XOR) | Python (real crypto → pycryptodome, RSA tools) |
| "What does this network capture show?" | Wireshark (display filter, follow stream) | Export objects from the capture |
| "What happened on this machine (memory dump)?" | Volatility 3 | Cross-reference process/network lists |
| "What does this binary do?" | `file` → `strings` → run it (`ltrace`/`strace`) | Ghidra or rizin for the full picture |
| "How do I exploit this binary?" | `checksec` (which protections?) | gdb+GEF/pwndbg to debug → pwntools to script |
| "Is this web request tamperable?" | Browser dev tools | Burp Suite to intercept/edit; curl to replay |
| "Who/what is behind this public clue?" | Google dorks, theHarvester, TinEye | Stay strictly within the challenge's scope |

**Heuristic:** cheap checks first (`file`, `strings`, metadata), expensive analysis last (decompilers, memory forensics). Most beginner flags die in the first two rows.

## VM setup note

Many CTF tools are Linux-native. You have three reasonable options:

| Option | Pros | Cons | Notes |
|---|---|---|---|
| **Kali Linux or Parrot OS in a VM** (VirtualBox/VMware) | Huge toolset preinstalled; isolated from your main OS; free | Needs ~4–8 GB RAM allocated; some overhead | Download ISOs **only from official sites** (kali.org, parrotsec.org) and verify checksums — never random mirror links |
| **Install tools on your own OS** (Ubuntu/macOS/Windows + WSL2) | Fast, no VM overhead; tools integrate with your files | Setup is manual, tool coverage varies; macOS/Windows gaps for some RE/pwn tools | WSL2 on Windows is now a very practical middle ground for most categories |
| **Hybrid** — WSL2/host for scripting + one Kali VM for heavy tooling | Best of both | Two environments to maintain | Common working setup |

**Setup tips:**
- Whatever you choose, **install the General set first** (Python 3, CyberChef bookmark, git) — it unblocks every category.
- Keep your practice VMs **snapshotted**: take a clean snapshot after setup so you can roll back after messy experiments.
- Note in your write-ups which environment you used (readers on other systems need to know).
- Platform-provided instances (HackTheBox, TryHackMe, OverTheWire, PortSwigger labs) don't require a VM at all — you interact with *their* environment.

## Golden rule

> **Tools are only as legal as the target you point them at.**

Burp Suite, sqlmap, nmap-class scanners, hashcat — none of them are illegal to *own* or *learn*. What determines legality is the target:

- ✅ **Practice on given challenges only:** your own lab VMs, apps you run yourself (WebGoat, VulnHub), and the instances the CTF platform explicitly gives you.
- ❌ **Never point them at anything else:** other people's websites, your employer's or school's systems, friends' devices, or "just scanning" random hosts.

A tool used on a platform-provided target is *practice*. The identical command aimed at a system you weren't given is potentially a crime in most jurisdictions — the binary doesn't change; the authorization does.

**Before every command, be able to answer: "Was I *given* this target?"** If the answer isn't a clear yes, stop.

---

**Related:** [Practice Platforms](platforms.md) · [Writing Write-ups](writeups.md)


## See also

- [CTF Categories](../fundamentals/ctf-categories.md)
- [Practice Platforms](platforms.md)
- [Web Exploitation Challenges](../challenges/web.md)
- [Writing Write-ups](writeups.md)
