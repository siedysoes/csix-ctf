# Tantangan Forensik

Tantangan forensik menyerahkan kepada Anda sebuah artefak — citra disk, foto, PCAP, atau memory dump — dan meminta Anda menggali flag darinya. Ini penting karena respons insiden sungguhan justru dimulai dengan cara ini: identifikasi apa yang Anda pegang, triase secara murah, lalu selami bagian yang terlihat menarik. Bekerjalah hanya pada file yang diberikan kepada Anda oleh acara atau ditangkap di lab Anda sendiri.

## Triase forensik

```
file mystery.img                      # what am I actually holding?
strings mystery.img | grep -i flag    # cheap first pass for literal flags
binwalk mystery.img                   # anything embedded inside?
exiftool photo.jpg                    # metadata: GPS, camera, comments
# carve out what binwalk found (foremost/testdisk), then go deeper on each piece
```

Jalankan perintah murahnya secara berurutan. Sebagian besar flag pemula muncul di langkah 2 atau 4.

## Mengidentifikasi file

| Perintah | Apa yang diberitahukannya |
|---|---|
| `file mystery.bin` | Tipe dari magic bytes: ELF, PNG, ZIP, PDF, PCAP — ekstensinya bisa berbohong |
| `xxd mystery.bin \| head` (atau `head -c 64 mystery.bin \| xxd`) | Byte header mentah; mengonfirmasi apa yang diklaim `file` |
| `binwalk mystery.bin` | File dan arsip yang tertanam, beserta offset-nya |
| `foremost mystery.bin` / `testdisk` | Mengukir kembali file yang tertanam (verifikasi flag-nya untuk platform Anda) |
| `unzip -l file.zip` | Mendaftar isi arsip tanpa mengekstrak |

Sebuah `.png` yang dilaporkan `file` sebagai "Zip archive data" adalah kasus klasik: flag-nya ada di dalam, bukan di gambarnya.

## Berburu metadata

`exiftool photo.jpg` mencetak segala sesuatu yang dibawa file itu. Field yang layak dibaca terlebih dahulu:

| Field | Di mana muncul | Apa yang bisa diungkapkannya |
|---|---|---|
| `GPSLatitude` / `GPSLongitude` | Foto dari ponsel dan kamera | Titik lokasi pengambilan foto (sering dalam derajat/menit/detik — konversi ke desimal untuk peta) |
| `Make` / `Model` | EXIF kamera dan ponsel | Identitas perangkat, kadang sebuah petunjuk penulis |
| `Software` | File yang disimpan ulang oleh editor | Alat mana yang menyentuhnya — dan kadang sebuah nama akun |
| `Comment` / `ImageDescription` | Banyak format | Flag yang ditempel ke komentar |
| `DateTimeOriginal` | Foto | Mempersempit "kapan", berguna untuk teka-teki linimasa |
| `Artist` / `Copyright` | File media | Field penulis, nama pengguna |

## Steganografi

| Alat | Bekerja pada | Catatan |
|---|---|---|
| `steghide` | JPEG, BMP, WAV, AU | `steghide extract -sf image.jpg` — password sering tertulis di deskripsi tantangan |
| `zsteg` | PNG, BMP | Memeriksa LSB dan bit plane lain: `zsteg image.png` (verifikasi instalasi Anda) |
| LSB (least significant bit) | Format gambar pada umumnya | Konsep: setiap kanal piksel menyimpan sebuah angka; menyembunyikan data di bit terendah mengubah gambar secara tak terlihat. Skrip dan CyberChef bisa membacanya kembali |
| `outguess` | JPEG | Ekstrak dengan kunci yang diketahui: `outguess -r -k "password" image.jpg out.txt` (verifikasi flag-nya sebelum dipakai) |

Tantangan steg jarang mengumumkan dirinya — jika sebuah gambar terlihat normal dan `strings` tidak menemukan apa pun, coba tool steg sebelum menyimpulkan tidak ada apa pun di sana.

## Forensik jaringan (PCAP)

Buka tangkapan yang Anda berikan di Wireshark dan kerjakan dari atas ke bawah:

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

One-liner setara saat Anda lebih suka terminal (tshark ikut dikirim bersama Wireshark):

```bash
tshark -r chall.pcap -Y "http.request"
tshark -r chall.pcap --export-objects http,./exported
```

Contoh penyelesaian: filter `http.request`, temukan sebuah POST ke `/login`, ikuti TCP stream-nya untuk membaca data form, lalu ekspor objek untuk mengambil gambar yang dimuat halaman itu — flag sering berada di salah satu dari tiga tempat tersebut.

## Pengantar forensik memori

Memory dump menyimpan apa yang tidak bisa disimpan citra disk: proses yang berjalan, koneksi terbuka, materi yang sudah didekripsi. Volatility adalah tool standarnya; alur klasik Volatility 2 terlihat seperti ini:

```
volatility -f memdump.raw imageinfo                    # identify the profile
volatility -f memdump.raw --profile=Win7SP1x64 pslist  # process list
volatility -f memdump.raw --profile=Win7SP1x64 netscan # network connections
```

Volatility 3 memakai gaya perintah yang berbeda (`vol -f memdump.raw windows.info`) — cocokkan sintaksnya dengan versi yang Anda pasang. Mulailah dengan ringan: daftar proses, koneksi, lalu strings pada apa pun yang mencurigakan.

## Trik arsip dan pengemasan

- **Baca deskripsinya.** Password arsip sangat sering ditulis di sana — "trik"-nya adalah memperhatikan, bukan menebak.
- **Ekstensi yang salah.** Jalankan `file` sebelum Anda membuka apa pun; arsip atau executable yang diganti nama adalah hal umum.
- **Lapisan bersarang.** Satu arsip di dalam arsip lain di dalam sebuah PNG; triase memberi tahu Anda kapan harus terus membongkar.
- **Komentar dan flag yang terpecah.** Periksa field komentar arsip, dan ingat flag-nya bisa tersebar di beberapa file yang masing-masing menyimpan satu fragmen.

## Encoding misc klasik

Banyak flag forensik hanyalah ter-encode — Base64, hex, ROT13, atau rantainya. Pola, cipher yang umum, dan cara mengenalinya dibahas di [Tantangan Kriptografi](crypto.md).

Forensik sepenuhnya tentang artefak yang diserahkan kepada Anda: PCAP Anda, gambar-gambar Anda, mesin lab Anda. Tidak ada apa pun di artikel ini yang berlaku untuk data milik orang lain — memanen informasi pribadi dari orang atau sistem sungguhan bukanlah latihan forensik.

## Lihat juga

- [Tantangan Kriptografi](crypto.md)
- [OSINT & Tantangan Misc](misc-osint.md)
- [Platform Latihan](../practice/platforms.md)
- [Toolkit CTF](../practice/toolkit.md)