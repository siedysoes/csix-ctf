# Toolkit CTF

> Bagian dari bagian practice & resources **CSIX CTF** — [dasar belajar dwibahasa EN/ID](../../README.md).
> Pendamping [Platform Latihan](platforms.md) dan [Menulis Write-up](writeups.md).
> **Bingkai edukasional: setiap alat di bawah ini untuk challenge yang Anda terima — hanya lab milik Anda sendiri atau instance yang disediakan platform.**

Anda tidak butuh 100 alat. Anda butuh set *kecil* yang tepat, terpasang sebelum challenge pertama, dan peta mental alat mana yang menjawab pertanyaan mana.

> ⚠️ **Verifikasi sebelum memasang:** metode instalasi, nama paket, dan ketersediaan alat sering berubah (terutama di distro Linux dan melalui `pip`/`gem`). Periksa situs resmi atau package manager Anda untuk instruksi terkini — perlakukan setiap sel "cara mendapatkan" sebagai penunjuk, bukan jaminan.

## Toolkit per kategori

### Umum — fondasi setiap kategori

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **Python 3 + pip** | Bahasa scripting CTF: parsing, otomatisasi sekali pakai, matematika kripto | Pra-terpasang di sebagian besar Linux/macOS; `apt install python3 python3-pip`, `brew install python3`, atau python.org di Windows (idealnya via WSL2) |
| **CyberChef** | Encoding/decoding seret-dan-lepas: base64, hex, XOR, hash, kompresi, regex — mesin "bagaimana kalau saya coba semua decoding" | Aplikasi web gratis di cyberchef.org (tanpa instalasi); bisa di-self-host dari repositori GitHub resminya (gchq/CyberChef) |
| **gdb** | Debugger GNU: step through sebuah program, memeriksa memori dan register — esensial untuk pwn | `apt install gdb`, `brew install gdb` (macOS mungkin butuh langkah codesigning — verifikasi); sering sudah terpasang di Kali |
| **git** | Meng-clone alat, melacak write-up dan skrip Anda sendiri | `apt install git`, `brew install git`, git-scm.com, terpasang di sebagian besar lingkungan pengembangan |

### Web — menjebol dan memeriksa aplikasi web

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **Burp Suite Community** | Menyadap dan mengedit request/response HTTP di antara browser Anda dan server — alur kerja inti challenge web | Edisi Community gratis dari portswigger.net (butuh akun untuk mengunduh; verifikasi ketentuan terkini) |
| **curl** | Mengirim request HTTP bebas dari terminal; bisa diskripkan, presisi, tanpa noise browser | Pra-terpasang di Linux/macOS/WSL; selain itu `apt install curl` |
| **Browser dev tools** | Memeriksa cookie, storage, request, dan kode sisi klien (tab Elements/Network) | Bawaan setiap browser modern — gratis |
| **sqlmap** | Mengotomatiskan deteksi dan eksploitasi SQL injection ⚠️ **hanya untuk lab** — arahkan HANYA ke challenge/instance WebGoat milik Anda sendiri, jangan pernah ke situs yang tidak Anda miliki | sqlmap.org atau `apt install sqlmap` — **hanya legal terhadap target yang berwenang** |

### Crypto — cipher, hash, RSA

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **CyberChef** | Tujuan pertama untuk puzzle encoding: base64, hex, rot13, XOR, resep analisis frekuensi | Aplikasi web (lihat bagian Umum) |
| **pycryptodome** | Pustaka Python untuk AES, RSA, hash — implementasikan kode serangan alih-alih menulis kripto dari nol | `pip install pycryptodome` |
| **RsaCtfTool** | Mencoba serangan RSA yang umum (e kecil, modulus umum, Wiener, dll.) — alat berlevel *sebutan*: periksa GitHub-nya untuk langkah instalasi dan sy Python terkini, karena dependensi berubah dan proyeknya bisa tertinggal | Dari repositori GitHub resminya — **verifikasi instruksi instalasi terkini** |
| **hashid** | Mengidentifikasi *algoritma* mana yang menghasilkan sebuah string hash — langkah pertama sebelum mencoba memecahkannya | `pip install hashid` atau `apt install hashid` (verifikasi ketersediaan paket) |

