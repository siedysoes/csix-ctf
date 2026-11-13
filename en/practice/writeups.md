# Writing Write-ups

> Part of the **CSIX CTF** practice & resources section — [EN/ID bilingual learning base](../../README.md).
> Companion to [Practice Platforms](platforms.md) — this is step 4 of the study loop.
> **Educational framing: document challenges you solved yourself. Practice on given challenges only.**

## Why write-ups matter

A write-up is a record of *how* you solved a challenge — not just *what* the flag was. Writing one has four payoffs:

| Benefit | What happens |
|---|---|
| **Memory aid** | The act of writing forces you to reconstruct your reasoning. Studies of learning consistently show retrieval + explanation beats re-reading. Six months later, your write-up is the only proof of how you solved it — flags fade, methods don't. |
| **Portfolio** | A public folder of clear write-ups is the cheapest, most honest proof of skill you can show a teammate, mentor, or employer. Anyone can claim "I know web security"; a walkthrough of 15 solved challenges *shows* it. |
| **Teaches better than reading** | Reading a solution teaches recognition; *writing* your solution requires you to know why each step worked. If you can't explain it, you didn't learn it — better to find that out now than in a competition. |
| **Community contribution** | Beginner write-ups are disproportionately valuable: experts write for experts, and beginners write for the *next* beginner — the person googling your exact error message at 2 a.m. |

**The rule of thumb:** if solving took you 30 minutes, budget 10 minutes to document it. The document is part of the solve, not optional homework.

## Write-up structure

Every good write-up answers the same questions in the same order:

| Section | What goes in it | Length |
|---|---|---|
| **Title + metadata** | Challenge name, event/platform, category, points (if any), date solved, difficulty (your opinion) | 3–5 lines |
| **Summary** | Two sentences: the challenge in one, your result in the other. Someone should know if this write-up is relevant without reading further. | 2 lines |
| **Prerequisites** | Tools, files, and prior knowledge needed to follow along (e.g., "Linux shell, `binwalk`, basic understanding of ELF files") | Bullet list |
| **Solution** | Step-by-step: what you did, the command/code, the output, **and why you did it**. Include dead ends briefly. | The bulk |
| **What I learned** | The transferable technique, not the flag. "Check embedded files with `binwalk` before assuming a file is what it claims" — that travels to 50 future challenges. | 2–6 lines |
| **References** | Links to docs, other write-ups you consulted, the official challenge description. Credit where credit is due. | Bullets |

## Example: a full worked write-up skeleton

