# Getting Started in CTFs

You've read [What Is a CTF](what-is-ctf.md) and picked a category. This is the playbook for the event itself — what to do before it opens, how to spend the hours while it runs, and what to do after it closes.

## Before the event

**1. Pick a beginner event.** Your first event should be a picoCTF-style beginner competition or a small community CTF — not a top-tier weekend where the board is full of 500-point boss fights. Browse upcoming events on [CTFtime](https://ctftime.org/), ask in communities, or just start on a practice platform (see [Practice Platforms](../practice/platforms.md)).

**2. Set up your tooling — see [The CTF Toolkit](../practice/toolkit.md).** Before the clock starts, make sure your basics work: Python, a terminal you're comfortable in (Linux, macOS, or WSL), a proxy for web (Burp Suite Community Edition), CyberChef for encodings, and `strings`/`file` for quick triage. Tooling problems during an event cost you flags; tooling problems *before* the event cost you nothing.

**3. Form or find a team.** Check university clubs, community Discords, and CTFtime team pages — most events are more fun and more effective with 2–4 people. Solo is completely fine for a first event too; just know that a team covers your blind spots (see the roles table in [Formats & Scoring](competition-formats.md)). Either way, agree beforehand: a shared notes file, a way to talk (voice chat helps enormously), and who submits flags.

## During the event: the solving workflow

Run this loop from the moment the board opens:

```text
1. READ EVERY CHALLENGE DESCRIPTION FIRST — no solving yet.
   Flag hints, given files, and "format: flag{...}" live in the descriptions.
2. SORT the board: category and points, lowest points first.
3. SWEEP THE WARMUPS — grab every 50–100 point flag across all categories.
4. BRANCH OUT: pick your strongest category, commit to one mid-point
   challenge and go deep on it.
5. STUCK ALONE for 20–30 minutes? Switch — new challenge, new category,
   or a teammate (bring what you already tried, see "asking for help" below).
6. LOG every solved flag in your notes file AS YOU GO: challenge, steps,
   final flag. It becomes your write-up later.
7. SUBMIT carefully: correct prefix, correct case, no stray whitespace.
```

The single biggest beginner mistake is skipping step 1 and diving into whichever challenge sounds cool. Descriptions routinely contain the hint you'd otherwise spend an hour rediscovering.

## Time management (48-hour events)

A 48-hour event is a weekend, not a sprint. Pace accordingly:

| When | Push on | Switch / stop when |
|---|---|---|
| First 2–3 hours | Warmups everywhere — bank easy points and learn the board | Don't deep-dive anything yet |
| Day 1 afternoon | Your strongest category; one mid-point challenge with a plan | 20–30 min stuck alone → switch (see workflow) |
| Day 1 night | Greedy attempts at hard challenges if you're sharp | You're tired and rewriting the same payload — that's the signal to stop |
| Overnight | Sleep (seriously) | Tired mistakes waste more time than sleep costs |
| Day 2 morning | Re-approach yesterday's stuck challenges with fresh eyes | Don't start brand-new categories unless you're caught up |
| Final hours | Verify submissions, re-read descriptions for missed flags, finish notes | No risky new dives — bank what you have |

Events of other lengths compress or stretch the same rhythm: warmup phase → deep work → rest → final sweep.

## First-blood thinking

Dynamic scoring (see [Formats & Scoring](competition-formats.md)) rewards speed: a challenge's value drops as more teams solve it. That means:

- **Warmups first isn't just safe, it's optimal** — every minute you spend on a 500-pointer while the warmups decay costs you points.
- **First blood** — being the first team to solve a challenge — is often worth bragging rights *and* maximum points. If you're close on a hard challenge early in the event, the clock is your ally.
- **Don't chase decay forever.** If a challenge has dropped to its floor value, the points for solving it are now fixed — solve it for learning, not for scoring.

## How to ask for help properly

"Help, this web challenge is broken" tells your team nothing. A good ask takes 30 seconds and gets you an answer in 60:

```text
Challenge: "Login As Admin" (web, 150)
Tried: ' OR 1=1-- on both fields -> "invalid login"
       view-source -> one comment, nothing else; robots.txt empty
Stuck on: quotes seem to get stripped (prepared statement?)
Next idea: boolean-based 1=1 vs 1=2 — want to try that?
```

Show the description, what you tried, what came back, and your next idea. Teammates can immediately say "drop that path, try X" or "you're right, keep going" — instead of re-solving from zero.

## The notes habit

Write down every solution **as you solve it**, not afterwards:

```text
## Warmup: What's in this file? (50, forensics)
- file mystery -> ELF executable
- strings mystery | grep -i flag -> flag{r3vers1ng_1s_fun}
```

Three reasons this pays off during the event: it frees your memory for thinking, it lets a teammate pick up a half-finished idea, and it survives the event — turning directly into your write-up (see [Writing Write-ups](../practice/writeups.md)). Teams that keep notes consistently outscore teams that don't, because they never re-solve what they already cracked.

## After the event

1. **Read other people's write-ups.** Find your event on CTFtime, open the tasks you failed, and read how others solved them. This is where the real learning compresses: one good write-up replaces hours of stuck time.
2. **Note your gaps.** List the categories you skipped and the techniques you didn't know. That list is your study plan for the next few weeks.
3. **Practice on platforms.** Reproduce the techniques on a practice platform so they're yours, not borrowed — see [Practice Platforms](../practice/platforms.md).
4. **Publish your own write-up.** Even a short one. It cements your learning, and it's your proof of skill for teammates and future events.

## Your first three events

| Event | Focus | Realistic goal |
|---|---|---|
| 1st | Experience the workflow — read all, sweep warmups, take notes | Finish with a handful of flags and a notes file; understand the mechanics |
| 2nd | Commit to your chosen category from [CTF Categories](ctf-categories.md) | Solve a mid-point challenge in that category **unassisted** |
| 3rd | Speed and teamwork | Contribute points on a team, ask good questions, publish your first write-up |

By the third event the board will feel familiar. That's the whole point — nobody's first event makes sense.

## Everyone's first event was confusing

It's worth saying plainly: the board looked like a foreign language to everyone who now makes it look easy. Veterans' first CTF had them staring at a description with no idea where to start, too. The confusion isn't a sign you're failing — it *is* the curriculum. Read descriptions, take the warmups, ask good questions, keep notes, and come back. The pattern repeats until one day you recognize the challenge type before you've finished reading it.

## See also

- [What Is a CTF](what-is-ctf.md) — the basics
- [CTF Categories](ctf-categories.md) — where to focus
- [Formats & Scoring](competition-formats.md) — how your event runs and scores
- [The CTF Toolkit](../practice/toolkit.md) · [Practice Platforms](../practice/platforms.md) · [Writing Write-ups](../practice/writeups.md) — prepare, practice, publish
