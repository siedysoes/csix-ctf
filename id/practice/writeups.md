# Menulis Write-up

> Bagian dari bagian practice & resources **CSIX CTF** — [dasar belajar dwibahasa EN/ID](../../README.md).
> Pendamping [Platform Latihan](platforms.md) — ini adalah langkah ke-4 dari siklus belajar.
> **Bingkai edukasional: dokumentasikan challenge yang Anda pecahkan sendiri. Berlatih hanya pada challenge yang diberikan.**

## Mengapa write-up penting

Write-up adalah catatan tentang *bagaimana* Anda memecahkan sebuah challenge — bukan hanya *apa* flag-nya. Menulis satu memberi empat keuntungan:

| Manfaat | Apa yang terjadi |
|---|---|
| **Penolong ingatan** | Proses menulis memaksa Anda merekonstruksi penalaran Anda. Penelitian tentang pembelajaran konsisten menunjukkan retrieval + penjelasan mengalahkan membaca ulang. Enam bulan kemudian, write-up Anda adalah satu-satunya bukti bagaimana Anda memecahkannya — flag memudar, metode tidak. |
| **Portofolio** | Folder publik berisi write-up yang jelas adalah bukti keterampilan termurah dan paling jujur yang bisa Anda tunjukkan kepada rekan tim, mentor, atau pemberi kerja. Siapa pun bisa mengklaim "saya paham keamanan web"; walkthrough 15 challenge yang diselesaikan *menunjukkannya*. |
| **Mengajar lebih baik daripada membaca** | Membaca solusi mengajarkan pengenalan; *menulis* solusi Anda sendiri mengharuskan Anda tahu kenapa setiap langkah berhasil. Jika Anda tidak bisa menjelaskannya, Anda tidak mempelajarinya — lebih baik menemukan itu sekarang daripada saat kompetisi. |
| **Kontribusi komunitas** | Write-up pemula sangat berharga secara tidak proporsional: para ahli menulis untuk sesama ahli, dan pemula menulis untuk pemula *berikutnya* — orang yang mengecek pesan error persis milik Anda di Google pukul 2 pagi. |

**Patokannya:** jika memecahkan butuh 30 menit, alokasikan 10 menit untuk mendokumentasikannya. Dokumentasi itu bagian dari solve, bukan pekerjaan rumah yang opsional.

## Struktur write-up

Setiap write-up yang baik menjawab pertanyaan yang sama dengan urutan yang sama:

| Bagian | Isinya | Panjang |
|---|---|---|
| **Judul + metadata** | Nama challenge, acara/platform, kategori, poin (jika ada), tanggal selesai, kesulitan (menurut Anda) | 3–5 baris |
| **Ringkasan** | Dua kalimat: challenge-nya satu kalimat, hasil Anda kalimat lainnya. Pembaca seharusnya bisa tahu apakah write-up ini relevan tanpa membaca lebih jauh. | 2 baris |
| **Prasyarat** | Alat, file, dan pengetahuan sebelumnya yang diperlukan untuk mengikuti (mis., "shell Linux, `binwalk`, pemahaman dasar file ELF") | Daftar butir |
| **Solusi** | Langkah demi langkah: apa yang Anda lakukan, perintah/kode-nya, output-nya, **dan kenapa Anda melakukannya**. Sertakan jalan buntu secara singkat. | Bagian terbesar |
| **Yang saya pelajari** | Teknik yang bisa dipindahkan, bukan flag-nya. "Periksa file tertanam dengan `binwalk` sebelum menganggap sebuah file adalah seperti klaimnya" — itu berlaku untuk 50 challenge berikutnya. | 2–6 baris |
| **Referensi** | Tautan ke dokumentasi, write-up lain yang Anda baca, deskripsi challenge resmi. Beri kredit di mana kredit itu pantas. | Butir |

## Contoh: kerangka write-up lengkap

Di bawah ini adalah **challenge pemula fiktif** dengan gaya picoCTF klasik — sebuah binary dengan string flag tertanam. Salin kerangka ini sebagai templat Anda. (Flag-nya hanya placeholder: `flag{...}` — jangan pernah menerbitkan flag sungguhan dari kompetisi berjalan selama acaranya masih berlangsung.)

````markdown
# strings everywhere — a beginner RE warm-up

