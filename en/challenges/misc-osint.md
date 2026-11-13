# OSINT & Misc Challenges

OSINT challenges give you a photo, a username, or a post and ask you to recover what is hidden in it — a location, a linked account, or the flag itself — using only information that belongs to the challenge data. Misc is the grab bag around it: encodings, hashes, QR codes, and small scripting puzzles. Both reward careful observation over expensive tooling.

## OSINT methodology for CTFs

Extract every clue from the provided artifact before searching for anything:

| Clue | Where to look | What it gives you |
|---|---|---|
| EXIF GPS | `exiftool photo.jpg` | Latitude/longitude → drop into a map |
| Unique landmarks | Reverse image search (below) | City or monument, sometimes the flag's location |
| Signage language and script | Read the image | Country or region, which narrows every later search |
| Weather, season, shadows | Compare against the file's timestamp | Confirms or rejects a location guess |
| Username pivoting | Search the handle on several platforms | The same handle elsewhere links the accounts the challenge wants |
| Timestamps | Post dates, file dates | Builds the timeline a question asks about |

Work inside the challenge's own data: the photo, the handle, the posts the task points you at. Guessing at unrelated private accounts is not part of the puzzle.

### Common OSINT challenge shapes

| Shape | What you are given | What you must produce |
|---|---|---|
| Geolocation | A photo or a screenshot of a street | Coordinates, city, or a place name (often printed as the flag) |
| Username pivot | A handle and one profile | The other accounts the challenge links to it |
| Landmark ID | A building, statue, or sign | Its name — check the flag is stored near that location |
| Timeline puzzle | Several dated posts | An order of events or a specific date |
| Source tracing | An image, quote, or claim | Where it originally came from within the challenge's data |

A worked geolocation flow, entirely on the provided photo:

```
exiftool photo.jpg | grep -i gps     # coordinates, if the author left them in
# no GPS? reverse image search the photo (TinEye/Yandex/Google)
# read the signage: language and script narrow the country
# shadows and vegetation hint at hemisphere and season — compare with the date
# confirm the candidate spot in a map's street-level view, then note the flag
```

## Google dorking for CTF

Dorking is restricting a search so results come from the context the challenge provides:

| Operator | Example | Use |
|---|---|---|
| `site:` | `site:example-ctf.org flag` | Only pages on a domain the challenge names |
| `filetype:` | `filetype:pdf osint` | Only a given file type |
| `inurl:` | `inurl:login example-ctf` | Text appears in the URL |
| `intitle:` | `intitle:"index of" backup` | Text appears in the page title |
| `""` (exact phrase) | `"flag{" site:example-ctf.org` | Exact wording, often combined with `site:` |

These operators find public pages relevant to the challenge. They are a search technique, not a way to probe systems — stick to the material you were pointed at.

## Reverse image search

Give the challenge photo to one of these and compare results:

| Engine | Best at | Notes |
|---|---|---|
| Google Images (Lens) | Objects, logos, general scenes | Widest index; good default first try |
| TinEye | Exact and re-uploaded copies | Best for "where else has this image appeared" |
| Yandex | Places, faces, distinctive scenery | Often strongest for landmarks and people; results vary by region |

Which works best depends on the image: TinEye for provenance, Yandex for places and faces, Google for everything else. Upload only the challenge's image.

## Classic misc skills

| Skill | Tool or trick | Example |
|---|---|---|
| Hash identification | `hashid <hash>` or hash-identifier | Recognize md5/sha256/bcrypt before touching a cracker |
| Encoding chains | CyberChef | Recipe: "From Base64" → "From Hex" → "From ROT13", repeat until readable |
| Decoding by hand | `base64 -d`, `xxd` | `echo "ZmxhZ3t0ZXN0fQ==" \| base64 -d` prints `flag{test}` |
| Programming puzzles | Python one-liners | Reimplement the loop the challenge describes: `python -c "print(''.join(chr(ord(c)-1) for c in 'gmbh'))"` |
| QR codes | `zbarimg code.png` | Decodes QR and barcodes straight from an image |
| Hidden text | `strings`, view-source | HTML comments and page source hide flags in web-adjacent misc tasks |

A quick CyberChef idea for a flag-shaped blob: paste it, try "Decode base64", and if the output still looks encoded, rotate through encodings until `flag{...}` appears. The cryptography guide has the full pattern catalogue.

## Regex and automation habit

When a task says "find every flag-shaped string in this 40 MB log", stop reading and write ten lines of code:

```python
import re, sys
text = sys.stdin.read()
for match in re.findall(r"flag\{[^}]+\}", text):
    print(match)
```

The same habit applies to usernames, coordinates, or any repeated pattern: if you would do it more than five times by hand, script it. Brute-forcing a challenge-provided pattern space is a normal CTF technique; brute-forcing anything else is not.

## Ethics and privacy

Public information about real people is different from challenge data. A CTF gives you a fictionalized target whose data belongs to the task; a real person's phone number, home address, or private accounts do not become fair game because you found them. Respect privacy, follow local laws, and remember that OSINT skills carry responsibility. This repository's OSINT depth lives at https://siedysoes.github.io/csix-osint/.

Every technique here is for puzzles given to you — competitions, platforms, and your own lab data. Use them on anything else and you are no longer doing OSINT.

## See also

- [Cryptography Challenges](crypto.md)
- [Forensics Challenges](forensics.md)
- [CTF Categories](../fundamentals/ctf-categories.md)
- [Practice Platforms](../practice/platforms.md)
