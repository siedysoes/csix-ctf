# What Is a CTF?

A Capture The Flag (CTF) is a cybersecurity competition where the organizers hand you a set of puzzles — a website, an encrypted message, a binary program, a disk image — and your job is to dig the hidden string out of it. That string is the **flag**. Submit it to the scoring server and your team earns points. The whole game is a race: solve as many of the given challenges as you can before the clock runs out.

Everything you attack in a CTF was built to be attacked. The website is *meant* to be broken, the cipher is *meant* to be cracked. That is what makes CTFs one of the safest and fastest ways to practice real security skills.

## Flags: the universal currency

Every challenge hides a flag, and every flag follows a pattern: a recognizable prefix, curly braces, and a message inside.

```
flag{some_hidden_string_here}
CTF{another_format}
picoCTF{yet_another_prefix}
```

**Flag formats vary per event.** One competition uses `flag{...}`, the next uses `CTF{...}`, another uses a branded prefix like `picoCTF{...}`. Always check the event's rules page or challenge description for the exact format. Flags are usually case-sensitive and must be submitted exactly as found — `Flag{...}` is not the same as `flag{...}`.

The inside of the flag is the reward for solving the puzzle: it might be a phrase (`flag{read_the_source}`), a hex string, or a hint at what you just learned.

## Where CTFs came from

CTFs trace their roots to the "Capture the Flag" hacking games played at DEF CON, the security convention that has run since 1993, where teams raced to find flags hidden on rival teams' networks while defending their own. From those roots the format split into the two main styles you'll meet today: **jeopardy** (solve independent puzzles for points — covered in [Formats & Scoring](competition-formats.md)) and **attack-defense** (break into other teams' live services while keeping yours alive).

## Why CTFs are one of the best ways to learn security

- **A safe, legal playground.** You get real attacks — SQL injection, memory corruption, cipher-breaking — with zero legal risk, because the target exists solely for you to break it. No permissions to negotiate, no risk of collateral damage.
- **Pressure that builds skill.** A clock, a leaderboard, and teammates create the kind of focused urgency that tutorials never do. You learn to make decisions with incomplete information — exactly like the job.
- **Breadth without commitment.** One event exposes you to web, crypto, reverse engineering, forensics, and OSINT in a single weekend, so you discover what you're actually good at before you specialize.
- **Instant feedback.** The flag is either right or wrong. There is no partial credit for a long explanation — you know immediately whether your mental model of the system was correct.

## Who plays CTFs

- **Students** — the classic entry point; university clubs run practice events, and beginner CTFs are designed for people with one semester of security knowledge or less.
- **Working professionals** — red-teamers, blue-teamers, pentesters, and developers sharpen specific skills and prove them on a public scoreboard.
- **Career switchers** — a few CTF results plus public write-ups are a concrete portfolio that says "I can actually do this," often louder than a certificate.
- **Hobbyists and teachers** — people who just enjoy puzzles and keep the community running.

You do not need to be a genius; you need to be curious and willing to look things up under time pressure.

## What you actually learn

| Skill | Where it comes from in a CTF |
|---|---|
| Web exploitation | Web challenges: broken logins, injection, tampered parameters |
| Cryptanalysis | Crypto challenges: broken ciphers, misconfigured RSA setups |
| Reverse engineering | Rev challenges: taking apart binaries to find the secret check |
| Binary exploitation | Pwn challenges: memory corruption in provided programs |
| Forensics & analysis | File, PCAP, and memory-dump challenges ("what happened here?") |
| OSINT & research | OSINT challenges: finding what a person or company leaked publicly |
| Scripting & automation | Nearly every category — Python to decode, fuzz, or brute-force for you |
| Communication | Write-ups: explaining your solution clearly enough that others can repeat it |
| Teamwork & time management | The clock and the shared scoreboard force you to divide and conquer |

## Where CTFs happen

| Event / resource | Level | What it is |
|---|---|---|
| picoCTF (now on Carnegie Mellon's CyLab Security Academy) | Beginner | The classic first CTF — CMU's free, puzzle-based learning competition, ideal for your first flags |
| PlaidCTF | Top tier | Annual jeopardy-style event run by the Plaid Parliament of Pwning (PPP), one of the strongest teams in the world |
| DEF CON CTF | Top tier | The flagship CTF tied to DEF CON, historically attack-defense, where the world's best teams meet in the finals |
| [CTFtime](https://ctftime.org/) | All levels | Not an event — the community's calendar: upcoming and past events, team rankings, challenge lists, and a huge archive of public write-ups |

Schedules, formats, and platforms change over time — verify current details on each event's official site before you register.

## Your first flag: a walkthrough

Picture your first event. The board opens, you're staring at 40 challenge titles, and the lowest one says:

> **Warmup: What's in this file?** (50 points, forensics)
> Our intern left this file behind: `mystery`. Figure out what he was hiding.

Here's the whole solve, start to finish:

```bash
# 1. What even is this file?
$ file mystery
mystery: ELF 64-bit LSB executable, x86-64

# 2. It's a program — but maybe the string is just sitting in the data
$ strings mystery | grep -i flag
[...]
flag{r3vers1ng_1s_fun}
```

You paste `flag{r3vers1ng_1s_fun}` into the submission box (checking the prefix and capitalization first), hit submit, and the board turns green: **+50 points**. Nothing on your machine was broken, nothing illegal happened, and you just used a real reverse-engineering technique — the same `strings` trick professionals use for a first pass on unknown binaries.

That's the loop of every CTF: read the description, form a hypothesis, try a tool, adjust, repeat — until the flag appears.

## The rule that never bends: ethics

> ⚠️ **Only challenges given to you.** Everything in this learning base is about solving puzzles that are provided to you — in competitions, on practice platforms, and in your own lab. Never use these techniques against systems you do not own or do not have explicit permission to test. Outside the challenge scope, the same actions are simply unauthorized access — the flags don't exist there, and the consequences are real.

## See also

- [CTF Categories](ctf-categories.md) — which kind of puzzle suits you
- [Formats & Scoring](competition-formats.md) — how events are run and scored
- [Getting Started in CTFs](getting-started.md) — your first-event playbook
- [The CTF Toolkit](../practice/toolkit.md) — the tools used in the walkthrough above