### Reverse engineering — apa sih binary ini lakukan?

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **Ghidra** | Disassembler/decompiler gratis dan open-source (dari NSA) — membaca ELF/PE dan menampilkan kode pseudo-C; suite RE gratis standar | Unduhan gratis dari situs resmi Ghidra (ghidra-sre.org / GitHub NationalSecurityAgency/ghidra); butuh sebuah JDK |
| **file** | Mengidentifikasi jenis file sebelum Anda menyentuhnya (ELF, zip, pcap…) — selalu langkah 1 | Pra-terpasang di Linux/macOS/WSL (`apt install file`) |
| **strings** | Mengekstrak teks terbaca dari binary apa pun — sering menemukan flag tertanam di RE pemula | Terpasang bersama binutils (`apt install binutils`) |
| **radare2 / rizin** | Disassembler berbasis terminal — lebih ringan/lebih cepat daripada Ghidra, bisa diskripkan, bagus untuk crackmes kecil | rizin via `apt install rizin` atau rilis GitHub-nya; radare2 via repositori resmi/script instalasinya — **verifikasi metode terkini** (nama paket berubah) |
| **ltrace / strace** | Memantau pemanggilan pustaka / system call sebuah program saat berjalan — cara tercepat untuk melihat apa yang binary *lakukan* saat dieksekusi | `apt install ltrace strace` (sering sudah terpasang di Kali) |

### Pwn — mengeksploitasi program

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **pwntools** | Framework Python untuk pwn CTF: mengemas alamat, berbicara dengan layanan remote, helper ROP | `pip install pwntools` (di Linux Anda mungkin perlu header dev Python dulu, mis. `apt install python3-dev` — verifikasi) |
| **checksec** | Menampilkan proteksi sebuah binary (NX, PIE, stack canary, RELRO) — memberi tahu Anda strategi eksploitasi mana yang mungkin | Ikut terpasang dengan pwntools (`pwn checksec <file>`); ada skrip mandiri di GitHub — verifikasi terkini |
| **GDB + GEF atau pwndbg** | Debugging interaktif dengan plugin *ramah-CTF*: tampilan register, stack, dan memori yang mudah dibaca | `apt install gdb`, lalu pasang GEF (github.com/hugsy/gef) atau pwndbg (github.com/pwndbg/pwndbg) via perintah instalasi terkini mereka — verifikasi |

### Forensik — menemukan yang tersembunyi

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **binwalk** | Memindai file untuk payload/arsip tertanam — "apakah ada file lain di dalam file ini?" | `apt install binwalk` (catatan: versi pip dan distro bisa berbeda — verifikasi mana yang Anda dapatkan) |
| **foremost** | Mengukir file tertanam (gambar, dokumen) keluar dari file atau disk image yang lebih besar | `apt install foremost` |
| **exiftool** | Membaca metadata gambar/dokumen: kamera, penulis, GPS, stempel waktu — tempat persembunyian petunjuk favorit | `apt install libimage-exiftool-perl` (nama paket berbeda per distro — verifikasi) |
| **steghide** | Mengekstrak data yang disembunyikan di gambar/audio dengan passphrase (sering kali passphrase-nya *adalah* petunjuknya) | `apt install steghide` |
| **zsteg** | Menemukan data LSB/tersembunyi di gambar PNG/BMP — pemindai stego gambar standar | Ruby gem: `gem install zsteg` (butuh Ruby — verifikasi) |
| **Wireshark** | Penganalisis paket berbasis GUI: memeriksa trafik jaringan yang ditangkap, mengikuti stream TCP, memfilter protokol | Gratis dari wireshark.org; `apt install wireshark` (mungkin butuh pengaturan grup tambahan — verifikasi) |
| **Volatility** | Forensik memori: mengekstrak proses, koneksi, dan secret dari dump RAM | Volatility 3 dari GitHub resminya (verifikasi instalasi: sering `pip install volatility3` atau dari source); Volatility 2 yang lebih lama masih muncul di write-up lama — periksa mana yang diharapkan challenge |

