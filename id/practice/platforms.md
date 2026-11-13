# Platform Latihan

> Bagian dari bagian practice & resources **CSIX CTF** — [dasar belajar dwibahasa EN/ID](../../README.md).
> **Bingkai edukasional: berlatihlah pada challenge yang diberikan platform ini kepada Anda — tidak lebih dari itu.** Hanya lab milik Anda sendiri atau instance yang disediakan platform.

## Mengapa platform latihan lebih baik daripada menunggu kompetisi

Kompetisi CTF biasanya berlangsung 24–72 jam, sekali atau dua kali setahun. Jika satu-satunya rencana Anda adalah "menunggu kompetisi berikutnya", mungkin baru berbulan-bulan lagi Anda menyentuh challenge pertama — dan Anda akan berhadapan dengan orang-orang yang berlatih sepanjang waktu.

Platform latihan mengatasi hal ini dengan empat cara:

1. **Mereka tidak pernah tutup.** Challenge yang tidak bisa Anda pecahkan pukul 2 pagi tetap ada esok hari, tanpa tekanan papan skor.
2. **Mereka bersifat bertahap.** Kategori diurutkan menurut tingkat kesulitan, sehingga Anda bisa mulai dari "membaca jenis file" dan berakhir di "mengeksploitasi buffer overflow" tanpa menebak-nebak apa yang harus dipelajari berikutnya.
3. **Mereka memaksa pengulangan.** Memecahkan 30 challenge web pemula mengajarkan pengenalan pola yang tidak akan pernah diajarkan oleh satu kompetisi saja.
4. **Mereka aman secara desain.** Segala sesuatu yang Anda serang adalah target yang memang *ditujukan* platform untuk Anda serang — legal, etis, dan berada dalam lingkup secara bawaan.

Kompetisi adalah ujiannya. Platform latihan adalah semesternya. Kerjakan semesternya dulu.

## Perbandingan besar

> ⚠️ **Verifikasi sebelum Anda berkomitmen:** harga, paket gratis, dan akses challenge sering berubah di setiap platform di bawah ini. Selalu periksa situs resmi untuk ketentuan terkini — perlakukan tabel ini sebagai peta, bukan daftar harga.

| Platform | Biaya | Format | Level | Fokus |
|---|---|---|---|---|
| **picoCTF** | Gratis | Kompetisi tahunan + soal latihan sepanjang tahun | Pemula | Campuran: web, crypto, RE, forensik, misc — "CTF pertama" yang klasik |
| **OverTheWire** | Gratis | Wargame (level berurutan, SSH ke sebuah server) | Pemula → Menengah | Baris perintah Linux, dasar keamanan, eksploitasi dasar |
| **CryptoHack** | Campuran gratis/bayar | Modul interaktif + set challenge | Pemula → Mahir | Kriptografi: encoding, cipher klasik, RSA, kurva elips |
| **HackTheBox** | Gratis/langganan untuk sebagian konten | Mesin ter-hosting, lab, challenge, track | Menengah → Mahir | Pengujian penetrasi pada mesin penuh; sebagian konten butuh paket berbayar |
| **TryHackMe** | Kamar gratis + langganan berbayar untuk sebagian | "Room" berpandu dengan tutorial bawaan | Pemula | Jalur belajar terpandu; langkah pertama yang bagus setelah picoCTF |
| **Root-Me** | Gratis/bayar untuk sebagian | Challenge mandiri, banyak kategori | Pemula → Mahir | Web, pemrograman, forensik, reversing, crypto, jaringan — cakupan luas |
| **CTFlearn** | Gratis | Challenge mandiri buatan komunitas | Pemula | Soal pemanasan lintas kategori, bagus untuk latihan kecepatan |
| **crackmes.one** | Gratis | Crackmes unggahan komunitas (reverse engineering) | Pemula → Mahir | Reverse engineering saja — kumpulan RE gratis terlengkap |
| **PortSwigger Academy** | Gratis | Lab keamanan web (teori + praktik) | Pemula → Mahir | Kerentanan web — kurikulum web gratis terbaik menurut banyak orang |
| **WebGoat** | Gratis (self-hosted) | Aplikasi web Java yang sengaja rentan, Anda jalankan sendiri | Pemula → Menengah | Latihan keamanan web di **mesin Anda sendiri** |
| **VulnHub** | Gratis (VM yang bisa diunduh) | Virtual machine rentan siap pakai yang Anda boot secara lokal | Pemula → Mahir | Eksploitasi sistem penuh, privesc, enumerasi dengan ritme Anda sendiri |
| **CyberDefenders** | Campuran gratis/bayar | Lab tim biru: forensik, respons insiden, threat hunting | Menengah | Keamanan defensif dan forensik digital — sisi "menemukan apa yang terjadi" |

