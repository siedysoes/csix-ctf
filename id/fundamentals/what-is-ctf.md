# Apa Itu CTF

Capture The Flag (CTF) adalah kompetisi keamanan siber di mana penyelenggara memberi Anda sekumpulan teka-teki — sebuah situs web, pesan terenkripsi, sebuah program biner, atau sebuah citra disk — dan tugas Anda adalah menggali string tersembunyi di dalamnya. String itulah yang disebut **flag**. Kirimkan flag ke server penilaian, dan tim Anda mendapat poin. Keseluruhan permainan adalah perlombaan: selesaikan sebanyak mungkin tantangan yang diberikan sebelum waktu habis.

Segala sesuatu yang Anda serang di CTF memang dibuat untuk diserang. Situs web itu *memang* untuk dipecahkan, sandi itu *memang* untuk dibobol. Hal inilah yang membuat CTF menjadi salah satu cara teraman dan tercepat untuk berlatih keterampilan keamanan nyata.

## Flag: mata uang universal

Setiap tantangan menyembunyikan sebuah flag, dan setiap flag mengikuti pola yang sama: awalan yang mudah dikenali, kurung kurawal, dan sebuah pesan di dalamnya.

```
flag{some_hidden_string_here}
CTF{another_format}
picoCTF{yet_another_prefix}
```

**Format flag berbeda tiap acara.** Satu kompetisi memakai `flag{...}`, kompetisi berikutnya memakai `CTF{...}`, yang lain memakai awalan bermerek seperti `picoCTF{...}`. Selalu periksa halaman aturan acara atau deskripsi tantangan untuk mengetahui format yang tepat. Flag biasanya bersifat case-sensitive dan harus dikirim persis seperti saat ditemukan — `Flag{...}` tidak sama dengan `flag{...}`.

Isi di dalam flag adalah hadiah atas pemecahan teka-teki tersebut: bisa berupa frasa (`flag{read_the_source}`), string heksadesimal, atau petunjuk tentang apa yang baru saja Anda pelajari.

## Asal-usul CTF

CTF berakar dari permainan peretasan "Capture the Flag" yang digelar di DEF CON, konvensi keamanan yang berjalan sejak 1993, di mana para tim berlomba menemukan flag yang tersembunyi di jaringan tim lawan sambil mempertahankan jaringan mereka sendiri. Dari akar itu, format ini bercabang menjadi dua gaya utama yang akan Anda temui hari ini: **jeopardy** (selesaikan teka-teki independen untuk mendapat poin — dibahas di [Format & Skor](competition-formats.md)) dan **attack-defense** (bobol layanan langsung milik tim lain sambil menjaga layanan Anda tetap hidup).

## Mengapa CTF adalah salah satu cara terbaik belajar keamanan

- **Taman bermain yang aman dan legal.** Anda mendapat serangan nyata — injeksi SQL, korupsi memori, membobol sandi — tanpa risiko hukum apa pun, karena target memang ada agar Anda menjebolnya. Tidak ada izin yang harus dinegosiasikan, tidak ada risiko kerusakan sampingan.
- **Tekanan yang membangun keterampilan.** Papan peringkat, jam, dan rekan setim menciptakan urgensi terfokus yang tidak pernah bisa ditiru tutorial. Anda belajar mengambil keputusan dengan informasi yang belum lengkap — persis seperti pekerjaan sebenarnya.
- **Wawasan luas tanpa komitmen.** Satu acara memperkenalkan Anda pada web, crypto, reverse engineering, forensics, dan OSINT dalam satu akhir pekan, sehingga Anda menemukan apa yang sebenarnya Anda kuasai sebelum menekuni satu bidang.
- **Umpan balik instan.** Flag itu benar atau salah. Tidak ada nilai parsial untuk penjelasan panjang — Anda langsung tahu apakah model mental Anda tentang sistem itu sudah benar.

## Siapa yang bermain CTF

- **Mahasiswa** — jalur masuk klasik; klub kampus menggelar acara latihan, dan CTF pemula dirancang untuk orang dengan pengetahuan keamanan satu semester atau kurang.
- **Profesional yang bekerja** — red-teamer, blue-teamer, pentester, dan pengembang mengasah keterampilan tertentu dan membuktikannya di papan peringkat publik.
- **Pengalih karier** — beberapa hasil CTF plus write-up publik adalah portofolio nyata yang berarti "saya benar-benar bisa melakukan ini," dan sering kali lebih bergaring daripada sertifikat.
- **Hobiis dan pengajar** — orang-orang yang menikmati teka-teki dan menjaga komunitas tetap hidup.

Anda tidak perlu jenius; Anda hanya perlu penasaran dan mau mencari informasi di bawah tekanan waktu.

