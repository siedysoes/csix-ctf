# Web Exploitation Challenges

Web is the most common CTF category and the friendliest entry point: the target is a website, the feedback is instant, and your main tool is a browser. You'll meet broken logins, careless parameters, and hidden comments — every one of them planted there for you to find.

> ⚠️ **Scope reminder:** every technique here applies to challenge instances **given to you** — competition sites, practice platforms, your own lab. Never point them at a system you don't own or lack permission to test.

## The CTF web landscape

| Challenge type | What it tests | First thing to try |
|---|---|---|
| Hidden info in source | Patience, attention | View source (Ctrl+U): HTML/JS comments, hidden fields, disabled buttons |
| Cookies & localStorage | Session handling | Dump cookies/localStorage — look for roles (`admin=0`), flags, or plain-text tokens |
| IDOR / parameter tampering | Access control | Change `id=1` to `id=2` (or `role=user` to `role=admin`) and watch what changes |
| SQL injection in login/search | Input handling | `'` to provoke an error, then classic payloads like `' OR 1=1--` |
| SSTI (server-side template injection) | Template engines | Probe with `{{7*7}}` or `${7*7}` — 49 in the response means injection |
| File inclusion (LFI/RFI) & XXE | File/path handling | `../` traversal, `file:///etc/passwd`, or an XML body that makes the server fetch a file |
| Auth bypass | Login logic | Default credentials, type-juggling, JWT `alg=none`, forced browsing to `/admin` |

Don't memorize the table — learn the *posture*: every string on the page is input, every value in storage is a suggestion, and every ID is a guess.

## The CTF web workflow

Run this sequence on every web challenge before you get clever:

1. **View the source** — real flags have been hidden in HTML comments more times than anyone would like to admit.
2. **Check the classics** — `robots.txt`, `sitemap.xml`, `/flag.txt`, hidden directories, and the JS bundle for endpoints or commented-out logic.
3. **Play with the app** — click every button, submit every form, note every request and parameter. You can't tamper with what you haven't mapped.
4. **Intercept with Burp** — proxy the traffic, then work in Repeater: modify one parameter at a time and compare responses.
5. **Inspect cookies and storage** — then tamper: change values, downgrade `admin=0` to `admin=1`, strip a signature, replay a request.

Boring, systematic, repeatable — that's what wins web points.

## The big three, hands-on

### SQL injection in CTF style

When user input is concatenated into a database query, your input becomes syntax. The CTF classic is a login bypass:

```text
username: admin' OR 1=1--
password: (anything)
```

- `'` breaks out of the string literal; `OR 1=1` makes the WHERE clause always true; `--` comments out the rest of the query (in MySQL, `#` works too).
- **Error-based detection first:** send `admin'` alone. A database error in the response (or a behavior change) tells you the input reaches a query — that's your green light before trying clever payloads.
- In modern CTFs the bypass is often only step one: the real flag appears when you **extract data** — e.g., dump the `users`/`flags` table with UNION SELECT or blind boolean techniques.

All of this is for **provided challenge instances only**. On a real production site, injection testing without written permission is a crime, not curiosity.

### XSS basics

Cross-site scripting means the app reflects your input as HTML/JavaScript. The CTF version of reflected XSS usually looks like this:

1. The challenge gives you a URL parameter or form that echoes your input.
2. You inject a script payload (start with the harmless proof: `<script>alert(1)</script>`).
3. **The twist:** many CTFs hand you an "admin bot" — a button like *Report to admin* that makes a bot visit your URL with the flag in its cookie. Your job is to exfiltrate that cookie (conceptually: the payload sends `document.cookie` to a server the challenge gives you, or you capture it in the challenge's own webhook).

That's why XSS matters: it's code running in *someone else's* session. In the challenge, that someone is the bot; on the real web, that someone is a user.

### IDOR and parameter tampering

The cheapest flag on the board is often a single digit:

```text
GET /api/notes?id=1   -> your note: "grocery list"
GET /api/notes?id=2   -> someone else's note: flag{d0nt_trust_cl1ent_ids}
```

IDOR (Insecure Direct Object Reference) means the server trusts an identifier you control instead of checking *your* permission on that object. The habit: find every ID, token, and role value in the app — then increment, decrement, swap, or delete it. The same instinct covers `role=user` → `role=admin`, `?admin=false` → `?admin=true`, and filename parameters.

## Burp Suite setup (quick note)

Burp Suite **Community Edition** is free and enough for CTF work:

1. Install Burp, create a project, and note the proxy listener: `127.0.0.1:8080` (its default).
2. Point your browser at it — use Burp's embedded browser (no setup) or your system proxy.
3. For HTTPS targets, visit `http://burpsuite` in the proxied browser and install the CA certificate so requests appear instead of errors.
4. Keep **Intercept off**, browse the challenge normally, then work from **Proxy → HTTP history** into **Repeater** (Ctrl+R), where you edit and replay requests one variable at a time.

Scope rule: set the proxy scope to the challenge host so you never accidentally capture — or touch — anything else.

## Script the boring parts (Python `requests`)

Hand-clicking a parameter through 200 values is how flags get missed. One habit solves it — when you notice repetition, script it:

```python
import requests

base = "http://challenge.local/api/notes"

# IDOR sweep: 200 requests in seconds
for i in range(1, 201):
    r = requests.get(base, params={"id": i})
    if "flag{" in r.text:
        print(f"id={i}: {r.text.strip()}")
        break
```

The same idea covers brute-forcing a Caesar shift, trying wordlists against a login, or extracting data one boolean at a time. If you do it twice by hand, do it once by script.

## First-approach checklist

- [ ] Read the full description and any given files (hints hide there)
- [ ] View source: HTML comments, hidden fields, JS logic
- [ ] `robots.txt`, `sitemap.xml`, obvious paths (`/flag`, `/admin`)
- [ ] Click through the whole app; map every page, form, parameter, cookie
- [ ] Proxy with Burp; move interesting requests to Repeater
- [ ] Tamper: IDs, roles, cookies, headers, HTTP methods
- [ ] Inject: `'`, `{{7*7}}`, `../`, `<script>alert(1)</script>`
- [ ] Check localStorage and cookies for tokens/roles/flags
- [ ] If repetition appears → write the Python script
- [ ] Re-read the description — sometimes the flag is the endpoint you missed

## Where to go deeper

This guide covers the CTF-flavored patterns. For the full discipline — methodology, OWASP Top 10 mapping, Burp workflows, and lab practice — read the companion **[CSIX Pentest: Web Exploitation](https://siedysoes.github.io/csix-pentest/#/en/exploitation/web-exploitation)** article.

## See also

- [CTF Categories](../fundamentals/ctf-categories.md) — where web fits on the board
- [Getting Started in CTFs](../fundamentals/getting-started.md) — the event workflow this fits into
- [Cryptography Challenges](crypto.md) — the other beginner-friendly category
- [The CTF Toolkit](../practice/toolkit.md) — Burp, Python, and the rest of the kit
