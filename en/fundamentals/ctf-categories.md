# CTF Categories

CTF challenges are grouped into categories, and each category is a different flavor of puzzle with its own tools and its own way of thinking. You don't need all of them to start — you need one you enjoy, plus enough familiarity with the rest to know when to ask a teammate.

## The categories at a glance

| Category | What you get | What you solve | Key skill |
|---|---|---|---|
| **Web** | An HTML/JS application (and often its source) | Find the flaw in a login form, a parameter, or the app's logic | How the web really works — HTTP, sessions, injection |
| **Crypto** | A cipher, hash, or encrypted message | Break the *implementation or setup*, not the math itself | Pattern recognition + modular arithmetic |
| **Rev** | A compiled binary (ELF/EXE) | Take the program apart until you understand its secret check | Reading disassembly, using a debugger |
| **Pwn** | A vulnerable binary, sometimes with a live service | Corrupt its memory to make it do what you want | Stack/heap layout, exploit development |
| **Forensics** | Files, disk images, PCAPs, memory dumps | Reconstruct what happened from artifacts | Tooling: carving, sniffing, timeline analysis |
| **Misc** | Anything: images, audio, scripts, data | Stego puzzles, encodings, programming tasks | Breadth, encoding knowledge, scripting |
| **OSINT** | A person, company, or event description | Find the information they left public | Clever research and Google-fu |
| **Blockchain** | A smart contract (often on a provided testnet) | Find the bug — reentrancy, bad access control — and drain the contract | Solidity semantics and contract logic |

Misc quietly contains a lot of points: **steganography** (hiding data in images/audio), **encoding chains** (base64 → hex → ROT13), and quick **programming** tasks where you write a script to generate the answer.

## Choosing your first category

Use this as a decision aid — pick whatever matches how you already like to think:

- **You like puzzles, patterns, and math** → start with **crypto**. Classical ciphers are solvable with pencil-and-paper logic plus one tool.
- **You like reading code and poking at apps** → start with **web**. The feedback loop is fast: click, request, response.
- **You like taking things apart** → start with **rev**. If you've ever wondered "how does this program know?", that's rev.
- **You like tools, artifacts, and detective stories** → start with **forensics**. Every challenge is "what happened here?"
- **You like researching people and stories** → start with **OSINT**. The only tool you truly need is a search box.
- **You just want your first flag fast** → take the lowest-point **misc** and **web warmups**; most events seed easy wins in every category.

Whichever you pick, read every challenge description anyway — an easy flag sometimes hides in a category you'd skip.

## Growing with the CSIX ecosystem

The CSIX family of learning bases goes deeper than a CTF event ever will. After (or alongside) your CTF practice, these companion sites take you from "solved the challenge" to "understand the discipline":

- **[CSIX Pentest](https://siedysoes.github.io/csix-pentest/)** — depth on web hacking: methodology, exploitation, and lab platforms for when web becomes your category.
- **[CSIX Reversing](https://siedysoes.github.io/csix-reversing/)** — depth on reverse engineering, for the day `strings` stops being enough.
- **[CSIX OSINT](https://siedysoes.github.io/csix-osint/)** — depth on open-source intelligence, for turning OSINT challenges into a real research skill set.

## How difficulty scales

Points are the event's rough estimate of how hard a challenge is — and a signal of how many stages are hiding inside:

| Point band (typical) | What it usually means |
|---|---|
| ~50–100 | Warmup: one trick, one tool, five minutes if you know it |
| ~150–250 | One solid concept plus real tooling — you'll get stuck once or twice |
| ~300–400 | Multi-stage: several ideas chained, custom scripts required |
| ~500 | The boss fight: specialized knowledge, heavy tooling, or hours of grind |

A beginner solving a 50-pointer is doing the same *kind* of work as a pro solving a 500-pointer — just with fewer layers peeled back. The skill is in the peeling.

> **Note:** point ranges vary by event. Some events cap everything at 500, others run 100–500, and a "100" in one event can be a "300" in another. Use the band as a rough guide, then trust the description.

## See also

- [What Is a CTF](what-is-ctf.md) — flags, history, and why this works
- [Formats & Scoring](competition-formats.md) — how points and difficulty interact with scoring
- [Getting Started in CTFs](getting-started.md) — how to pick your first event
- [Web Exploitation Challenges](../challenges/web.md) · [Cryptography Challenges](../challenges/crypto.md) — the two category deep-dives in this base