### OSINT — pengintaian data publik

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **theHarvester** | Mengumpulkan email, subdomain, dan nama dari sumber pencarian publik | `apt install theharvester` atau dari repositori GitHub-nya — verifikasi |
| **Google dorks** | Operator pencarian (`site:`, `filetype:`, `intitle:`) yang memunculkan halaman terindeks tetapi terselubung | Gratis — hanya mesin pencari; pelajari sintaks operatornya |
| **TinEye / reverse image search** | Menemukan di mana sebuah gambar muncul secara online, atau menemukan versi beresolusi lebih tinggi/versi aslinya | Layanan web di tineye.com (dan fitur reverse-search mesin lain) — paket gratis bervariasi, verifikasi |

⚠️ **Aturan OSINT:** publik tidak berarti boleh bebas dijadikan bahan *penggangguan*. Di CTF, gunakan hanya lingkup yang dinyatakan challenge; di luar CTF, hormati privasi dan hukum setempat.

### Misc — kantong serba ada

| Alat | Apa itu / kenapa Anda butuh | Cara mendapatkannya (verifikasi terkini) |
|---|---|---|
| **zbarimg** | Mendekode kode QR dan barcode dari file gambar — umum di misc/forensik pemula | `apt install zbar-tools` (menyediakan `zbarimg`) |
| **One-liner Python** | `python3 -c "..."` untuk matematika cepat, konversi, dan operasi file — sering kali lebih cepat daripada mencari-cari alat | Bawaan bersama Python 3 |
| **hashcat / john** | Pemecah password-hash ⚠️ **hanya untuk lab** — pakai pada file challenge dan hash uji milik Anda sendiri, jangan pernah pada data yang Anda tidak berwenang memecahkannya | `apt install hashcat john` — **hanya legal terhadap target yang berwenang** |

## Alat mana untuk pertanyaan mana

Saat Anda membuka sebuah challenge dan tidak tahu harus mulai dari mana, cari *pertanyaan* Anda di tabel ini:

| Pertanyaan Anda | Mulai dari | Lalu |
|---|---|---|
| "Ini file jenis apa?" | `file` | `binwalk` (ada yang tertanam?), `strings` (ada teks terbaca?) |
| "Apakah ada teks terbaca yang tersembunyi?" | `strings \| grep -i flag` | CyberChef (encoding?) |
| "Apakah ada file lain yang tersembunyi di dalam ini?" | `binwalk` | `foremost` untuk mengukirnya keluar |
| "Apa isi metadata file ini?" | `exiftool` | Editor hex / pemeriksaan manual |
| "Apakah ada data tersembunyi *di dalam* gambar ini?" | `zsteg` (PNG/BMP) → `steghide` (passphrase?) | Pemeriksaan visual lapis demi lapis |
| "Apa isi QR/barcode ini?" | `zbarimg` | — |
| "Apakah hash ini lemah/algoritmanya apa?" | `hashid` | john/hashcat (**hanya untuk lab**) |
| "Ini encoding atau enkripsi?" | CyberChef (coba base64/hex/XOR) | Python (kripto nyata → pycryptodome, alat RSA) |
| "Apa yang ditunjukkan tangkapan jaringan ini?" | Wireshark (display filter, follow stream) | Ekspor objek dari tangkapan tersebut |
| "Apa yang terjadi di mesin ini (memory dump)?" | Volatility 3 | Silang-referensikan daftar proses/jaringan |
| "Apa binary ini lakukan?" | `file` → `strings` → jalankan (`ltrace`/`strace`) | Ghidra atau rizin untuk gambaran lengkap |
| "Bagaimana saya mengeksploitasi binary ini?" | `checksec` (proteksi apa saja?) | gdb+GEF/pwndbg untuk debug → pwntools untuk diskripkan |
| "Apakah request web ini bisa dimanipulasi?" | Browser dev tools | Burp Suite untuk menyadap/mengedit; curl untuk replay |
| "Siapa/apa yang berada di balik petunjuk publik ini?" | Google dorks, theHarvester, TinEye | Tetap ketat dalam lingkup challenge |

