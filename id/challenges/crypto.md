# Tantangan Kriptografi

Tantangan kripto adalah teka-teki tentang *setup yang rusak*, bukan matematika yang rusak. Anda tidak akan pernah diminta menyerang AES, keluarga SHA-255, atau teorema bilangan prima itu sendiri — kriptografi sungguhan, yang diimplementasikan dengan benar, tidak bisa dipatahkan di laptop Anda. Yang diberikan CTF kepada Anda adalah 90% sisanya dari kenyataan: encoding yang bukan enkripsi, cipher yang dipakai dengan kunci lemah, atau kunci RSA yang dihasilkan dengan kesalahan. Tugas Anda adalah mengenali mana yang sedang Anda pegang.

> ⚠️ **Hanya target yang berwenang.** Semuanya di sini berlaku untuk data tantangan yang diserahkan kepada Anda oleh kompetisi dan platform latihan. Mematahkan kripto pada sistem yang tidak Anda miliki atau yang tidak Anda punya izin untuk diuji bukanlah tantangan — itu kejahatan.

## Triase pertama: lihat datanya dulu sebelum berpikir

Sebagian besar tantangan kripto yang "kelihatan sulit" adalah encoding yang menyamar. Baca ciphertext-nya dengan mata Anda terlebih dahulu:

| Yang Anda lihat | Kemungkinan besar... | Alat pertama |
|---|---|---|
| `A–Z a–z 0–9 + /` dengan padding `=`, panjang kelipatan 4 | Base64 | CyberChef → *From Base64*; `base64 -d` |
| Hanya `0–9 a–f`, panjang genap | Hex | CyberChef; `xxd -r -p` |
| `A–Z 2–7` dengan padding `=` (sering diakhiri `====`) | Base32 | CyberChef → *From Base32* |
| Titik, strip, spasi | Kode Morse | dcode.fr → Morse |
| Hanya `A–Z`, digeser seragam (gkg... terlihat "janggal") | Caesar / ROT13 | CyberChef ROT13; brute-force semua 25 pergeseran |
| `A–Z` dengan pergeseran *tidak seragam* | Vigenère | dcode.fr → Vigenère |
| Byte yang terlihat acak, tanpa charset | XOR atau cipher modern dengan kelemahan yang bocor | Brute-force (lihat di bawah) |