## Apa yang sebenarnya Anda pelajari

| Keterampilan | Dari mana asalnya di CTF |
|---|---|
| Web exploitation | Tantangan web: login yang bisa ditembus, injeksi, parameter yang dimanipulasi |
| Kriptanalisis | Tantangan crypto: sandi yang bobol, konfigurasi RSA yang salah |
| Reverse engineering | Tantangan rev: membongkar biner untuk menemukan pemeriksaan rahasia |
| Eksploitasi biner | Tantangan pwn: korupsi memori pada program yang disediakan |
| Forensics & analisis | Tantangan berkas, PCAP, dan memory dump ("apa yang terjadi di sini?") |
| OSINT & riset | Tantangan OSINT: menemukan informasi yang bocor secara publik dari seseorang atau perusahaan |
| Scripting & otomasi | Hampir semua kategori — Python untuk decode, fuzz, atau brute-force bagi Anda |
| Komunikasi | Write-up: menjelaskan solusi Anda dengan cukup jelas agar orang lain bisa mengulanginya |
| Kerja sama tim & manajemen waktu | Jam dan papan peringkat bersama memaksa Anda membagi tugas dan menaklukkan bersama |

## Di mana CTF digelar

| Acara / sumber | Tingkat | Isinya |
|---|---|---|
| picoCTF (kini di CyLab Security Academy Carnegie Mellon) | Pemula | CTF perdananya klasik — kompetisi belajar berbasis teka-teki gratis dari CMU, ideal untuk flag pertama Anda |
| PlaidCTF | Kelas atas | Acara tahunan bergaya jeopardy yang dijalankan Plaid Parliament of Pwning (PPP), salah satu tim terkuat di dunia |
| DEF CON CTF | Kelas atas | CTF unggulan yang terkait dengan DEF CON, secara historis attack-defense, tempat tim terbaik dunia bertemu di babak final |
| [CTFtime](https://ctftime.org/) | Semua tingkat | Bukan acara — kalender komunitas: acara mendatang dan lampau, peringkat tim, daftar tantangan, serta arsip besar write-up publik |

Jadwal, format, dan platform bisa berubah sewaktu-waktu — verifikasi detail terkini di situs resmi setiap acara sebelum Anda mendaftar.

## Flag pertama Anda: panduan langkah demi langkah

Bayangkan acara perdana Anda. Papan terbuka, Anda memandang 40 judul tantangan, dan yang paling bawah berbunyi:

> **Warmup: Apa isi berkas ini?** (50 poin, forensics)
> Magang kami meninggalkan berkas ini: `mystery`. Cari tahu apa yang dia sembunyikan.

Berikut keseluruhan penyelesaiannya, dari awal sampai akhir:

```bash
# 1. What even is this file?
$ file mystery
mystery: ELF 64-bit LSB executable, x86-64

# 2. It's a program — but maybe the string is just sitting in the data
$ strings mystery | grep -i flag
[...]
flag{r3vers1ng_1s_fun}
```

Anda menempelkan `flag{r3vers1ng_1s_fun}` ke kotak pengiriman (memeriksa awalan dan kapitalisasinya terlebih dahulu), menekan kirim, dan papan berubah hijau: **+50 poin**. Tidak ada apa pun di mesin Anda yang rusak, tidak ada yang ilegal terjadi, dan Anda baru saja memakai teknik reverse-engineering sungguhan — trik `strings` yang sama yang dipakai profesional untuk penelusuran awal pada biner yang tidak dikenal.

Itulah siklus setiap CTF: baca deskripsi, susun hipotesis, coba sebuah alat, sesuaikan, ulangi — sampai flag muncul.

## Aturan yang tidak pernah melentur: etika

> ⚠️ **Hanya tantangan yang diberikan kepada Anda.** Segala sesuatu di basis belajar ini tentang memecahkan teka-teki yang disediakan untuk Anda — di kompetisi, di platform latihan, dan di lab Anda sendiri. Jangan pernah memakai teknik ini terhadap sistem yang tidak Anda miliki atau yang tidak Anda izinkan secara eksplisit untuk diuji. Di luar cakupan tantangan, tindakan yang sama hanyalah akses tanpa izin — flag-nya tidak ada di sana, dan konsekuensinya nyata.

## Lihat juga

- [Kategori CTF](ctf-categories.md) — teka-teki seperti apa yang cocok untuk Anda
- [Format & Skor](competition-formats.md) — bagaimana acara dijalankan dan dinilai
- [Memulai di CTF](getting-started.md) — panduan untuk acara pertama Anda
- [Toolkit CTF](../practice/toolkit.md) — alat yang dipakai dalam panduan di atas
