# Cryptography Challenges

Crypto challenges are puzzles about *broken setups*, not broken math. You will never be asked to attack AES, SHA-255's family, or the prime number theorem itself — real cryptography, properly implemented, is not breakable on your laptop. What CTFs give you instead is the other 90% of reality: an encoding that isn't encryption, a cipher used with a weak key, or an RSA key generated with a mistake. Your job is to spot which one you're holding.

> ⚠️ **Authorized targets only.** Everything here applies to challenge data handed to you by competitions and practice platforms. Breaking crypto on systems you don't own or lack permission to test isn't a challenge — it's a crime.

## First triage: look at the data before you think

Most "hard-looking" crypto challenges are an encoding wearing a disguise. Read the ciphertext with your eyes first:

| What you see | It's probably... | First tool |
|---|---|---|
| `A–Z a–z 0–9 + /` with `=` padding, length a multiple of 4 | Base64 | CyberChef → *From Base64*; `base64 -d` |
| `0–9 a–f` only, even length | Hex | CyberChef; `xxd -r -p` |
| `A–Z 2–7` with `=` padding (often ends `====`) | Base32 | CyberChef → *From Base32* |
| Dots, dashes, spaces | Morse code | dcode.fr → Morse |
| Only `A–Z`, uniformly shifted (gkg... looks "off") | Caesar / ROT13 | CyberChef ROT13; brute-force all 25 shifts |
| `A–Z` with *non-uniform* shifts | Vigenère | dcode.fr → Vigenère |
| Random-looking bytes, no charset | XOR or a modern cipher with a leaked weakness | Brute-force (see below) |

