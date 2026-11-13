# OSINT & Tantangan Misc

Tantangan OSINT memberi Anda sebuah foto, nama pengguna, atau unggahan dan meminta Anda memulihkan apa yang disembunyikan di dalamnya — sebuah lokasi, akun yang terhubung, atau flag itu sendiri — hanya dengan informasi yang menjadi bagian dari data tantangan. Misc adalah kantong campurannya: encoding, hash, kode QR, dan teka-teki scripting kecil. Keduanya menghargai pengamatan yang teliti alih-alih tool yang mahal.

## Metodologi OSINT untuk CTF

Ekstrak setiap petunjuk dari artefak yang disediakan sebelum mencari apa pun:

| Petunjuk | Di mana melihat | Apa yang Anda dapatkan |
|---|---|---|
| EXIF GPS | `exiftool photo.jpg` | Lintang/bujur → masukkan ke peta |
| Landmark unik | Reverse image search (di bawah) | Kota atau monumen, kadang lokasi flag-nya |
| Bahasa dan aksara papan nama | Baca gambarnya | Negara atau wilayah, yang mempersempit setiap pencarian berikutnya |
| Cuaca, musim, bayangan | Bandingkan dengan timestamp file-nya | Mengonfirmasi atau menolak sebuah tebakan lokasi |
| Pivot nama pengguna | Cari handle-nya di beberapa platform | Handle yang sama di tempat lain menautkan akun-akun yang dituntut tantangan |
| Timestamp | Tanggal unggahan, tanggal file | Membangun linimasa yang ditanyakan sebuah pertanyaan |

Bekerjalah di dalam data milik tantangan itu sendiri: foto, handle, unggahan yang menunjuk Anda pada tugas. Menebak-nebak akun pribadi yang tidak berhubungan bukan bagian dari teka-teki ini.

### Bentuk tantangan OSINT yang umum

| Bentuk | Apa yang Anda berikan | Apa yang harus Anda hasilkan |
|---|---|---|
| Geolokasi | Foto atau tangkapan layar sebuah jalan | Koordinat, kota, atau nama tempat (sering dicetak sebagai flag) |
| Pivot nama pengguna | Sebuah handle dan satu profil | Akun-akun lain yang ditautkan tantangan kepadanya |
| Identifikasi landmark | Sebuah bangunan, patung, atau papan nama | Namanya — periksa apakah flag disimpan di dekat lokasi tersebut |
| Teka-teki linimasa | Beberapa unggahan bertanggal | Urutan peristiwa atau tanggal tertentu |
| Pelacakan sumber | Sebuah gambar, kutipan, atau klaim | Dari mana asalnya di dalam data tantangan |

Alur geolokasi penyelesaian, sepenuhnya pada foto yang disediakan:

```
exiftool photo.jpg | grep -i gps     # coordinates, if the author left them in
# no GPS? reverse image search the photo (TinEye/Yandex/Google)
# read the signage: language and script narrow the country
# shadows and vegetation hint at hemisphere and season — compare with the date
# confirm the candidate spot in a map's street-level view, then note the flag
```

## Google dorking untuk CTF

Dorking adalah membatasi pencarian agar hasilnya berasal dari konteks yang disediakan tantangan:

| Operator | Contoh | Penggunaan |
|---|---|---|
| `site:` | `site:example-ctf.org flag` | Hanya halaman di domain yang disebut tantangan |
| `filetype:` | `filetype:pdf osint` | Hanya tipe file tertentu |
| `inurl:` | `inurl:login example-ctf` | Teks muncul di URL |
| `intitle:` | `intitle:"index of" backup` | Teks muncul di judul halaman |
| `""` (frasa persis) | `"flag{" site:example-ctf.org` | Kata-kata persis, sering dikombinasikan dengan `site:` |

Operator-operator ini menemukan halaman publik yang relevan dengan tantangan. Ini teknik pencarian, bukan cara menyingkap sistem — tetaplah pada materi yang ditunjukkan kepada Anda.

