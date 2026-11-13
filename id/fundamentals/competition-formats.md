# Format & Skor

Dua pertanyaan menentukan bagaimana sebuah acara *terasa*: format apa yang dipakai, dan bagaimana poin dihitung? Pelajari ini sebelum acara pertama Anda, dan papan peringkat langsung tidak lagi mistis.

## Gaya jeopardy

Format yang dimaksud kebanyakan orang ketika mereka berkata "CTF":

- Papan menampilkan daftar **tantangan independen**, masing-masing dengan kategori, nilai poin, dan deskripsi.
- Tim mana pun boleh mencoba tantangan mana pun kapan pun — tidak ada urutan tetap.
- Selesaikan sebuah tantangan, kirimkan flag, dapatkan poin tantangan itu. Selesai.

Jeopardy ramah pemula karena tantangannya tidak saling bergantung: kalau crypto hari ini tidak mempan, ambil warmup web saja. Hampir semua acara pemula, ditambah platform latihan bergaya picoCTF, memakai format jeopardy.

## Attack-defense (ringkas)

Format klasik lainnya, dan satu tingkat lebih intens:

- Setiap tim menjalankan **salinan identik dari set layanan yang sama** (aplikasi web, server surat, sistem pemungutan suara...).
- Anda **mempertahankan** layanan sendiri (menambalnya, memantaunya) sambil **menyerang** salinan milik tim lain untuk mencuri flag mereka.
- Poin datang dari keduanya: berhasil menyerang tim lain *dan* menjaga layanan Anda tetap hidup.

Attack-defense memberi penghargaan pada tim yang bisa melakukan segalanya sekaligus — cepat, kacau, dan umumnya bukan tempat Anda mengambil acara perdana. Nyamankan dulu di jeopardy; banyak acara kemudian menambahkan satu ronde attack-defense untuk tim tingkat lanjut.

## Format lain (sekilas)

- **Kingslayer** — tim tidak punya skor permanen; siapa pun yang saat ini memegang "mahkota" mendapat poin sementara yang lain berusaha merebutnya, sehingga papan peringkat terus bergerak.
- **Campuran attack-defense + jeopardy** — beberapa acara menjalankan keduanya: ronde kualifikasi jeopardy yang meloloskan ke final attack-defense, atau berjalan berdampingan dalam akhir pekan yang sama.

Periksa halaman acara sebelum Anda mendaftar: format menentukan bagaimana Anda harus bersiap sebagai tim.

## Dynamic scoring: mengapa penyelesaian awal bernilai lebih

Sebagian besar acara modern tidak memakai poin statis. Sebagai gantinya, nilai sebuah tantangan **menurun seiring bertambahnya tim yang menyelesaikannya**:

| Jumlah tim yang sudah menyelesaikannya saat Anda mengirim | Poin untuk tantangan 500 poin (ilustratif) |
|---|---|
| 1–5 | ~500 (nilai penuh) |
| 10 | ~450 |
| 30 | ~350 |
| 100 | ~150 |

Logikanya sederhana: tantangan yang diselesaikan semua orang tidak lagi memisahkan peringkat peserta, jadi nilainya turun. Tim pertama yang menyelesaikannya mendapat hadiah maksimal.

> **Rumus persisnya berbeda tiap acara.** Sebagian memakai kurva halus, sebagian memakai tingkatan, sebagian tetap statis — selalu baca aturan acara untuk melihat bagaimana poin bergerak. Namun *prinsipnya* universal: **kecepatan bernilai poin.**

## Timeboxing

CTF dijalankan melawan jam. **48 jam adalah hal yang umum untuk acara besar** (satu akhir pekan penuh), tapi bisa berbeda — Anda juga akan menemukan format 12 jam, 24 jam, bahkan seminggu penuh, plus platform latihan yang tidak pernah tutup. Acara 48 jam adalah maraton dengan interval lari sprint: jadwal di [Memulai di CTF](getting-started.md) disusun di sekitar itu.

## CTFtime: kalender dan buku catatan

