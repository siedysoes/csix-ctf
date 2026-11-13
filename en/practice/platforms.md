# Practice Platforms

> Part of the **CSIX CTF** practice & resources section — [EN/ID bilingual learning base](../../README.md).
> **Educational framing: practice on the challenges these platforms give you — nothing else.** Your own lab or the platform's provided instances only.

## Why practice platforms beat waiting for competitions

A CTF competition usually runs for 24–72 hours, once or twice a year. If your only plan is "wait for the next competition," you might touch your first challenge in months — and you'll be competing against people who practiced all along.

Practice platforms fix this in four ways:

1. **They never close.** A challenge you can't solve at 2 a.m. is still there tomorrow, with no scoreboard pressure.
2. **They are progressive.** Categories are sorted by difficulty, so you can start at "read a file's type" and end at "exploit a buffer overflow" without guessing what to learn next.
3. **They force repetition.** Solving 30 beginner web challenges teaches pattern recognition that one competition never will.
4. **They are safe by design.** Everything you attack is a target the platform *intends* you to attack — legal, ethical, and scoped by default.

Competition is the exam. Practice platforms are the semester. Do the semester first.

## The big comparison

> ⚠️ **Verify before you commit:** pricing, free tiers, and challenge access change often on every platform below. Always check the official site for the current terms — treat this table as a map, not a price list.

| Platform | Cost | Format | Level | Focus |
|---|---|---|---|---|
| **picoCTF** | Free | Annual competition + year-round practice problems | Beginner | Mixed: web, crypto, RE, forensics, misc — the classic "first CTF" |
| **OverTheWire** | Free | Wargames (sequential levels, SSH into a server) | Beginner → Intermediate | Linux command line, security fundamentals, basic exploitation |
| **CryptoHack** | Free / paid mix | Interactive modules + challenge sets | Beginner → Advanced | Cryptography: encoding, classical ciphers, RSA, elliptic curves |
| **HackTheBox** | Free / subscription for some content | Hosted machines, labs, challenges, tracks | Intermediate → Advanced | Penetration testing on full machines; some content needs a paid tier |
| **TryHackMe** | Free rooms + paid subscription for some | Guided "rooms" with tutorials built in | Beginner | Hand-held learning paths; great first step after picoCTF |
| **Root-Me** | Free / paid for some | Standalone challenges, many categories | Beginner → Advanced | Web, programming, forensics, reversing, crypto, network — broad coverage |
| **CTFlearn** | Free | Community-written standalone challenges | Beginner | Warm-up problems across categories, good for speed drills |
| **crackmes.one** | Free | Community-uploaded crackmes (reverse engineering) | Beginner → Advanced | Reverse engineering only — the deepest free RE pool |
| **PortSwigger Academy** | Free | Web security labs (theory + hands-on) | Beginner → Advanced | Web vulnerabilities — arguably the best free web curriculum |
| **WebGoat** | Free (self-hosted) | Intentionally vulnerable Java web app you run yourself | Beginner → Intermediate | Web security practice on your **own machine** |
| **VulnHub** | Free (downloadable VMs) | Pre-built vulnerable virtual machines you boot locally | Beginner → Advanced | Full-system exploitation, privesc, enumeration at your own pace |
| **CyberDefenders** | Free / paid mix | Blue-team labs: forensics, incident response, threat hunting | Intermediate | Defensive security and digital forensics — the "find what happened" side |

**How to read this table:**
- **Cost** is a starting point, not a contract — free tiers get trimmed or expanded over time.
- **Level** describes the *entry point*, not the ceiling; every platform has hard content.
- **Focus** tells you which skill you'll actually be drilling.

## How to choose: your interest → starting platform

| If you're curious about… | Start here | Then move to… |
|---|---|---|
| "I don't know anything yet, where do I begin?" | **picoCTF** practice problems | **TryHackMe** beginner rooms |
| Linux command line & shell basics | **OverTheWire Bandit** | **TryHackMe** Linux rooms |
| Breaking websites (logins, cookies, injections) | **PortSwigger Academy** | **Root-Me** web challenges → **HackTheBox** web |
| Codes, ciphers, "is this encryption breakable?" | **CryptoHack** | **Root-Me** crypto challenges |
| "What does this binary *do*?" | **crackmes.one** (easy ones) | **picoCTF** RE → **VulnHub** VMs |
| Exploiting programs (overflows, shells) | **picoCTF** pwn → local practice VMs | **HackTheBox** machines |
| Hidden data in images/audio/files | **picoCTF** forensics | **CyberDefenders** labs |
| "Who left this clue on the internet?" (public data) | **Root-Me** / **picoCTF** OSINT-flavored tasks | Dedicated OSINT rooms on **TryHackMe** |
| Defending / investigating attacks | **CyberDefenders** | **TryHackMe** blue-team paths |

**Pick ONE starting platform.** Two tabs of unfinished accounts is the most common beginner failure mode.

## The study loop

Every challenge you solve should go through the same four steps. Repeat until it's automatic.

```
pick  →  timebox  →  document  →  review write-ups
```