Below is a **fictional beginner challenge** in the classic picoCTF style — a binary with an embedded flag string. Copy this skeleton as your template. (The flag is a placeholder: `flag{...}` — never publish a real flag from a live competition while it's running.)

````markdown
# strings everywhere — a beginner RE warm-up

**Meta**
- Platform/Event: picoCTF-style practice (fictional challenge)
- Category: Reverse Engineering / Misc
- Points: 50 (practice)
- Solved: 2026-01-05 — took ~12 minutes
- Difficulty (my opinion): ★☆☆☆☆

**Summary**
A tiny binary refuses to print anything useful when run. Running `strings` on it
reveals a readable flag embedded right in the executable.

**Prerequisites**
- A Linux shell (native, WSL, or a VM — see [toolkit.md](toolkit.md))
- `strings` (ships with binutils) and `file`
- No reversing experience required

**Solution**

1. **Identify the file before touching it.**

   ```
   $ file rev_strings
   rev_strings: ELF 64-bit LSB executable, x86-64, ...
   ```

   *Why:* `file` tells you what you're holding. An ELF executable behaves
   differently from a zip or a script — knowing the type picks the toolset.

2. **Look for readable text first — it's the cheapest signal.**

   ```
   $ strings rev_strings | head
   ...
   welcome to the challenge
   ...
   flag{example_only_not_a_real_flag}
   ...
   ```

   *Why:* beginner challenges often hide data in plain sight. `strings` extracts
   printable character runs; flags have a recognizable pattern (`flag{...}`,
   `picoCTF{...}`, etc.), so grep-able output often solves it outright.

3. **Verify, don't guess.** Copy the candidate flag and submit it to the
   platform's checker to confirm it's the intended answer.

   *Why:* `strings` can produce coincidental matches (library names, error
   messages). Only the checker confirms you found *the* flag.

**Dead ends (kept brief on purpose)**
- First I just ran `./rev_strings` — it printed garbage/asked for input and
  exited. Noted: running unknown binaries is safe *inside this challenge*, but
  I should still check the file type first (that's what step 1 is for).

**What I learned**
- Order of operations for unknown files: `file` → `strings`/`binwalk` → only
  then execute or disassemble.
- "Hidden" data in beginner challenges is often *embedded*, not encrypted.
- Always confirm with the platform checker before trusting a candidate flag.

**References**
- Challenge description (fictional, created for this example)
- `man strings`, `man file`
- [The CTF Toolkit — general tools](toolkit.md)
````

**Anatomy of a great solution section:** every step has *the action*, *the output*, and *the why*. If you delete the "why" paragraphs, you have a transcript; a transcript teaches nobody.

## Teaching-value rules

Apply these to every write-up you publish or keep:

1. **Explain WHY each step, not just what.** "I ran `binwalk -e`" is a transcript. "I ran `binwalk -e` because the file claimed to be a JPEG but was 40 KB larger than a JPEG should be — embedded payloads usually live past the image data" is teaching.
2. **Show the command AND the relevant output.** Commands let readers reproduce; output proves the command did what you claim. Trim noise (use `| head`, highlight the important lines), never fabricate it.
3. **Note dead ends — briefly.** One or two lines: what you tried, why it failed, what made you pivot. Dead ends teach readers to *recognize* wrong turns, and they make your write-up honest instead of suspiciously perfect.
4. **State assumptions.** "I assumed little-endian because the platform's intro module said so" helps readers who don't share your background.
5. **Name the transferable technique** in "What I learned." Flags are unique; techniques are reusable. Write for the technique.
6. **Credit sources.** If a hint, teammate, or another write-up got you past a wall, say so — it's professional and it teaches readers where to look.

## Spoiler etiquette

Flags are spoilers. Platforms differ on when — and whether — you may share them.

- **Check the platform's rules first.** Most distinguish between *practice/archived* challenges (write-ups welcome, often encouraged) and *live competitions* (write-ups usually prohibited until the event ends). Rules change; read the current policy page or ruleset, not a blog post from 2019.
- **During a live competition:** do not publish flags, full solutions, or hints that trivially yield the flag — on social media, Discord, blogs, or GitHub. This includes "helpful" partial solutions.
- **After the event:** many events explicitly invite write-ups; some still restrict certain categories (e.g., a sponsor's challenge). When in doubt, ask an organizer.
- **On practice platforms:** write-ups for solved/archived problems are generally fine, but don't post answers to challenges that are *currently* part of a running contest on that platform.
- **Blur or placeholder real flags** if you're unsure: `flag{...}` / `REDACTED` conveys the method without the answer.
- **Platforms that track copied flags:** some detect flags submitted from write-ups and may penalize accounts. Another reason the method — not the flag — is the point.

**When publishing, ask:** "Does this spoil a live event, or someone else's chance to struggle productively?" If yes to either, delay or redact.

## Where to publish

| Venue | Good for | Notes |
|---|---|---|
| **Personal blog** (any static site or hosted platform) | Full control, your own domain and style | Simplest starting point — even a single `writeups.md` works |
| **GitHub repository / GitHub Pages** (like this CSIX CTF base) | Version-controlled write-ups, easy to link, free | Great for portfolios; keep a consistent folder structure (`category/challenge-name.md`) |
| **CTFtime write-up aggregation** | Discoverability — readers search CTFtime for event write-ups | Verify current availability, submission rules, and account requirements on ctftime.org before relying on it; policies change |
| **Platform-native solutions/forums** | In-platform discussion after events | Some platforms have official solution threads — check what's allowed there |
| **Communities (Discord/Reddit/team blogs)** | Feedback from peers | Respect each community's spoiler rules before posting flags |

Whatever you choose: pick **one** home and publish consistently. Ten write-ups in one tidy repo beat fifty scattered across five platforms.

## Documentation habits while solving

Good write-ups are born *during* the solve, not after. Keep a scratch file open from minute one:

```
## 2026-01-05 14:02  challenge: rev_strings (picoCTF-style, 50pts)
- downloaded file, ran `file` -> ELF 64-bit executable
- ran ./rev_strings -> asks for input, exits. dead end?
- ran strings -> spotted flag{example...} near line 40
- submitted -> accepted
- lesson: file -> strings -> only then execute/disassemble
```

**Habits that make write-ups trivial to write later:**

| Habit | Why it works |
|---|---|
| **Timestamps on every entry** | Reconstructing your timeline later takes seconds instead of guesswork |
| **Paste commands as you run them** (copy from terminal history) | No "what was that flag again?" — you have the exact syntax |
| **Paste trimmed output** (`| head`, screenshot the interesting part) | The evidence you based your decision on is preserved |
| **Screenshot key moments** (Burp request, Ghidra decompilation, Wireshark filter) | Visual proof you can embed later; saves re-doing the setup |
| **One file per challenge, or one running log with clear headers** | Either works — consistency matters more than format |
| **Record dead ends as you hit them** | You *will* forget the dead end in 20 minutes; note it now |
| **Note the "why" the moment you figure it out** | The insight is the fragile part; commands are easy to re-derive |

Ten minutes of honest notes during the solve = a finished write-up in ten minutes after.

## Quality checklist

Before you publish (or even archive) a write-up, tick these:

- [ ] **Metadata complete:** event/platform, category, points, date, self-rated difficulty
- [ ] **Summary fits in two sentences** and tells a reader whether this is relevant to them
- [ ] **Prerequisites listed** — a newcomer knows what they need installed
- [ ] **Every solution step has:** the action, the output (trimmed), and a *why*
- [ ] **Commands are copy-pasteable** and outputs are real, not imagined
- [ ] **At least one dead end or wrong turn noted** (honesty + teaching value)
- [ ] **"What I learned" states a transferable technique**, not just the flag
- [ ] **Sources credited** (docs, hints, other write-ups, teammates)
- [ ] **Spoilers handled:** no live-competition flags; real flags blurred if unsure
- [ ] **Platform rules checked** for *when* this solution may be shared
- [ ] **Flag/placeholder sanity check** — you're publishing a fictional or permitted flag only
- [ ] **Proofread once** — commands especially (a typo'd flag breaks every reader's reproduction)

## Common mistakes

| Mistake | Why it hurts | Fix |
|---|---|---|
| **Only the final flag** | Zero teaching value; you can't reproduce it; it's a spoiler with no upside | Every flag needs the path that produced it |
| **No explanation of steps** | Reader can copy but not learn; you probably didn't learn it either | Add a "why" sentence to each step |
| **Copy-pasted tool output without interpretation** | A wall of hex/log noise; readers can't see what mattered | Trim to the relevant lines and say what you noticed |
| **No metadata** | Impossible to find, categorize, or compare later | Fill the title block every time |
| **Write-up written from memory days later** | Steps get skipped, commands get garbled | Document during the solve (scratch file) |
| **Publishing live-competition flags** | Ruins the event for others; may violate rules; may penalize *your* account | Check spoiler policy; blur or wait until the event ends |
| **Claiming someone else's method as yours** | Ethically wrong and easily spotted | Credit sources — it costs nothing and builds trust |
| **No "what I learned"** | You documented an event, not a skill | Always name the reusable technique |
| **Perfectionism — never publishing** | A good-enough write-up beats a perfect unwritten one | Ship, then improve; your first five will be rough and that's fine |

---

**Next:** fill your scratch file with *good commands* → [The CTF Toolkit](toolkit.md) · back to [Practice Platforms](platforms.md)


## See also

- [Practice Platforms](platforms.md)
- [The CTF Toolkit](toolkit.md)
- [Getting Started in CTFs](../fundamentals/getting-started.md)
- [What Is a CTF](../fundamentals/what-is-ctf.md)