[CTFtime](https://ctftime.org/) adalah hub komunitas yang dipakai setiap pemain CTF — diverifikasi sebagai sumber hidup pada saat penulisan:

- **Events** — acara mendatang dan lampau beserta tanggal, format, dan bobotnya.
- **Teams** — halaman tim dan rating tahunan yang menunjukkan siapa yang kuat saat ini.
- **Tasks** — daftar tantangan per acara, sehingga Anda bisa melihat apa yang sebenarnya diminta sebuah format.
- **Write-ups** — arsip besar solusi publik, tempat Anda pergi *setelah* sebuah acara untuk mempelajari apa yang terlewat.

Tandai sebelum acara perdana Anda: di situlah Anda menemukan kompetisi, menemukan rekan setim, dan belajar.

## Dinamika tim: siapa mengerjakan apa

Jeopardy adalah olahraga tim. Sebuah skuad yang berjalan biasanya kira-kira seperti ini:

| Peran | Apa yang Anda lakukan | Cocok jika Anda... |
|---|---|---|
| Orang web | Menyadap request, menguji login, mengejar parameter | Suka mengklik-klik aplikasi dan membaca HTTP |
| Orang crypto | Mengenali encoding, menjalankan serangan, memfaktorkan angka | Menikmati pola dan teka-teki matematika |
| Orang rev/pwn | Mendisassemble biner, menulis skrip exploit | Suka program, debugger, dan detail level rendah |
| Orang catatan (scribe) | Menandai yang selesai/belum, menjaga berkas catatan bersama, mengirim flag | Teratur dan tenang di bawah tekanan |
| Serba bisa | Menyapu warmup di semua kategori, lalu membantu siapa pun yang tersendat | Suka keragaman dan berganti-ganti terus-menerus |

Peran catatan adalah pahlawan tanpa tanda jasa di setiap tim pemenang — lihat [Memulai di CTF](getting-started.md) untuk kebiasaan mencatat. Di tim kecil, semua orang memakai banyak topi; yang penting adalah *seseorang* bertanggung jawab atas pengiriman flag dan catatan bersama.

## Dasar anti-kecurangan

Setiap acara menerbitkan aturannya sendiri, tapi titik persamaannya adalah:

- **Satu akun per tim.** Berbagi login atau membagi tim ke banyak akun biasanya menjadi alasan diskualifikasi.
- **Tidak berbagi solusi antar tim selama acara.** Mengoordinasikan flag dengan tim lain adalah kolusi; selesaikan tantangan Anda sendiri.
- **Kebijakan write-up berbeda.** Sebagian acara menyambut write-up segera, sebagian meminta Anda menunggu sampai acara selesai — periksa aturannya.
- **Penyelenggara bisa mendeteksi pola yang mencurigakan** — pengiriman identik antar tim, berbagi akun dari satu IP, atau solusi yang tidak muncul di mana pun kecuali di tim Anda.

Jika ragu: baca halaman aturan sekali sebelum acara dimulai. Sepuluh menit di sana mencegah sebagian besar diskualifikasi.

## Contoh penilaian yang dihitung

Tantangan yang sama, solusi yang sama, dua momen berbeda — dynamic scoring beraksi:

```text
Challenge: "Sneaky Login" — listed at 500 points

Team A submits at 10:05, when only 10 teams had solved it
→ dynamic score applies: 450 points

Team B submits at 23:40, when 100 teams had solved it
→ dynamic score applies: 150 points
```

Kedua tim melakukan pekerjaan yang identik. Tim A mendapat **3× lebih banyak** karena memecahkan teka-teki yang sama tiga jam lebih awal. Kalikan itu dengan seluruh papan 40 tantangan, dan Anda akan mengerti mengapa para veteran menyapu buah yang mudah dipetik di jam-jam pertama sebuah acara — serta mengapa "first blood" (penyelesaian pertama sebuah tantangan) begitu dihargai.

*(Angka bersifat ilustratif; rumus persis setiap acara berbeda — lihat aturannya.)*

## Lihat juga

- [Apa Itu CTF](what-is-ctf.md) — dasar yang dibangun artikel ini
- [Kategori CTF](ctf-categories.md) — bagaimana rentang kesulitan dipetakan ke kategori
- [Memulai di CTF](getting-started.md) — mengubah pengetahuan format menjadi rencana acara
- [Menulis Write-up](../practice/writeups.md) — apa yang terjadi dengan solusi Anda setelah acara