**Cara membaca tabel ini:**
- **Biaya** adalah titik awal, bukan kontrak — paket gratis bisa dipangkas atau diperluas dari waktu ke waktu.
- **Level** menggambarkan *titik masuk*, bukan batas atas; setiap platform punya konten yang sulit.
- **Fokus** memberi tahu Anda keterampilan mana yang benar-benar akan Anda asah.

## Cara memilih: minat Anda → platform awal

| Jika Anda penasaran tentang… | Mulai di sini | Lalu lanjut ke… |
|---|---|---|
| "Saya belum tahu apa-apa, mulai dari mana?" | Soal latihan **picoCTF** | Kamar pemula **TryHackMe** |
| Baris perintah Linux & dasar shell | **OverTheWire Bandit** | Kamar Linux **TryHackMe** |
| Menjebol situs web (login, cookie, injeksi) | **PortSwigger Academy** | Challenge web **Root-Me** → web **HackTheBox** |
| Kode, cipher, "apakah enkripsi ini bisa dipecahkan?" | **CryptoHack** | Challenge crypto **Root-Me** |
| "Apa sih binary ini *lakukan*?" | **crackmes.one** (yang mudah) | RE **picoCTF** → VM **VulnHub** |
| Mengeksploitasi program (overflow, shell) | pwn **picoCTF** → VM latihan lokal | Mesin **HackTheBox** |
| Data tersembunyi di gambar/audio/file | Forensik **picoCTF** | Lab **CyberDefenders** |
| "Siapa yang meninggalkan petunjuk ini di internet?" (data publik) | Tugas beraroma OSINT **Root-Me** / **picoCTF** | Kamar OSINT khusus di **TryHackMe** |
| Membela / menyelidiki serangan | **CyberDefenders** | Jalur tim biru **TryHackMe** |

**Pilih SATU platform awal.** Dua tab akun yang tidak pernah selesai adalah kegagalan pemula yang paling umum.

## Siklus belajar

Setiap challenge yang Anda pecahkan harus melewati empat langkah yang sama. Ulangi sampai jadi otomatis.

```
pick  →  timebox  →  document  →  review write-ups
```

| Langkah | Yang Anda lakukan | Aturan |
|---|---|---|
| **1. Pick** | Pilih satu challenge yang sedikit di atas level Anda saat ini (aturan "satu langkah ke atas") | Satu per satu. Selesaikan sebelum membuka yang lain. |
| **2. Timebox** | Atur timer — 30–45 menit untuk pemula, 60–90 menit untuk yang lebih sulit | Saat berbunyi: pecahkan, atau tulis persis di mana Anda tersangkut. *Berhenti karena rencana bukan berarti menyerah.* |
| **3. Document** | Selama memecahkan, buka file scratch: perintah, output, ide, stempel waktu | Challenge yang terselesaikan tanpa dokumentasi mengajarkan Anda setengahnya saja. |
| **4. Review write-ups** | Baca 1–2 solusi orang lain **setelah upaya Anda sendiri** | Bandingkan metode: "Apakah mereka memakai jalur yang lebih pendek? Kenapa pendekatan saya gagal?" |

Lalu kembali ke langkah 1 dengan challenge yang sedikit lebih sulit.

**Kenapa "setelah upaya Anda" itu penting:** membaca write-up sebelum mencoba terasa efisien, tetapi membangun *pengenalan*, bukan *keterampilan*. Anda akan mengenali solusinya lain kali, tetapi tidak mampu menghasilkannya sendiri. Berjuanglah dulu — write-up-nya baru terasa masuk akal, bukan sihir.

## Menggunakan write-up secara etis

Write-up adalah bahan belajar, bukan kunci jawaban.

**Lakukan:**
- ✅ Coba challenge-nya dulu, dalam timebox Anda.
- ✅ Gunakan write-up untuk membuka jalan di langkah tertentu yang *tujuannya Anda pahami* tetapi tidak bisa dieksekusi ("Saya tahu saya perlu mengekstrak file itu — saya tidak tahu flag `binwalk`-nya").
- ✅ Pelajari **metodenya**: apa pertanyaannya, alat apa yang menjawabnya, dan kenapa.
- ✅ Implementasikan ulang tekniknya pada challenge yang *berbeda* untuk membuktikan Anda benar-benar mempelajarinya.
- ✅ Beri kredit kepada penulisnya saat Anda mengutip solusinya.

**Jangan:**
- ❌ Salin-tempel flag dari write-up ke kotak pengiriman. Anda tidak belajar apa pun, dan banyak platform yang melacaknya.
- ❌ Memecahkan massal dengan membuka tab write-up untuk seluruh kategori. Itu mengasah papan skor Anda, bukan keterampilan Anda.
- ❌ Menerbitkan flag kompetisi berjalan selama acara masih berlangsung (lihat [writeups.md — etika spoiler](writeups.md)).
- ❌ Memakai write-up untuk melewati aturan platform tentang *kapan* solusi boleh dibagikan.

