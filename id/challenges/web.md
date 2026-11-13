# Tantangan Web Exploitation

Web adalah kategori CTF yang paling umum sekaligus pintu masuk paling ramah pemula: targetnya adalah sebuah situs web, umpan baliknya instan, dan alat utama Anda hanyalah browser. Anda akan menemukan login yang rusak, parameter yang ceroboh, dan komentar tersembunyi — semuanya sengaja ditanam di sana untuk Anda temukan.

> ⚠️ **Pengingat cakupan:** setiap teknik di sini hanya berlaku untuk **instance tantangan yang diberikan kepada Anda** — situs kompetisi, platform latihan, atau lab Anda sendiri. Jangan pernah menujukkannya ke sistem yang tidak Anda miliki atau yang tidak Anda punya izin untuk diuji.

## Lanskap web di CTF

| Jenis tantangan | Yang diuji | Langkah pertama yang dicoba |
|---|---|---|
| Info tersembunyi di source | Kesabaran, ketelitian | View source (Ctrl+U): komentar HTML/JS, field tersembunyi, tombol yang dinonaktifkan |
| Cookie & localStorage | Penanganan session | Dump cookie/localStorage — cari peran (`admin=0`), flag, atau token teks biasa |
| IDOR / manipulasi parameter | Kontrol akses | Ubah `id=1` menjadi `id=2` (atau `role=user` menjadi `role=admin`) dan perhatikan apa yang berubah |
| SQL injection di login/pencarian | Penanganan input | `'` untuk memancing error, lalu payload klasik seperti `' OR 1=1--` |
| SSTI (server-side template injection) | Template engine | Uji dengan `{{7*7}}` atau `${7*7}` — angka 49 di respons berarti injeksi |
| File inclusion (LFI/RFI) & XXE | Penanganan file/path | Traversal `../`, `file:///etc/passwd`, atau body XML yang membuat server mengambil sebuah file |
| Auth bypass | Logika login | Kredensial default, type-juggling, JWT `alg=none`, forced browsing ke `/admin` |

Jangan menghafal tabelnya — pelajari *posture*-nya: setiap string di halaman adalah input, setiap nilai di storage adalah sekadar saran, dan setiap ID hanyalah tebakan.

## Alur kerja web di CTF

Jalankan urutan berikut pada setiap tantangan web sebelum Anda mulai bermain-main:

1. **Lihat source-nya** — flag asli pernah disembunyikan di komentar HTML lebih banyak kali daripada yang ingin diakui siapa pun.
2. **Periksa yang klasik** — `robots.txt`, `sitemap.xml`, `/flag.txt`, direktori tersembunyi, dan bundle JS untuk endpoint atau logika yang dikomentari.
3. **Mainkan aplikasinya** — klik setiap tombol, kirim setiap form, catat setiap request dan parameter. Anda tidak bisa memanipulasi apa yang belum Anda petakan.
4. **Intercept dengan Burp** — proxy-kan trafiknya, lalu kerjakan di Repeater: ubah satu parameter pada satu waktu dan bandingkan responsnya.
5. **Periksa cookie dan storage** — lalu manipulasikan: ubah nilai, turunkan `admin=0` menjadi `admin=1`, lepas signature, putar ulang sebuah request.

Membosankan, sistematis, bisa diulang — itulah cara memenangkan poin web.

## Tiga besar, langsung praktik

### SQL injection gaya CTF

Ketika input pengguna digabungkan ke dalam query database, input Anda menjadi sintaks. Versi klasik CTF adalah bypass login:

```text
username: admin' OR 1=1--
password: (anything)
```

- `'` keluar dari string literal; `OR 1=1` membuat klausa WHERE selalu true; `--` mengomentari sisa query (di MySQL, `#` juga bisa).
- **Deteksi berbasis error dulu:** kirim `admin'` saja. Error database di respons (atau perubahan perilaku) memberi tahu Anda bahwa input mencapai sebuah query — itu lampu hijau Anda sebelum mencoba payload yang macam-macam.
- Di CTF modern, bypass sering hanya langkah pertama: flag sebenarnya muncul saat Anda **mengekstrak data** — misalnya, dump tabel `users`/`flags` dengan UNION SELECT atau teknik blind boolean.

Semua ini hanya untuk **instance tantangan yang disediakan**. Di situs produksi sungguhan, pengujian injeksi tanpa izin tertulis adalah kejahatan, bukan rasa ingin tahu.

### Dasar XSS

Cross-site scripting berarti aplikasi memantulkan input Anda sebagai HTML/JavaScript. Versi CTF dari reflected XSS biasanya terlihat seperti ini:

1. Tantangan memberi Anda parameter URL atau form yang mengecho input Anda.
2. Anda menyuntikkan sebuah payload script (mulai dari bukti yang tidak berbahaya: `<script>alert(1)</script>`).
3. **Twist-nya:** banyak CTF memberi Anda "admin bot" — sebuah tombol seperti *Report to admin* yang membuat bot mengunjungi URL Anda dengan flag di dalam cookie-nya. Tugas Anda adalah mengekfiltrasi cookie itu (secara konseptual: payload mengirim `document.cookie` ke server yang diberikan tantangan, atau Anda menangkapnya di webhook milik tantangan tersebut).

Itulah mengapa XSS penting: ini adalah kode yang berjalan di *sesi orang lain*. Di tantangan, orang itu adalah bot; di web sungguhan, orang itu adalah pengguna.

### IDOR dan manipulasi parameter

Flag termurah di papan skor sering hanya berupa satu digit:

```text
GET /api/notes?id=1   -> your note: "grocery list"
GET /api/notes?id=2   -> someone else's note: flag{d0nt_trust_cl1ent_ids}
```

IDOR (Insecure Direct Object Reference) berarti server memercayai pengidentifikasi yang Anda kendalikan alih-alih memeriksa *izin Anda* pada objek tersebut. Kebiasaannya: temukan setiap ID, token, dan nilai peran di aplikasi — lalu tambah, kurang, tukar, atau hapus nilainya. Insting yang sama mencakup `role=user` → `role=admin`, `?admin=false` → `?admin=true`, dan parameter nama file.

## Penyiapan Burp Suite (catatan singkat)

Burp Suite **Community Edition** gratis dan cukup untuk pekerjaan CTF:

1. Pasang Burp, buat sebuah project, dan catat proxy listenernya: `127.0.0.1:8080` (default-nya).
2. Arahkan browser Anda ke situ — gunakan browser bawaan Burp (tanpa penyiapan) atau proxy sistem Anda.
3. Untuk target HTTPS, kunjungi `http://burpsuite` di browser yang di-proxy dan pasang sertifikat CA agar request muncul alih-alih error.
4. Biarkan **Intercept off**, telusuri tantangan dengan normal, lalu kerjakan dari **Proxy → HTTP history** ke **Repeater** (Ctrl+R), di mana Anda mengedit dan memutar ulang request satu variabel pada satu waktu.

Aturan cakupan: atur proxy scope ke host tantangan agar Anda tidak pernah tidak sengaja menangkap — atau menyentuh — apa pun yang lain.

## Skripkan bagian yang membosankan (Python `requests`)

Mengklik manual sebuah parameter melalui 200 nilai adalah cara flag terlewat. Satu kebiasaan menyelesaikannya — saat Anda melihat pengulangan, skripkan:

```python
import requests

base = "http://challenge.local/api/notes"

# IDOR sweep: 200 requests in seconds
for i in range(1, 201):
    r = requests.get(base, params={"id": i})
    if "flag{" in r.text:
        print(f"id={i}: {r.text.strip()}")
        break
```

Idema yang sama mencakup brute-force pergeseran Caesar, mencoba wordlist terhadap login, atau mengekstrak data satu boolean pada satu waktu. Jika Anda melakukannya dua kali secara manual, lakukan sekali dengan skrip.

## Checklist pendekatan pertama

- [ ] Baca deskripsi lengkap dan file yang disertakan (petunjuk sering bersembunyi di sana)
- [ ] View source: komentar HTML, field tersembunyi, logika JS
- [ ] `robots.txt`, `sitemap.xml`, path yang jelas (`/flag`, `/admin`)
- [ ] Klik seluruh aplikasi; petakan setiap halaman, form, parameter, cookie
- [ ] Proxy dengan Burp; pindahkan request menarik ke Repeater
- [ ] Manipulasi: ID, peran, cookie, header, metode HTTP
- [ ] Suntikkan: `'`, `{{7*7}}`, `../`, `<script>alert(1)</script>`
- [ ] Periksa localStorage dan cookie untuk token/peran/flag
- [ ] Jika muncul pengulangan → tulis skrip Python-nya
- [ ] Baca ulang deskripsi — kadang flag adalah endpoint yang Anda lewatkan

## Ke mana melangkah lebih jauh

Panduan ini membahas pola-pola bercita rasa CTF. Untuk disiplinnya secara lengkap — metodologi, pemetaan OWASP Top 10, alur kerja Burp, dan latihan lab — baca artikel pendamping **[CSIX Pentest: Web Exploitation](https://siedysoes.github.io/csix-pentest/#/en/exploitation/web-exploitation)**.

## Lihat juga

- [Kategori CTF](../fundamentals/ctf-categories.md) — di mana web berada di papan skor
- [Memulai di CTF](../fundamentals/getting-started.md) — alur kerja acara yang memuat ini
- [Tantangan Kriptografi](crypto.md) — kategori ramah pemula yang lain
- [Toolkit CTF](../practice/toolkit.md) — Burp, Python, dan sisanya