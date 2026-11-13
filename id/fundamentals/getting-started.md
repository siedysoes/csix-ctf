# Memulai di CTF

Anda sudah membaca [Apa Itu CTF](what-is-ctf.md) dan memilih sebuah kategori. Ini adalah panduan untuk acaranya sendiri — apa yang dilakukan sebelum papan dibuka, bagaimana memanfaatkan jam-jam saat acara berjalan, dan apa yang dilakukan setelah acara selesai.

## Sebelum acara

**1. Pilih acara pemula.** Acara pertama Anda seharusnya adalah kompetisi pemula bergaya picoCTF atau CTF komunitas kecil — bukan akhir pekan kelas atas yang papan penuh dengan pertarungan bos 500 poin. Telusuri acara mendatang di [CTFtime](https://ctftime.org/), tanya di komunitas, atau mulai saja di platform latihan (lihat [Platform Latihan](../practice/platforms.md)).

**2. Siapkan perkakas Anda — lihat [Toolkit CTF](../practice/toolkit.md).** Sebelum jam dimulai, pastikan dasar-dasarnya berfungsi: Python, terminal yang nyaman bagi Anda (Linux, macOS, atau WSL), proxy untuk web (Burp Suite Community Edition), CyberChef untuk encoding, serta `strings`/`file` untuk triase cepat. Masalah perkakas saat acara berlangsung menghabiskan flag Anda; masalah perkakas *sebelum* acara tidak menghabiskan apa pun.

**3. Bentuk atau temukan sebuah tim.** Periksa klub kampus, Discord komunitas, dan halaman tim di CTFtime — sebagian besar acara lebih seru dan lebih efektif dengan 2–4 orang. Main solo juga sepenuhnya baik untuk acara pertama; hanya ketahuilah bahwa tim menutupi titik buta Anda (lihat tabel peran di [Format & Skor](competition-formats.md)). Apa pun pilihannya, sepakati sejak awal: sebuah berkas catatan bersama, cara untuk berbicara (obrolan suara sangat membantu), dan siapa yang mengirim flag.

## Selama acara: alur kerja penyelesaian

Jalankan siklus ini sejak papan terbuka:

```text
1. READ EVERY CHALLENGE DESCRIPTION FIRST — no solving yet.
   Flag hints, given files, and "format: flag{...}" live in the descriptions.
2. SORT the board: category and points, lowest points first.
3. SWEEP THE WARMUPS — grab every 50–100 point flag across all categories.
4. BRANCH OUT: pick your strongest category, commit to one mid-point
   challenge and go deep on it.
5. STUCK ALONE for 20–30 minutes? Switch — new challenge, new category,
   or a teammate (bring what you already tried, see "asking for help" below).
6. LOG every solved flag in your notes file AS YOU GO: challenge, steps,
   final flag. It becomes your write-up later.
7. SUBMIT carefully: correct prefix, correct case, no stray whitespace.
```

Kesalahan pemula terbesar adalah melewatkan langkah 1 dan langsung terjun ke tantangan mana pun yang kedengaran keren. Deskripsi secara rutin memuat petunjuk yang seandainya Anda butuh satu jam untuk menemukannya kembali.

## Manajemen waktu (acara 48 jam)

Acara 48 jam adalah akhir pekan, bukan sprint. Atur kecepatannya:

| Kapan | Maju terus | Ganti/berhenti saat |
|---|---|---|
| 2–3 jam pertama | Warmup di mana-mana — kumpulkan poin mudah dan kenali papan | Jangan menyelami sesuatu secara mendalam dulu |
| Sore hari pertama | Kategori terkuat Anda; satu tantangan poin menengah dengan rencana | 20–30 menit tersendat sendirian → ganti (lihat alur kerja) |
| Malam hari pertama | Percobaan serakah pada tantangan sulit jika Anda masih segar | Anda lelah dan menulis ulang payload yang sama — itu tanda untuk berhenti |
| Semalaman | Tidur (serius) | Kesalahan karena lelah membuang lebih banyak waktu daripada biaya tidurnya |
| Pagi hari kedua | Dekati ulang tantangan yang tersendat kemarin dengan mata segar | Jangan mulai kategori baru sama sekali kecuali Anda sudah tertinggal |
| Jam-jam terakhir | Verifikasi pengiriman, baca ulang deskripsi untuk flag yang terlewat, selesaikan catatan | Tidak ada penyelaman baru yang berisiko — amankan yang sudah Anda dapat |

Acara dengan durasi lain memampatkan atau meregangkan ritme yang sama: fase pemanasan → kerja mendalam → istirahat → sapuan terakhir.

## Cara berpikir first blood

Dynamic scoring (lihat [Format & Skor](competition-formats.md)) memberi penghargaan pada kecepatan: nilai sebuah tantangan turun seiring bertambahnya tim yang menyelesaikannya. Artinya:

- **Warmup dulu bukan hanya aman, tapi optimal** — setiap menit yang Anda habiskan untuk tantangan 500 poin selagi warmup sedang menurun menghabiskan poin Anda.
- **First blood** — menjadi tim pertama yang menyelesaikan sebuah tantangan — sering kali bernilai gengsi *dan* poin maksimal. Jika Anda hampir berhasil pada tantangan sulit di awal acara, jam adalah sekutu Anda.
- **Jangan mengejar penurunan selamanya.** Jika sebuah tantangan sudah turun ke nilai dasarnya, poin untuk menyelesaikannya kini sudah tetap — selesaikan untuk belajar, bukan untuk skor.

## Cara meminta bantuan dengan benar

"Bantuan, tantangan web ini rusak" tidak mengatakan apa-apa pada tim Anda. Permintaan yang baik memakan 30 detik dan mendapat jawaban dalam 60:

```text
Challenge: "Login As Admin" (web, 150)
Tried: ' OR 1=1-- on both fields -> "invalid login"
       view-source -> one comment, nothing else; robots.txt empty
Stuck on: quotes seem to get stripped (prepared statement?)
Next idea: boolean-based 1=1 vs 1=2 — want to try that?
```

Tampilkan deskripsinya, apa yang sudah Anda coba, apa yang Anda dapat, dan ide Anda berikutnya. Rekan setim bisa langsung berkata "tinggalkan jalur itu, coba X" atau "kamu benar, lanjutkan" — alih-alih menyelesaikan ulang dari nol.

## Kebiasaan mencatat

Tuliskan setiap solusi **saat Anda menyelesaikannya**, bukan sesudahnya:

```text
## Warmup: What's in this file? (50, forensics)
- file mystery -> ELF executable
- strings mystery | grep -i flag -> flag{r3vers1ng_1s_fun}
```

Tiga alasan mengapa kebiasaan ini terbayar selama acara: ia membebaskan ingatan Anda untuk berpikir, memungkinkan rekan setim mengambil alih ide yang setengah jadi, dan ia bertahan setelah acara — langsung berubah menjadi write-up Anda (lihat [Menulis Write-up](../practice/writeups.md)). Tim yang konsisten mencatat selalu unggul atas tim yang tidak, karena mereka tidak pernah menyelesaikan ulang sesuatu yang sudah mereka bobol.

## Setelah acara

1. **Baca write-up orang lain.** Temukan acara Anda di CTFtime, buka tugas-tugas yang Anda gagalkan, dan baca bagaimana orang lain menyelesaikannya. Di sinilah pembelajaran sesungguhnya terkompresi: satu write-up yang baik menggantikan berjam-jam waktu tersendat.
2. **Catat celah Anda.** Daftarkan kategori yang Anda lewati dan teknik yang tidak Anda ketahui. Daftar itu adalah rencana belajar Anda untuk beberapa minggu ke depan.
3. **Berlatih di platform.** Reproduksikan teknik-teknik itu di platform latihan agar menjadi milik Anda, bukan titipan — lihat [Platform Latihan](../practice/platforms.md).
4. **Terbitkan write-up Anda sendiri.** Meskipun singkat. Ia memantapkan pembelajaran Anda, dan ia adalah bukti keterampilan Anda untuk rekan setim dan acara-acara berikutnya.

## Tiga acara pertama Anda

| Acara | Fokus | Tujuan realistis |
|---|---|---|
| Pertama | Rasakan alur kerjanya — baca semua, sapu warmup, ambil catatan | Selesaikan dengan segenggam flag dan sebuah berkas catatan; pahami mekanismenya |
| Kedua | Berkomitmen pada kategori pilihan Anda dari [Kategori CTF](ctf-categories.md) | Selesaikan tantangan poin menengah di kategori itu **tanpa bantuan** |
| Ketiga | Kecepatan dan kerja sama tim | Berkontribusi poin di sebuah tim, ajukan pertanyaan yang baik, terbitkan write-up pertama Anda |

Menjelang acara ketiga, papan akan terasa familiar. Itulah intinya — acara pertama siapa pun tidak pernah masuk akal.

## Acara pertama semua orang membingungkan

Layak dikatakan secara terang-terangan: papan itu terasa seperti bahasa asing bagi semua orang yang kini membuatnya tampak mudah. CTF pertama para veteran pun membuat mereka memandang deskripsi tanpa tahu harus mulai dari mana. Kebingungan bukan tanda bahwa Anda gagal — kebingungan *adalah* kurikulumnya. Baca deskripsi, ambil warmup-nya, ajukan pertanyaan yang baik, catat, dan kembali lagi. Polanya berulang sampai suatu hari Anda mengenali jenis tantangan itu sebelum selesai membacanya.

## Lihat juga

- [Apa Itu CTF](what-is-ctf.md) — dasar-dasarnya
- [Kategori CTF](ctf-categories.md) — ke mana harus fokus
- [Format & Skor](competition-formats.md) — bagaimana acara Anda dijalankan dan dinilai
- [Toolkit CTF](../practice/toolkit.md) · [Platform Latihan](../practice/platforms.md) · [Menulis Write-up](../practice/writeups.md) — siap, latih, terbitkan