**Heuristik:** cek murah dulu (`file`, `strings`, metadata), analisis mahal belakangan (decompiler, forensik memori). Sebagian besar flag pemula tumbang di dua baris pertama.

## Catatan penyiapan VM

Banyak alat CTF berbasis Linux. Anda punya tiga opsi yang wajar:

| Opsi | Kelebihan | Kekurangan | Catatan |
|---|---|---|---|
| **Kali Linux atau Parrot OS di sebuah VM** (VirtualBox/VMware) | Set alat besar sudah terpasang; terisolasi dari OS utama Anda; gratis | Butuh alokasi ~4–8 GB RAM; ada sedikit overhead | Unduh ISO **hanya dari situs resmi** (kali.org, parrotsec.org) dan verifikasi checksum — jangan pernah tautan mirror acak |
| **Pasang alat di OS Anda sendiri** (Ubuntu/macOS/Windows + WSL2) | Cepat, tanpa overhead VM; alat terintegrasi dengan file Anda | Penyiapan manual, cakupan alat bervariasi; ada celah di macOS/Windows untuk sebagian alat RE/pwn | WSL2 di Windows kini merupakan jalan tengah yang sangat praktis untuk sebagian besar kategori |
| **Hibrida** — WSL2/host untuk scripting + satu VM Kali untuk alat berat | Terbaik dari kedua dunia | Dua lingkungan untuk dirawat | Penyiapan kerja yang umum |

**Tips penyiapan:**
- Apa pun pilihan Anda, **pasang set Umum dulu** (Python 3, bookmark CyberChef, git) — itu membuka jalan untuk setiap kategori.
- Jaga VM latihan Anda tetap **bersnapshot**: ambil snapshot bersih setelah penyiapan agar Anda bisa rollback setelah eksperimen yang berantakan.
- Catat di write-up Anda lingkungan mana yang Anda pakai (pembaca di sistem lain perlu tahu).
- Instance yang disediakan platform (HackTheBox, TryHackMe, OverTheWire, lab PortSwigger) sama sekali tidak membutuhkan VM — Anda berinteraksi dengan *lingkungan mereka*.

## Aturan emas

> **Alat hanyalah selegal target yang Anda arahkan kepadanya.**

Burp Suite, sqlmap, pemindai sekelas nmap, hashcat — tidak ada yang ilegal untuk *dimiliki* atau *dipelajari*. Yang menentukan legalitas adalah targetnya:

- ✅ **Berlatih hanya pada challenge yang diberikan:** lab VM milik Anda sendiri, aplikasi yang Anda jalankan sendiri (WebGoat, VulnHub), dan instance yang secara eksplisit diberikan platform CTF kepada Anda.
- ❌ **Jangan pernah mengarahkannya ke hal lain:** situs web orang lain, sistem pemberi kerja atau sekolah Anda, perangkat teman, atau "sekadar memindai" host acak.

Alat yang dipakai pada target yang disediakan platform adalah *latihan*. Perintah identik yang diarahkan ke sistem yang tidak diberikan kepada Anda berpotensi menjadi kejahatan di sebagian besar yurisdiksi — biner-nya tidak berubah; izinnya yang berubah.

**Sebelum setiap perintah, Anda harus bisa menjawab: "Apakah target ini *diberikan* kepada saya?"** Jika jawabannya bukan ya yang jelas, hentikan.

---

**Terkait:** [Platform Latihan](platforms.md) · [Menulis Write-up](writeups.md)


## Lihat juga

- [Kategori CTF](../fundamentals/ctf-categories.md)
- [Platform Latihan](platforms.md)
- [Challenge Eksploitasi Web](../challenges/web.md)
- [Menulis Write-up](writeups.md)