**Meta**
- Platform/Event: picoCTF-style practice (fictional challenge)
- Category: Reverse Engineering / Misc
- Points: 50 (practice)
- Solved: 2026-01-05 — took ~12 minutes
- Difficulty (my opinion): ★☆☆☆☆

**Summary**
A tiny binary refuses to print anything useful when run. Running `strings` on it
reveals a readable flag embedded right in the executable.

**Prerequisites**
- A Linux shell (native, WSL, or a VM — see [toolkit.md](toolkit.md))
- `strings` (ships with binutils) and `file`
- No reversing experience required

**Solution**

1. **Identify the file before touching it.**

   ```
   $ file rev_strings
   rev_strings: ELF 64-bit LSB executable, x86-64, ...
   ```

   *Why:* `file` tells you what you're holding. An ELF executable behaves
   differently from a zip or a script — knowing the type picks the toolset.

2. **Look for readable text first — it's the cheapest signal.**

   ```
   $ strings rev_strings | head
   ...
   welcome to the challenge
   ...
   flag{example_only_not_a_real_flag}
   ...
   ```

   *Why:* beginner challenges often hide data in plain sight. `strings` extracts
   printable character runs; flags have a recognizable pattern (`flag{...}`,
   `picoCTF{...}`, etc.), so grep-able output often solves it outright.

3. **Verify, don't guess.** Copy the candidate flag and submit it to the
   platform's checker to confirm it's the intended answer.

   *Why:* `strings` can produce coincidental matches (library names, error
   messages). Only the checker confirms you found *the* flag.

