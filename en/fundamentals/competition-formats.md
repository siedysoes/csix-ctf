# Formats & Scoring

Two questions decide how an event *feels*: what format is it running, and how are points calculated? Learn these before your first event and the scoreboard stops being mysterious.

## Jeopardy style

The format most people mean when they say "CTF":

- The board shows a list of **independent challenges**, each with a category, a point value, and a description.
- Any team can attempt any challenge at any time — there's no fixed order.
- Solve a challenge, submit the flag, earn that challenge's points. That's it.

Jeopardy is beginner-friendly because challenges don't depend on each other: if crypto isn't happening for you today, grab web warmups instead. Almost every beginner event, plus picoCTF-style practice platforms, runs jeopardy format.

## Attack-defense (brief)

The other classic format, and a step up in intensity:

- Every team runs an **identical copy of the same set of services** (a web app, a mail server, a voting system...).
- You **defend** your own services (patch them, monitor them) while **attacking** other teams' copies to steal their flags.
- Points come from both: successfully attacking others *and* keeping yours alive.

Attack-defense rewards teams that can do everything at once — it's fast, chaotic, and generally not where you take your first event. Get comfortable in jeopardy first; many events then mix in an attack-defense round for advanced teams.

## Other formats (one-liners)

- **Kingslayer** — teams don't have a permanent score; whoever currently holds the "crown" gains points while others try to take it, so the leaderboard keeps moving.
- **Attack-defense + jeopardy mix** — some events run both: a jeopardy qualification round that seeds a finals attack-defense, or side by side during the same weekend.

Check the event page before you register: format changes how you should prepare as a team.

## Dynamic scoring: why early solves pay more

Most modern events don't keep static points. Instead, a challenge's value **decays as more teams solve it**:

| Teams that had solved it when you submitted | Points for a 500-point challenge (illustrative) |
|---|---|
| 1–5 | ~500 (full value) |
| 10 | ~450 |
| 30 | ~350 |
| 100 | ~150 |

The logic is simple: a challenge everyone solves isn't separating the field anymore, so its value drops. The first team to solve it gets the maximum reward.

> **Exact formulas vary per event.** Some use smooth curves, some use tiers, some stay static — always read the event's rules to see how points move. The *principle* is universal though: **speed is worth points.**

## Timeboxing

CTFs run against a clock. **48 hours is typical for major events** (a full weekend), but it varies — you'll also find 12-hour, 24-hour, and even week-long formats, plus practice platforms that never close. A 48-hour event is a marathon with sprint intervals: the schedule in [Getting Started in CTFs](getting-started.md) is built around that.

## CTFtime: the calendar and the record book

[CTFtime](https://ctftime.org/) is the community hub every CTF player uses — verified as a live resource at the time of writing:

- **Events** — upcoming and past events with dates, formats, and weights.
- **Teams** — team pages and yearly ratings showing who's strong right now.
- **Tasks** — challenge lists per event, so you can see what a format actually asked.
- **Write-ups** — a huge archive of public solutions, which is where you go *after* an event to learn what you missed.

Bookmark it before your first event: it's how you find competitions, how you find teammates, and how you study.

## Team dynamics: who does what

Jeopardy is a team sport. A functional squad usually looks something like this:

| Role | What you do | Good if you... |
|---|---|---|
| Web person | Intercept requests, test logins, chase parameters | Like clicking around apps and reading HTTP |
| Crypto person | Spot encodings, run attacks, factor numbers | Enjoy patterns and math puzzles |
| Rev/pwn person | Disassemble binaries, script exploits | Like programs, debuggers, and low-level detail |
| Notes person (scribe) | Track solved/unsolved, keep the shared notes file, submit flags | Are organized and calm under pressure |
| Generalist | Sweeps warmups in every category, then supports whoever's stuck | Like variety and switching constantly |

The notes role is the unsung hero of every winning team — see [Getting Started in CTFs](getting-started.md) for the notes habit. In a small team everyone wears several hats; what matters is that *someone* owns the flag submissions and the shared notes.

## Anti-cheat basics

Every event publishes its own rules, but the common ground is:

- **One account per team.** Sharing logins or splitting a team across accounts is usually grounds for disqualification.
- **No cross-team solution sharing during the event.** Coordinating flags with another team is collusion; solve your own challenges.
- **Write-up policies vary.** Some events welcome write-ups immediately, some ask you to wait until the event closes — check the rules.
- **Organizers can detect suspicious patterns** — identical submissions across teams, account sharing from one IP, or solutions that appear nowhere else but your team's.

When in doubt: read the rules page once before the event starts. Ten minutes there prevents most disqualifications.

## A worked scoring example

Same challenge, same solution, two different moments — dynamic scoring in action:

```text
Challenge: "Sneaky Login" — listed at 500 points

Team A submits at 10:05, when only 10 teams had solved it
→ dynamic score applies: 450 points

Team B submits at 23:40, when 100 teams had solved it
→ dynamic score applies: 150 points
```

Both teams did identical work. Team A earned **3× more** for solving the same puzzle three hours earlier. Multiply that across a board of 40 challenges and you see why veterans sweep the low-hanging fruit in the first hours of an event — and why "first blood" (the first solve of a challenge) is so prized.

*(Illustrative numbers; every event's exact formula differs — see the rules.)*

## See also

- [What Is a CTF](what-is-ctf.md) — the basics this article builds on
- [CTF Categories](ctf-categories.md) — how difficulty bands map to categories
- [Getting Started in CTFs](getting-started.md) — turning format knowledge into an event plan
- [Writing Write-ups](../practice/writeups.md) — what happens with your solutions after the event
