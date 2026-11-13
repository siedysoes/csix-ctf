# Kategori CTF

Tantangan CTF dikelompokkan ke dalam kategori-kategori, dan setiap kategori adalah varian teka-teki yang berbeda, dengan alat dan cara berpikirnya sendiri. Anda tidak perlu menguasai semuanya untuk memulai — Anda hanya butuh satu yang Anda nikmati, plus pengetahuan yang cukup tentang sisanya untuk tahu kapan harus minta bantuan rekan setim.

## Sekilas tentang kategori-kategori

| Kategori | Yang Anda dapat | Yang Anda pecahkan | Keterampilan kunci |
|---|---|---|---|
| **Web** | Aplikasi HTML/JS (dan sering kali kodenya) | Temukan cacat pada formulir login, sebuah parameter, atau logika aplikasi | Cara web bekerja sesungguhnya — HTTP, sesi, injeksi |
| **Crypto** | Sebuah sandi, hash, atau pesan terenkripsi | Bobol *implementasi atau konfigurasinya*, bukan matematikanya sendiri | Pengenalan pola + aritmetika modular |
| **Rev** | Biner yang sudah dikompilasi (ELF/EXE) | Bongkar program sampai Anda memahami pemeriksaan rahasianya | Membaca disassembly, memakai debugger |
| **Pwn** | Biner yang rentan, terkadang dengan layanan langsung | Korupsikan memorinya agar melakukan apa yang Anda inginkan | Tata letak stack/heap, pengembangan exploit |
| **Forensics** | Berkas, citra disk, PCAP, memory dump | Rekonstruksi apa yang terjadi dari jejak-jejak yang tersisa | Perkakas: carving, sniffing, analisis linimasa |
| **Misc** | Apa saja: gambar, audio, skrip, data | Teka-teki stego, encoding, tugas pemrograman | Keluasan, pengetahuan encoding, scripting |
| **OSINT** | Deskripsi seseorang, perusahaan, atau acara | Temukan informasi yang mereka tinggalkan secara publik | Riset yang jeli dan jago Google |
| **Blockchain** | Smart contract (sering kali di testnet yang disediakan) | Temukan bug — reentrancy, kontrol akses yang lemah — dan kosongkan kontrak | Semantika Solidity dan logika kontrak |

Misc diam-diam menyimpan banyak poin: **steganography** (menyembunyikan data di gambar/audio), **rantai encoding** (base64 → hex → ROT13), dan tugas **pemrograman** singkat di mana Anda menulis skrip untuk menghasilkan jawabannya.

## Memilih kategori pertama Anda

Gunakan ini sebagai alat bantu keputusan — pilih apa pun yang cocok dengan cara Anda selama ini berpikir:

- **Anda suka teka-teki, pola, dan matematika** → mulai dari **crypto**. Sandi klasik bisa diselesaikan dengan logika kertas-pensil plus satu alat.
- **Anda suka membaca kode dan mengutak-atik aplikasi** → mulai dari **web**. Siklus umpan baliknya cepat: klik, request, response.
- **Anda suka membongkar sesuatu** → mulai dari **rev**. Jika Anda pernah bertanya "bagaimana program ini tahu?", itu rev.
- **Anda suka alat, artefak, dan cerita detektif** → mulai dari **forensics**. Setiap tantangan berbunyi "apa yang terjadi di sini?"
- **Anda suka meneliti orang dan kisah** → mulai dari **OSINT**. Satu-satunya alat yang benar-benar Anda butuhkan adalah kotak pencarian.
- **Anda hanya ingin flag pertama dengan cepat** → ambil **warmup misc** dan **web** dengan poin terendah; sebagian besar acara menyiapkan kemenangan mudah di setiap kategori.

Apa pun pilihan Anda, tetap baca setiap deskripsi tantangan — flag yang mudah kadang tersembunyi di kategori yang Anda lewati.

## Tumbuh bersama ekosistem CSIX

Keluarga basis belajar CSIX membawa Anda lebih dalam daripada yang pernah dilakukan sebuah acara CTF. Setelah (atau seiring dengan) latihan CTF Anda, situs-situs pendamping ini mengantar Anda dari "berhasil menyelesaikan tantangan" hingga "memahami bidangnya":

- **[CSIX Pentest](https://siedysoes.github.io/csix-pentest/)** — kedalaman tentang peretasan web: metodologi, eksploitasi, dan platform lab untuk saat web menjadi kategori Anda.
- **[CSIX Reversing](https://siedysoes.github.io/csix-reversing/)** — kedalaman tentang reverse engineering, untuk hari ketika `strings` tidak lagi cukup.
- **[CSIX OSINT](https://siedysoes.github.io/csix-osint/)** — kedalaman tentang open-source intelligence, untuk mengubah tantangan OSINT menjadi kumpulan keterampilan riset yang nyata.

## Bagaimana kesulitan berskala

Poin adalah perkiraan kasar penyelenggara tentang seberapa sulit sebuah tantangan — sekaligus sinyal berapa banyak tahap yang tersembunyi di dalamnya:

| Rentang poin (umumnya) | Artinya |
|---|---|
| ~50–100 | Pemanasan: satu trik, satu alat, lima menit jika Anda sudah tahu |
| ~150–250 | Satu konsep yang solid plus perkakas sungguhan — Anda akan tersendat sekali atau dua kali |
| ~300–400 | Bertahap: beberapa ide dirangkai, skrip khusus diperlukan |
| ~500 | Pertarungan bos: pengetahuan khusus, perkakas berat, atau berjam-jam perjuangan |

Seorang pemula yang menyelesaikan tantangan 50 poin melakukan pekerjaan yang *sama jenisnya* dengan profesional yang menyelesaikan tantangan 500 poin — hanya dengan lebih sedikit lapisan yang dikupas. Keterampilannya ada pada pengupasan itu.

> **Catatan:** rentang poin berbeda tiap acara. Beberapa acara membatasi semuanya di 500, yang lain memakai rentang 100–500, dan "100" di satu acara bisa jadi "300" di acara lain. Gunakan rentang di atas sebagai panduan kasar, lalu percayai deskripsinya.

## Lihat juga

- [Apa Itu CTF](what-is-ctf.md) — flag, sejarah, dan mengapa ini bekerja
- [Format & Skor](competition-formats.md) — bagaimana poin dan kesulitan berinteraksi dengan penilaian
- [Memulai di CTF](getting-started.md) — cara memilih acara pertama Anda
- [Tantangan Web Exploitation](../challenges/web.md) · [Tantangan Kriptografi](../challenges/crypto.md) — dua pembahasan mendalam kategori di basis ini