**Dead ends (kept brief on purpose)**
- First I just ran `./rev_strings` — it printed garbage/asked for input and
  exited. Noted: running unknown binaries is safe *inside this challenge*, but
  I should still check the file type first (that's what step 1 is for).

**What I learned**
- Order of operations for unknown files: `file` → `strings`/`binwalk` → only
  then execute or disassemble.
- "Hidden" data in beginner challenges is often *embedded*, not encrypted.
- Always confirm with the platform checker before trusting a candidate flag.

**References**
- Challenge description (fictional, created for this example)
- `man strings`, `man file`
- [The CTF Toolkit — general tools](toolkit.md)
````

**Anatomi bagian solusi yang hebat:** setiap langkah punya *aksi*-nya, *output*-nya, dan *kenapa*-nya. Jika Anda menghapus paragraf "kenapa", yang tersisa hanyalah transkrip; transkrip tidak mengajarkan apa pun kepada siapa pun.

## Aturan nilai pengajaran

Terapkan ini pada setiap write-up yang Anda terbitkan atau simpan:

1. **Jelaskan KENAPA setiap langkah, bukan hanya apa.** "Saya menjalankan `binwalk -e`" adalah transkrip. "Saya menjalankan `binwalk -e` karena file itu mengklaim diri sebagai JPEG tetapi 40 KB lebih besar dari ukuran JPEG yang wajar — payload tertanam biasanya berada setelah data gambar" itu baru mengajar.
2. **Tampilkan perintah DAN output yang relevan.** Perintah memungkinkan pembaca mereproduksi; output membuktikan perintah melakukan apa yang Anda klaim. Pangkas noise (pakai `| head`, sorot baris yang penting), jangan pernah mengarangnya.
3. **Catat jalan buntu — secara singkat.** Satu atau dua baris: apa yang Anda coba, kenapa gagal, apa yang membuat Anda berbalik arah. Jalan buntu mengajarkan pembaca *mengenali* belokan yang salah, dan membuat write-up Anda jujur alih-alih mencurigakan sempurna.
4. **Nyatakan asumsi.** "Saya mengasumsikan little-endian karena modul pengantar platform mengatakan begitu" membantu pembaca yang tidak berbagi latar belakang Anda.
5. **Sebutkan teknik yang bisa dipindahkan** di bagian "Yang saya pelajari." Flag itu unik; teknik itu bisa dipakai ulang. Tulis untuk tekniknya.
6. **Beri kredit sumbernya.** Jika sebuah petunjuk, rekan tim, atau write-up lain membawa Anda melewati tembok, katakan begitu — itu profesional dan mengajarkan pembaca di mana harus mencari.

## Etika spoiler

Flag adalah spoiler. Platform berbeda soal kapan — dan apakah — Anda boleh membagikannya.

- **Periksa aturan platform dulu.** Sebagian besar membedakan antara challenge *latihan/arsip* (write-up diterima, sering didorong) dan *kompetisi berjalan* (write-up biasanya dilarang sampai acara selesai). Aturan berubah; baca halaman kebijakan atau aturan terkini, bukan blog post dari 2019.
- **Selama kompetisi berjalan:** jangan menerbitkan flag, solusi lengkap, atau petunjuk yang secara trivial menghasilkan flag — di media sosial, Discord, blog, atau GitHub. Ini termasuk solusi parsial yang "membantu".
- **Setelah acara:** banyak acara secara eksplisit mengundang write-up; sebagian masih membatasi kategori tertentu (mis., challenge dari sponsor). Ragu? Tanya panitia.
- **Di platform latihan:** write-up untuk masalah yang sudah diselesaikan/berarsip umumnya boleh, tetapi jangan memasang jawaban untuk challenge yang *saat ini* merupakan bagian dari kontes berjalan di platform tersebut.
- **Buramkan atau beri placeholder pada flag sungguhan** jika Anda ragu: `flag{...}` / `REDACTED` menyampaikan metode tanpa jawabannya.
- **Platform yang melacak flag yang disalin:** sebagian mendeteksi flag yang dikirim dari write-up dan bisa menjatuhkan sanksi ke akun. Alasan lain mengapa metode — bukan flag — lah yang menjadi intinya.

**Saat menerbitkan, tanyakan:** "Apakah ini merusak acara berjalan, atau kesempatan orang lain untuk berjuang secara produktif?" Jika ya untuk salah satunya, tunda atau redaksi.

## Di mana menerbitkan

| Tempat | Cocok untuk | Catatan |
|---|---|---|
| **Blog pribadi** (situs statis apa pun atau platform hosting) | Kendali penuh, domain dan gaya Anda sendiri | Titik mulai paling sederhana — bahkan satu `writeups.md` pun sudah cukup |
| **Repositori GitHub / GitHub Pages** (seperti dasar CSIX CTF ini) | Write-up terkontrol versi, mudah ditautkan, gratis | Bagus untuk portofolio; jaga struktur folder yang konsisten (`category/challenge-name.md`) |
| **Agregasi write-up CTFtime** | Keterbacaan — pembaca mencari write-up acara di CTFtime | Verifikasi ketersediaan, aturan pengiriman, dan persyaratan akun terkini di ctftime.org sebelum mengandalkannya; kebijakan berubah |
| **Solusi/forum bawaan platform** | Diskusi di dalam platform setelah acara | Sebagian platform punya utas solusi resmi — periksa apa yang diizinkan di sana |
| **Komunitas (Discord/Reddit/blog tim)** | Umpan balik dari sesama | Hormati aturan spoiler setiap komunitas sebelum memasang flag |

Apa pun pilihan Anda: pilih **satu** rumah dan terbitkan secara konsisten. Sepuluh write-up di satu repo rapi mengalahkan lima puluh yang tersebar di lima platform.

## Kebiasaan dokumentasi saat memecahkan

Write-up yang baik lahir *selama* solve, bukan setelahnya. Buka file scratch dari menit pertama:

```
## 2026-01-05 14:02  challenge: rev_strings (picoCTF-style, 50pts)
- downloaded file, ran `file` -> ELF 64-bit executable
- ran ./rev_strings -> asks for input, exits. dead end?
- ran strings -> spotted flag{example...} near line 40
- submitted -> accepted
- lesson: file -> strings -> only then execute/disassemble
```

**Kebiasaan yang membuat write-up mudah ditulis kemudian:**

| Kebiasaan | Kenapa berhasil |
|---|---|
| **Stempel waktu di setiap entri** | Merekonstruksi linimasa Anda kemudian memakan waktu beberapa detik, bukan tebakan |
| **Tempel perintah saat Anda menjalankannya** (salin dari riwayat terminal) | Tidak ada lagi "flag-nya tadi apa ya?" — Anda punya sintaks yang persis |
| **Tempel output yang sudah dipangkas** (`| head`, screenshot bagian yang menarik) | Bukti yang menjadi dasar keputusan Anda terlindungi |
| **Screenshot momen kunci** (request Burp, dekompilasi Ghidra, filter Wireshark) | Bukti visual yang bisa Anda sematkan kemudian; menghemat pengulangan setup |
| **Satu file per challenge, atau satu log berjalan dengan header yang jelas** | Keduanya bisa — konsistensi lebih penting daripada format |
| **Catat jalan buntu saat Anda mengalaminya** | Anda *akan* melupakan jalan buntu itu dalam 20 menit; catat sekarang juga |
| **Catat "kenapa"-nya begitu Anda menemukannya** | Wawasan itu bagian yang rapuh; perintah mudah diturunkan kembali |

Sepuluh menit catatan jujur selama solve = write-up yang selesai dalam sepuluh menit sesudahnya.

## Checklist kualitas

Sebelum Anda menerbitkan (atau bahkan mengarsipkan) sebuah write-up, centang ini:

- [ ] **Metadata lengkap:** acara/platform, kategori, poin, tanggal, kesulitan versi penilaian Anda sendiri
- [ ] **Ringkasan muat dalam dua kalimat** dan memberi tahu pembaca apakah ini relevan bagi mereka
- [ ] **Prasyarat tercantum** — pendatang baru tahu apa yang perlu mereka pasang
- [ ] **Setiap langkah solusi punya:** aksinya, output-nya (yang dipangkas), dan sebuah *kenapa*
- [ ] **Perintah bisa disalin-tempel** dan output sungguhan, bukan karangan
- [ ] **Setidaknya satu jalan buntu atau belokan salah dicatat** (jujur + nilai pengajaran)
- [ ] **"Yang saya pelajari" menyebutkan teknik yang bisa dipindahkan**, bukan cuma flag-nya
- [ ] **Sumber dikreditkan** (dokumentasi, petunjuk, write-up lain, rekan tim)
- [ ] **Spoiler ditangani:** tanpa flag kompetisi berjalan; flag sungguhan diburamkan jika ragu
- [ ] **Aturan platform diperiksa** soal *kapan* solusi ini boleh dibagikan
- [ ] **Pemeriksaan kewarasan flag/placeholder** — Anda hanya menerbitkan flag fiktif atau yang memang diizinkan
- [ ] **Baca ulang sekali** — perintah terutama (typo pada flag merusak reproduksi setiap pembaca)

## Kesalahan umum

| Kesalahan | Kenapa merugikan | Perbaikan |
|---|---|---|
| **Hanya flag akhir** | Nilai pengajaran nol; Anda tidak bisa mereproduksinya; itu spoiler tanpa keuntungan | Setiap flag butuh jalur yang menghasilkannya |
| **Tanpa penjelasan langkah** | Pembaca bisa menyalin tetapi tidak belajar; Anda sendiri mungkin juga tidak belajar | Tambahkan kalimat "kenapa" di setiap langkah |
| **Output alat ditempel tanpa interpretasi** | Dinding noise hex/log; pembaca tidak bisa melihat apa yang penting | Pangkas ke baris yang relevan dan katakan apa yang Anda perhatikan |
| **Tanpa metadata** | Mustahil menemukan, mengategorikan, atau membandingkan kemudian | Isi blok judul setiap kali |
| **Write-up ditulis dari ingatan beberapa hari kemudian** | Langkah terlewat, perintah jadi berantakan | Dokumentasikan selama solve (file scratch) |
| **Menerbitkan flag kompetisi berjalan** | Merusak acara bagi orang lain; bisa melanggar aturan; bisa menjatuhkan sanksi ke *akun Anda* | Periksa kebijakan spoiler; buramkan atau tunggu sampai acara selesai |
| **Mengklaim metode orang lain sebagai milik Anda** | Secara etis salah dan mudah terlihat | Beri kredit sumbernya — tidak biaya apa pun dan membangun kepercayaan |
| **Tanpa "yang saya pelajari"** | Anda mendokumentasikan sebuah acara, bukan keterampilan | Selalu sebutkan teknik yang bisa dipakai ulang |
| **Perfeksionisme — tidak pernah menerbitkan** | Write-up yang cukup baik mengalahkan yang sempurna tapi tidak pernah ditulis | Terbitkan dulu, lalu perbaiki; lima tulisan pertama akan kasar dan itu tidak apa-apa |

---

**Berikutnya:** isi file scratch Anda dengan *perintah yang baik* → [Toolkit CTF](toolkit.md) · kembali ke [Platform Latihan](platforms.md)


## Lihat juga

- [Platform Latihan](platforms.md)
- [Toolkit CTF](toolkit.md)
- [Memulai di CTF](../fundamentals/getting-started.md)
- [Apa Itu CTF](../fundamentals/what-is-ctf.md)