## Reverse image search

Berikan foto tantangan ke salah satu mesin berikut dan bandingkan hasilnya:

| Mesin | Paling bagus untuk | Catatan |
|---|---|---|
| Google Images (Lens) | Objek, logo, adegan umum | Indeks terluas; pilihan default yang baik untuk coba pertama |
| TinEye | Salinan persis dan yang diunggah ulang | Terbaik untuk "di mana lagi gambar ini pernah muncul" |
| Yandex | Tempat, wajah, pemandangan khas | Sering terkuat untuk landmark dan orang; hasil berbeda menurut wilayah |

Mana yang terbaik bergantung pada gambarnya: TinEye untuk asal-usul, Yandex untuk tempat dan wajah, Google untuk sisanya. Unggah hanya gambar milik tantangan.

## Keterampilan misc klasik

| Keterampilan | Tool atau trik | Contoh |
|---|---|---|
| Identifikasi hash | `hashid <hash>` atau hash-identifier | Kenali md5/sha256/bcrypt sebelum menyentuh cracker |
| Rantai encoding | CyberChef | Resep: "From Base64" → "From Hex" → "From ROT13", ulangi sampai terbaca |
| Decode manual | `base64 -d`, `xxd` | `echo "ZmxhZ3t0ZXN0fQ==" \| base64 -d` mencetak `flag{test}` |
| Teka-teki pemrograman | One-liner Python | Implementasikan ulang loop yang dijelaskan tantangan: `python -c "print(''.join(chr(ord(c)-1) for c in 'gmbh'))"` |
| Kode QR | `zbarimg code.png` | Mendecode QR dan barcode langsung dari sebuah gambar |
| Teks tersembunyi | `strings`, view-source | Komentar HTML dan source halaman menyembunyikan flag di tugas misc yang berdekatan dengan web |

Ide CyberChef cepat untuk sebuah blob berbentuk flag: tempelkannya, coba "Decode base64", dan jika outputnya masih terlihat ter-encode, putar melalui berbagai encoding sampai `flag{...}` muncul. Panduan kriptografi memiliki katalog pola yang lengkap.

## Kebiasaan regex dan otomasi

Saat sebuah tugas berkata "temukan setiap string berbentuk flag di log 40 MB ini", berhenti membaca dan tulis sepuluh baris kode:

```python
import re, sys
text = sys.stdin.read()
for match in re.findall(r"flag\{[^}]+\}", text):
    print(match)
```

Kebiasaan yang sama berlaku untuk nama pengguna, koordinat, atau pola berulang apa pun: jika Anda akan melakukannya lebih dari lima kali secara manual, skripkan. Brute-force ruang pola yang disediakan tantangan adalah teknik CTF yang wajar; brute-force apa pun yang lain bukan.

## Etika dan privasi

Informasi pubrik tentang orang sungguhan berbeda dari data tantangan. CTF memberi Anda target yang difiksionalisasi yang datanya milik tugas; nomor telepon, alamat rumah, atau akun pribadi seseorang tidak serta-merta menjadi sah dimainkan hanya karena Anda menemukannya. Hormati privasi, ikuti hukum setempat, dan ingat bahwa keterampilan OSINT membawa tanggung jawab. Kedalaman OSINT repositori ini ada di https://siedysoes.github.io/csix-osint/.

Setiap teknik di sini adalah untuk teka-teki yang diberikan kepada Anda — kompetisi, platform, dan data lab Anda sendiri. Menggunakannya pada hal apa pun yang lain berarti Anda tidak lagi melakukan OSINT.

## Lihat juga

- [Tantangan Kriptografi](crypto.md)
- [Tantangan Forensik](forensics.md)
- [Kategori CTF](../fundamentals/ctf-categories.md)
- [Platform Latihan](../practice/platforms.md)