| Step | What you do | Rule |
|---|---|---|
| **1. Pick** | Choose one challenge slightly above your current level (the "one step up" rule) | One at a time. Finish it before opening another. |
| **2. Timebox** | Set a timer — 30–45 min for beginners, 60–90 min for harder ones | When it rings: either solve it, or write down exactly where you got stuck. *Stopping on purpose is not quitting.* |
| **3. Document** | While solving, keep a scratch file: commands, outputs, ideas, timestamps | A solved challenge you didn't document teaches you half as much. |
| **4. Review write-ups** | Read 1–2 other people's solutions **after your own attempt** | Compare methods: "Did they use a shorter path? Why did my approach fail?" |

Then loop back to step 1 with a slightly harder challenge.

**Why "after your attempt" matters:** reading a write-up before trying feels efficient but builds *recognition*, not *skill*. You'll recognize the solution next time but not be able to produce it. Struggle first — the write-up then makes sense instead of magic.

## Using write-ups ethically

Write-ups are study material, not answer keys.

**Do:**
- ✅ Attempt the challenge first, within your timebox.
- ✅ Use a write-up to unstick a specific step you *understand the goal of* but can't execute ("I knew I needed to extract the file — I didn't know the `binwalk` flag").
- ✅ Learn the **method**: what was the question, what tool answered it, why.
- ✅ Re-implement the technique on a *different* challenge to prove you learned it.
- ✅ Credit the author when you reference their solution.

**Don't:**
- ❌ Copy-paste the flag from a write-up into the submission box. You learn nothing, and many platforms track it.
- ❌ Bulk-solve by opening write-up tabs for a whole category. This is grinding your scoreboard, not your skills.
- ❌ Publish a live-competition flag while the event is running (see [writeups.md — spoiler etiquette](writeups.md)).
- ❌ Use write-ups to bypass a platform's rules about *when* solutions may be shared.

**The test:** if someone erased your write-up and asked you to solve a *similar* challenge tomorrow, could you? If yes, you used it right.

## Your first 30 days

A concrete plan for a beginner with ~1 hour/day. Adjust the pace, keep the order.

| Days | Focus | Platform | Deliverable by the end |
|---|---|---|---|
| **1–3** | Orientation: accounts, mindset, tool tour | picoCTF practice | 2 easy challenges solved; scratch file created; toolkit.md read |
| **4–7** | Linux & shell basics | OverTheWire (Bandit) | 15–20 levels done; notes on 10 commands you now know |
| **8–12** | Web fundamentals | PortSwigger Academy (basic labs) + picoCTF web | 5 web labs solved; Burp Suite installed and proxy working |
| **13–17** | Crypto fundamentals | CryptoHack (intro modules) | Encoding/classical-cipher modules complete; 3 challenges solved |
| **18–22** | Reverse engineering basics | crackmes.one (easy) + picoCTF RE | 3 crackmes solved; `strings`/`file`/`Ghidra` used at least once each |
| **23–26** | Forensics basics | picoCTF forensics + CyberDefenders (intro labs) | 3 forensics challenges solved; `binwalk` + `exiftool` used for real |
| **27–30** | Review & consolidation | Revisit your hardest solve + read 3 write-ups | 3 polished write-ups (per [writeups.md](writeups.md)); a "what I'm still weak at" list |

**Rules for the 30 days:**
- One platform in focus at a time (others may be idle — that's fine).
- Every solved challenge gets at least a few lines in your scratch file.
- Stuck for 2× your timebox? Write down the blocker, peek at *one* write-up for that step only, continue.
- At the end of Day 30: pick your *next* 30-day focus from the "How to choose" table.

After that, consider entering a beginner-friendly competition (many run online and allow practice-era skills) — but keep the practice loop running either way.

## Safety note — scope every target

The single rule that keeps CTF practice legal and fun:

> **Practice on the challenges the platform gives you — on their servers, their hosted instances, or your own lab VMs. Nothing else.**

- ✅ **Your own lab:** WebGoat running locally, a VulnHub VM in VirtualBox, crackmes you downloaded, deliberately vulnerable apps in a sandboxed VM.
- ✅ **Platform-provided instances:** HackTheBox/TryHackMe machines they explicitly hand you, OverTheWire's game servers, PortSwigger's hosted labs.
- ❌ **Anything else:** other people's websites, your company's systems, your school's network, random IPs you found, "just scanning to see."

A tool's legality depends entirely on **where you point it**. Scanning a CTF platform's provided target = practice. Scanning your neighbor's router = a crime in most jurisdictions. The tools in [toolkit.md](toolkit.md) are all standard — the golden rule at the bottom of that page applies to every one of them.

**Practical guardrails:**
1. Do aggressive testing only inside VMs or the platform's instances.
2. Keep personal/production data off your practice VMs.
3. If a challenge seems to require attacking something *outside* its described scope — stop and report it to the platform.
4. When in doubt, the scope is the challenge description. Read it twice.

---

**Next:** learn to turn every solve into a reusable artifact → [Writing Write-ups](writeups.md) · [The CTF Toolkit](toolkit.md)


## See also

- [Getting Started in CTFs](../fundamentals/getting-started.md)
- [CTF Categories](../fundamentals/ctf-categories.md)
- [Writing Write-ups](writeups.md)
- [The CTF Toolkit](toolkit.md)