**Ujiannya:** kalau seseorang menghapus write-up Anda dan meminta Anda memecahkan challenge yang *serupa* besok, bisakah Anda? Jika ya, Anda menggunakannya dengan benar.

## 30 Hari Pertama Anda

Rencana konkret untuk pemula dengan ~1 jam/hari. Sesuaikan kecepatannya, pertahankan urutannya.

| Hari | Fokus | Platform | Hasil di akhir |
|---|---|---|---|
| **1–3** | Orientasi: akun, mindset, tur alat | Latihan picoCTF | 2 challenge mudah selesai; file scratch dibuat; toolkit.md dibaca |
| **4–7** | Dasar Linux & shell | OverTheWire (Bandit) | 15–20 level selesai; catatan 10 perintah yang kini Anda kuasai |
| **8–12** | Dasar web | PortSwigger Academy (lab dasar) + web picoCTF | 5 lab web selesai; Burp Suite terpasang dan proxy berfungsi |
| **13–17** | Dasar kriptografi | CryptoHack (modul pengantar) | Modul encoding/cipher klasik selesai; 3 challenge selesai |
| **18–22** | Dasar reverse engineering | crackmes.one (mudah) + RE picoCTF | 3 crackmes selesai; `strings`/`file`/`Ghidra` dipakai minimal sekali masing-masing |
| **23–26** | Dasar forensik | Forensik picoCTF + CyberDefenders (lab pengantar) | 3 challenge forensik selesai; `binwalk` + `exiftool` dipakai sungguhan |
| **27–30** | Tinjauan & konsolidasi | Tinjau ulang solve tersulit Anda + baca 3 write-up | 3 write-up rapi (sesuai [writeups.md](writeups.md)); daftar "apa yang masih saya lemah" |

**Aturan untuk 30 hari:**
- Satu platform dalam fokus pada satu waktu (yang lain boleh menganggur — tidak apa-apa).
- Setiap challenge yang selesai mendapat setidaknya beberapa baris di file scratch Anda.
- Tersangkut 2× timebox Anda? Tulis penghalangnya, intip *satu* write-up untuk langkah itu saja, lalu lanjutkan.
- Di akhir Hari 30: pilih fokus 30 hari *berikutnya* dari tabel "Cara memilih".

Setelah itu, pertimbangkan ikut kompetisi yang ramah pemula (banyak yang digelar online dan mengizinkan keterampilan era latihan) — tetap jalankan siklus latihan Anda apa pun yang terjadi.

## Catatan keamanan — tetapkan lingkup setiap target

Satu-satunya aturan yang menjaga latihan CTF tetap legal dan menyenangkan:

> **Berlatihlah pada challenge yang diberikan platform kepada Anda — di server mereka, instance hosting mereka, atau lab VM milik Anda sendiri. Tidak lebih dari itu.**

- ✅ **Lab milik Anda sendiri:** WebGoat berjalan lokal, VM VulnHub di VirtualBox, crackmes yang Anda unduh, aplikasi yang sengaja rentan di VM terisolasi.
- ✅ **Instance yang disediakan platform:** mesin HackTheBox/TryHackMe yang secara eksplisit mereka serahkan kepada Anda, server permainan OverTheWire, lab hosting PortSwigger.
- ❌ **Segala sesuatu yang lain:** situs web orang lain, sistem perusahaan Anda, jaringan sekolah Anda, IP acak yang Anda temukan, "sekadar memindai untuk melihat".

Kesahihan sebuah alat sepenuhnya bergantung pada **ke mana Anda mengarahkannya**. Memindai target yang disediakan platform CTF = latihan. Memindai router tetangga Anda = kejahatan di sebagian besar yurisdiksi. Alat-alat di [toolkit.md](toolkit.md) semuanya standar — aturan emas di bagian bawah halaman itu berlaku untuk semuanya.

**Pagar pengaman praktis:**
1. Lakukan pengujian agresif hanya di dalam VM atau instance platform.
2. Jauhkan data pribadi/produksi dari VM latihan Anda.
3. Jika sebuah challenge tampak membutuhkan serangan ke sesuatu di *luar* lingkup yang dijelaskannya — hentikan dan laporkan ke platform.
4. Ragu? Lingkupnya adalah deskripsi challenge-nya. Baca dua kali.

---

**Berikutnya:** belajar mengubah setiap solve menjadi artefak yang bisa dipakai ulang → [Menulis Write-up](writeups.md) · [Toolkit CTF](toolkit.md)


## Lihat juga

- [Memulai di CTF](../fundamentals/getting-started.md)
- [Kategori CTF](../fundamentals/ctf-categories.md)
- [Menulis Write-up](writeups.md)
- [Toolkit CTF](toolkit.md)