**[CyberChef](https://gchq.github.io/CyberChef/)** (browser-based, drag-and-drop recipe stacks) and **[dcode.fr](https://www.dcode.fr/)** (per-cipher solvers) between them handle most beginner crypto. When you see a string you can't classify, decode the layers you *do* recognize — chains are the norm, not the exception.

## Classical ciphers

These show up at warmup-to-mid difficulty and are breakable without any crypto background:

| Cipher | What it is | Attack |
|---|---|---|
| **Caesar / ROT** | Every letter shifted by a fixed amount | Brute-force all 25 shifts and eyeball the output; frequency analysis if it's a longer text |
| **Vigenère** | A repeating key shifts different letters differently | 1. Find the key **length**: Kasiski examination (repeated sequences) or the index of coincidence. 2. Treat each column as its own Caesar and solve by frequency |
| **Substitution** | One letter always maps to another | Frequency analysis (E/T/A are most common in English) + pattern words (`the`, `and`) + a solver like dcode.fr or CyberChef's *Substitution* |

The underlying skill is the same for all three: **compare what you have against what English (or whatever the plaintext language is) statistically looks like.** That instinct carries straight into harder challenges.

## The encoding zoo

Encodings aren't encryption — they're reversible packaging. Spot them by charset and length:

| Encoding | How to spot it | Decode it |
|---|---|---|
| Base64 | `A–Za–z0–9+/`, `=` padding, length %4 == 0 | `base64 -d`, CyberChef |
| Base32 | `A–Z2–7`, `=` padding (often 4 at the end) | CyberChef |
| Hex | `0–9a–f` pairs only | `xxd -r -p`, CyberChef |
| Morse | `.` `-` and spaces | dcode.fr, CyberChef |
| Binary | `01001...` in groups of 8 | CyberChef → *From Binary* |
| URL encoding | `%41%42` / `+` for spaces | CyberChef, browser devtools |

Pro tip: a flag that looks like three nested layers (`NzM3...` → hex → rot13) usually *is* three nested layers. Peel until you hit `flag{`.

## XOR basics: the workhorse

XOR (⊕) is the single most common primitive in CTF crypto. One property matters most: **xoring twice with the same key returns the original data** (`(m ⊕ k) ⊕ k = m`). When the key is a single byte, you don't attack it — you brute-force all 256 possibilities in milliseconds:

```python
# Ciphertext: "flag{xor_is_easy}" xored with one secret byte
ct = bytes.fromhex("242e2325393a2d301d2b311d2723313b3f")

for key in range(256):
    pt = bytes(b ^ key for b in ct)
    # only a real key turns the garbage into printable ASCII
    if all(32 <= c < 127 for c in pt):
        print(f"key = {key:#04x} -> {pt.decode()}")
# key = 0x42 -> flag{xor_is_easy}
```

The printable-ASCII check is the whole trick: random keys produce bytes that are unprintable or nonsense, while the right key jumps out as readable English. (Several keys may pass the printable test — pick the one that reads like a flag.) Extend the same idea to **repeating-key XOR** by attacking one key byte per column, exactly like Vigenère.

## RSA essentials for CTFs

RSA in three lines: pick two primes `p` and `q`, publish `n = p × q` and a public exponent `e`. Your secret `d` satisfies `e·d ≡ 1 (mod (p−1)(q−1))`. Encrypt: `c = mᵉ mod n`. Decrypt with `d`.

That's the textbook. The CTF flavor is always a **misconfiguration**:

| CTF flavor | Why it breaks | The move (concept) |
|---|---|---|
| Small `n` | `n` is only a few hundred bits — factorable | Factor `n` → get `p`, `q` → compute `d`. Factordb / yafu / RsaCtfTool do this |
| Shared prime | Two keys generated with a common factor | `gcd(n₁, n₂)` leaks `p` instantly |
| Small/stereotyped message | `mᵉ` smaller than `n` (or mostly known) | Take the integer `e`-th root — no modular math needed |
| Leaked or tiny `d` | Weak private exponent | Continued-fraction (Wiener-style) attacks — a known, named attack you run from a script |

> **Be precise about the claim:** CTF RSA is deliberately broken — small moduli, reused primes, leaked parameters. Properly generated RSA with modern key sizes remains out of reach; nobody in this community is "breaking RSA," they're breaking *badly configured* RSA. Keep that distinction in your write-ups and you'll sound like an engineer, not a movie.

## A worked example: base64 → hex → ROT13 → flag

You're given this "ciphertext" in a warmup challenge:

```
NzM3OTZlNzQ3YjcxMzM3MDMwNzEzMTYxNzQ1ZjM0Nzk3OTVmNjc3NTMzNWY2YTM0NmM3ZA==
```

Triage: `A–Z a–z 0–9 + /` with `==` padding → Base64. Peel the layers:

```bash
# Layer 1 — Base64
$ echo 'NzM3OTZlNzQ3YjcxMzM3MDMwNzEzMTYxNzQ1ZjM0Nzk3OTVmNjc3NTMzNWY2YTM0NmM3ZA==' | base64 -d
73796e747b71337030713161745f3479795f6775335f6a346c7d

# Layer 2 — that's hex (0-9a-f, even length)
$ echo '73796e747b71337030713161745f3479795f6775335f6a346c7d' | xxd -r -p
synt{q3pq0q1at_4yy_gu3_j4yl}

# Layer 3 — every letter is one step off (s→f, y→l): classic ROT13
$ echo 'synt{q3pq0q1at_4yy_gu3_j4yl}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
flag{d3c0d1ng_4ll_th3_w4y}
```

Flag: `flag{d3c0d1ng_4ll_th3_w4y}`. Total crypto knowledge required: "base64 has `=` padding," "hex uses a-f," "ROT13 is a Caesar with shift 13." The rest was triage — which is why the first table in this article is the most important one.

## Where to learn more

- **[CryptoHack](https://cryptohack.org/)** — verified as a live free platform: interactive challenges and courses (modular arithmetic, symmetric ciphers, public-key crypto, elliptic curves), a scoreboard, and a **CTF Archive** of real competition problems. It's the standard next step after this article.
- **CyberChef** — build decode recipes until the common chains are muscle memory.
- **dcode.fr** — one-click solvers for every classical cipher and encoding.
- **CSIX Wiki: Cryptography Basics** — the concepts behind the attacks: https://siedysoes.github.io/csix-wiki/#/en/fundamentals/cryptography-basics

## See also

- [CTF Categories](../fundamentals/ctf-categories.md) — how crypto difficulty scales on the board
- [Getting Started in CTFs](../fundamentals/getting-started.md) — triage fits the warmup sweep
- [Web Exploitation Challenges](web.md) — the other category for your first event
- [Practice Platforms](../practice/platforms.md) — where to drill after CryptoHack