**[CyberChef](https://gchq.github.io/CyberChef/)** (berbasis browser, tumpukan resep drag-and-drop) dan **[dcode.fr](https://www.dcode.fr/)** (solver per-cipher) bersama-sama menangani sebagian besar kripto pemula. Saat Anda melihat string yang tidak bisa diklasifikasikan, decode lapisan yang *Anda* kenali — rantai adalah kebiasaan, bukan pengecualian.

## Cipher klasik

Ini muncul pada tingkat kesulitan warmup hingga menengah dan bisa dipatahkan tanpa latar belakang kripto apa pun:

| Cipher | Apa itu | Serangan |
|---|---|---|
| **Caesar / ROT** | Setiap huruf digeser sejumlah tetap | Brute-force semua 25 pergeseran dan periksa outputnya secara visual; frequency analysis jika teksnya panjang |
| **Vigenère** | Kunci berulang menggeser huruf yang berbeda secara berbeda | 1. Tentukan **panjang** kunci: pemeriksaan Kasiski (urutan berulang) atau index of coincidence. 2. Perlakukan setiap kolom sebagai Caesar tersendiri dan pecahkan dengan frequency analysis |
| **Substitution** | Satu huruf selalu dipetakan ke huruf lain | Frequency analysis (E/T/A paling umum dalam bahasa Inggris) + kata pola (`the`, `and`) + solver seperti dcode.fr atau *Substitution* di CyberChef |

Keterampilan dasarnya sama untuk ketiganya: **bandingkan apa yang Anda punya dengan seperti apa bahasa Inggris (atau bahasa plaintext apa pun) secara statistik.** Insting itu langsung terbawa ke tantangan yang lebih sulit.

## Kebun binatang encoding

Encoding bukan enkripsi — itu kemasan yang bisa dibalik. Kenali dari charset dan panjangnya:

| Encoding | Cara mengenalinya | Cara decode |
|---|---|---|
| Base64 | `A–Za–z0–9+/`, padding `=`, panjang %4 == 0 | `base64 -d`, CyberChef |
| Base32 | `A–Z2–7`, padding `=` (sering 4 di akhir) | CyberChef |
| Hex | Hanya pasangan `0–9a–f` | `xxd -r -p`, CyberChef |
| Morse | `.`, `-`, dan spasi | dcode.fr, CyberChef |
| Binary | `01001...` dalam kelompok 8 | CyberChef → *From Binary* |
| URL encoding | `%41%42` / `+` untuk spasi | CyberChef, devtools browser |

Tips pro: flag yang terlihat seperti tiga lapisan bersarang (`NzM3...` → hex → rot13) biasanya *memang* tiga lapisan bersarang. Kupas sampai Anda menemukan `flag{`.

## Dasar XOR: senjata utama

XOR (⊕) adalah primitif paling umum dalam kripto CTF. Satu sifat yang paling penting: **melakukan XOR dua kali dengan kunci yang sama mengembalikan data aslinya** (`(m ⊕ k) ⊕ k = m`). Saat kuncinya hanya satu byte, Anda tidak menyerangnya — Anda brute-force semua 256 kemungkinan dalam hitungan milidetik:

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

Pemeriksaan printable-ASCII adalah seluruh triknya: kunci acak menghasilkan byte yang tidak bisa dicetak atau tidak bermakna, sementara kunci yang tepat melompat keluar sebagai bahasa Inggris yang terbaca. (Beberapa kunci mungkin lulus uji printable — pilih yang terbaca seperti flag.) Perluas idema yang sama ke **repeating-key XOR** dengan menyerang satu byte kunci per kolom, persis seperti Vigenère.

## Inti RSA untuk CTF

RSA dalam tiga baris: pilih dua prima `p` dan `q`, publikasikan `n = p × q` dan eksponen publik `e`. Rahasia Anda `d` memenuhi `e·d ≡ 1 (mod (p−1)(q−1))`. Enkripsi: `c = mᵉ mod n`. Dekripsi dengan `d`.

Itu teks bukunya. Rasa CTF-nya selalu berupa **miskonfigurasi**:

| Rasa CTF | Mengapa ini rusak | Langkahnya (konsep) |
|---|---|---|
| `n` kecil | `n` hanya beberapa ratus bit — bisa difaktorkan | Faktorkan `n` → dapatkan `p`, `q` → hitung `d`. Factordb / yafu / RsaCtfTool melakukan ini |
| Prima bersama | Dua kunci dihasilkan dengan faktor sama | `gcd(n₁, n₂)` membocorkan `p` seketika |
| Pesan kecil/stereotip | `mᵉ` lebih kecil dari `n` (atau sebagian besar diketahui) | Ambil akar bulat pangkat `e` — tanpa matematika modular |
| `d` bocor atau terlalu kecil | Eksponen privat lemah | Serangan pecahan lanjutan (gaya Wiener) — serangan yang sudah dikenal bernama, dijalankan dari skrip |

> **Waspadai klaimnya:** RSA di CTF sengaja dibuat rusak — modulus kecil, prima dipakai ulang, parameter bocor. RSA yang dihasilkan dengan benar dan ukuran kunci modern tetap di luar jangkauan; tidak ada orang di komunitas ini yang "mematahkan RSA" — mereka mematahkan RSA yang *buruk konfigurasinya*. Pertahankan perbedaan itu di write-up Anda dan Anda akan terdengar seperti seorang insinyur, bukan film.

## Contoh penyelesaian: base64 → hex → ROT13 → flag

Anda diberikan "ciphertext" berikut dalam sebuah tantangan warmup:

```
NzM3OTZlNzQ3YjcxMzM3MDMwNzEzMTYxNzQ1ZjM0Nzk3OTVmNjc3NTMzNWY2YTM0NmM3ZA==
```

Triase: `A–Z a–z 0–9 + /` dengan padding `==` → Base64. Kupas lapisannya:

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

Flag: `flag{d3c0d1ng_4ll_th3_w4y}`. Total pengetahuan kripto yang dibutuhkan: "base64 punya padding `=`", "hex memakai a-f", "ROT13 adalah Caesar dengan pergeseran 13." Sisanya adalah triase — itulah mengapa tabel pertama di artikel ini adalah yang paling penting.

## Ke mana belajar lebih lanjut

- **[CryptoHack](https://cryptohack.org/)** — diverifikasi sebagai platform gratis yang masih berjalan: tantangan dan kursus interaktif (aritmetika modular, cipher simetris, kriptografi public-key, kurva eliptik), papan skor, dan **CTF Archive** berisi soal kompetisi sungguhan. Ini langkah standar setelah artikel ini.
- **CyberChef** — bangun resep decode sampai rantai yang umum jadi ingatan otot.
- **dcode.fr** — solver sekali klik untuk setiap cipher dan encoding klasik.
- **CSIX Wiki: Dasar Kriptografi** — konsep di balik serangannya: https://siedysoes.github.io/csix-wiki/#/en/fundamentals/cryptography-basics

## Lihat juga

- [Kategori CTF](../fundamentals/ctf-categories.md) — bagaimana skala kesulitan kripto di papan skor
- [Memulai di CTF](../fundamentals/getting-started.md) — triase cocok untuk sapuan warmup
- [Tantangan Web Exploitation](web.md) — kategori lain untuk acara pertama Anda
- [Platform Latihan](../practice/platforms.md) — di mana berlatih setelah CryptoHack