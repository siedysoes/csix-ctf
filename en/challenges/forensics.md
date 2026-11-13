# Forensics Challenges

Forensics challenges hand you an artifact — a disk image, a photo, a PCAP, a memory dump — and ask you to dig the flag out of it. It matters because real incident response starts exactly this way: identify what you have, triage cheaply, then go deep on the piece that looks interesting. Work only on files given to you by the event or captured in your own lab.

## The forensics triage

```
file mystery.img                      # what am I actually holding?
strings mystery.img | grep -i flag    # cheap first pass for literal flags
binwalk mystery.img                   # anything embedded inside?
exiftool photo.jpg                    # metadata: GPS, camera, comments
# carve out what binwalk found (foremost/testdisk), then go deeper on each piece
```

Run the cheap commands in order. Most beginner flags appear in step 2 or 4.

## Identifying the file

| Command | What it tells you |
|---|---|
| `file mystery.bin` | Type from the magic bytes: ELF, PNG, ZIP, PDF, PCAP — the extension may lie |
| `xxd mystery.bin \| head` (or `head -c 64 mystery.bin \| xxd`) | The raw header bytes; confirms what `file` claims |
| `binwalk mystery.bin` | Embedded files and archives, with their offsets |
| `foremost mystery.bin` / `testdisk` | Carves embedded files back out (verify the flags for your platform) |
| `unzip -l file.zip` | Lists archive contents without extracting |

A `.png` that `file` reports as "Zip archive data" is a classic: the flag is inside, not in the image.

## Metadata hunting

`exiftool photo.jpg` prints everything the file carries. Fields worth reading first:

| Field | Where it shows up | What it may reveal |
|---|---|---|
| `GPSLatitude` / `GPSLongitude` | Photos from phones and cameras | The spot the shot was taken (often given in degrees/minutes/seconds — convert to decimal for a map) |
| `Make` / `Model` | Camera and phone EXIF | Device identity, sometimes an author hint |
| `Software` | Files re-saved by an editor | Which tool touched it — and sometimes an account name |
| `Comment` / `ImageDescription` | Many formats | Flags pasted into comments |
| `DateTimeOriginal` | Photos | Narrows "when", useful for timeline puzzles |
| `Artist` / `Copyright` | Media files | Author fields, usernames |

## Steganography

| Tool | Works on | Notes |
|---|---|---|
| `steghide` | JPEG, BMP, WAV, AU | `steghide extract -sf image.jpg` — password often sits in the challenge description |
| `zsteg` | PNG, BMP | Checks LSB and other bit planes: `zsteg image.png` (verify your install) |
| LSB (least significant bit) | Image formats generally | Concept: each pixel channel stores a number; hiding data in the lowest bit changes the image imperceptibly. Scripts and CyberChef can read it back |
| `outguess` | JPEG | Extract with a known key: `outguess -r -k "password" image.jpg out.txt` (verify flags before use) |

Steg challenges rarely announce themselves — if an image looks normal and `strings` found nothing, try a steg tool before assuming there is nothing there.

## Network forensics (PCAP)

Open the capture you were given in Wireshark and work top-down:

```
# 1. the big picture
Statistics > Conversations        # who talked to whom, how much data
# 2. find web traffic
filter bar: http.request          # every HTTP request
filter bar: http.request.method == "GET"    # a specific method
filter bar: tcp.stream == 5       # one conversation, after noting its stream number
# 3. read it in context
right-click a packet > Follow > TCP Stream
# 4. pull files out
File > Export Objects > HTTP
```

Equivalent one-liners when you prefer the terminal (tshark ships with Wireshark):

```bash
tshark -r chall.pcap -Y "http.request"
tshark -r chall.pcap --export-objects http,./exported
```

A worked example: filter `http.request`, spot a POST to `/login`, follow the TCP stream to read the form data, then export objects to grab the image the page loaded — flags often live in one of those three places.

## Memory forensics intro

Memory dumps hold what disk images cannot: running processes, open connections, decrypted material. Volatility is the standard tool; the classic Volatility 2 flow looks like this:

```
volatility -f memdump.raw imageinfo                    # identify the profile
volatility -f memdump.raw --profile=Win7SP1x64 pslist  # process list
volatility -f memdump.raw --profile=Win7SP1x64 netscan # network connections
```

Volatility 3 uses a different command style (`vol -f memdump.raw windows.info`) — check the syntax against the version you installed. Keep it light at first: process list, connections, then strings on anything suspicious.

## Archive and packaging tricks

- **Read the description.** Archive passwords are very often written there — the "trick" is paying attention, not guessing.
- **Wrong extensions.** `file` before you open anything; a renamed archive or executable is common.
- **Nested layers.** One archive inside another inside a PNG; triage tells you when to keep unpacking.
- **Comments and split flags.** Check archive comment fields, and remember the flag may be spread across several files that each hold a fragment.

## Classic misc encodings

Many forensics flags are simply encoded — Base64, hex, ROT13, or a chain of them. The patterns, common ciphers, and how to spot them are covered in [Cryptography Challenges](crypto.md).

Forensics is entirely about artifacts you were handed: your PCAP, your images, your lab machine. Nothing in this article applies to anyone else's data — harvesting private information from real people or systems is not a forensics exercise.

## See also

- [Cryptography Challenges](crypto.md)
- [OSINT & Misc Challenges](misc-osint.md)
- [Practice Platforms](../practice/platforms.md)
- [The CTF Toolkit](../practice/toolkit.md)
