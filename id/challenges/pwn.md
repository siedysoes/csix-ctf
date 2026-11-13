# Tantangan Binary Exploitation

Tantangan pwn memberi Anda sebuah program — sering dengan cara menjalankannya berulang kali — dan meminta Anda membuatnya melakukan sesuatu yang tidak dibangun untuk melakukannya, seperti memanggil fungsi tersembunyi atau membaca file flag yang enggan disentuhnya. Ini penting karena binary exploitation adalah tempat Anda mempelajari bagaimana memori, calling convention, dan proteksi benar-benar saling berinteraksi. Semua di bawah terjadi di dalam sandbox atau layanan yang disediakan acara untuk Anda.

## Mengapa buffer meluap

Banyak fungsi C menyalin input ke dalam buffer berukuran tetap tanpa pemeriksaan panjang (`gets`, `scanf("%s")`, `strcpy`). Di stack, buffer lokal duduk di bawah data bookkeeping yang dibutuhkan fungsi untuk kembali:

```
higher addresses
+----------------------+
| return address       |  <- where execution resumes after the function returns
+----------------------+
| saved RBP            |  <- caller's frame pointer
+----------------------+
| other locals         |
| buf[64]              |  <- your input lands here and keeps growing upward
+----------------------+
lower addresses
```

Input yang lebih panjang dari `buf` berjalan melewati akhir array, menimpa frame pointer tersimpan, lalu return address. Tidak ada yang menghentikannya sampai penyalinan selesai.

## Stack smash klasik, secara konseptual

Telusuri apa arti sebuah penimpaan, langkah demi langkah:

1. Program membaca lebih banyak byte daripada yang muat di buffer.
2. Byte ekstra menutupi `saved RBP`, lalu **return address**.
3. Saat fungsi mengeksekusi `ret`, CPU mem-pop nilai yang tertimpa itu ke dalam instruction pointer.
4. Eksekusi karena itu berlanjut ke mana pun yang dipilih penyerang — di CTF, biasanya ke tempat yang sudah ada di dalam binary, seperti fungsi `win()` yang mencetak flag.

Keterampilan CTF adalah mengukur jarak yang tepat ke return address (tool seperti cyclic pattern membuatnya aman dan mekanis) dan memutuskan ke mana eksekusi harus pergi sebagai gantinya. Ini adalah konsep yang harus dipahami pada binary tantangan yang diberikan kepada Anda — tujuannya adalah mencapai jalur kode yang dimaksudkan penulis untuk Anda capai.

## Proteksi modern

| Proteksi | Apa yang diblokirnya | Konsep level CTF (kerangka belajar) |
|---|---|---|
| NX / DEP | Menjalankan shellcode yang ditanam di stack | ret2win: gunakan ulang kode yang sudah dimiliki binary alih-alih mengeksekusi milik Anda sendiri |
| Stack canary | Penimpaan return address yang senyap — sebuah nilai acak diperiksa saat fungsi kembali | Canary harus diketahui atau dibocorkan terlebih dahulu; banyak tantangan pemula dikirim tanpa satu (verifikasi dengan checksec) |
| ASLR | Alamat hard-coded stack dan library | Bocorkan sebuah alamat, atau brute-force hanya di tempat yang mengizinkan percobaan lokal tanpa batas |
| PIE | Alamat hard-coded binary itu sendiri | Idema yang sama: bocorkan alamat dasar, atau gunakan referensi relatif; beberapa tantangan menonaktifkannya |

`checksec` memberi tahu Anda yang mana dari ini yang aktif sebelum Anda merencanakan apa pun.

## Alur kerja pwn di CTF

```
checksec --file=chall          # or: pwn checksec chall — which protections are on?
# open the binary in a decompiler (see the reversing guide)
# find the vulnerable function: gets, strcpy, sprintf with no bound, off-by-one reads
# plan: what SHOULD the program do? (e.g. reach a win() that exists in the binary)
# script the payload with pwntools, then test it against the provided instance
```

Rencanakan sebelum Anda mengetik. Mengetahui tujuan (fungsi mana yang harus dicapai, proteksi mana yang hilang) mengubah payload menjadi tugas rekayasa alih-alih tebakan.

## Kerangka pwntools

```python
from pwn import *
# connect to the challenge instance provided by the event
p = remote("challenge.example", 1337)
# ... build payload for the given challenge ...
p.interactive()
```

Dua kebiasaan aman yang layak dibangun sekarang:

- `cyclic(100)` menghasilkan pola yang mudah dikenali; `cyclic_find()` mengubah nilai yang crash kembali menjadi offset. Itulah cara Anda mengukur jarak ke return address tanpa menebak.
- Jaga skrip tetap lokal dan bisa direproduksi: satu file per tantangan, komentar tentang tujuan setiap field.

Ini adalah templat, bukan exploit yang berfungsi — setiap tantangan menyambungkannya secara berbeda.

## Dasar gdb

| Perintah | Fungsinya |
|---|---|
| `gdb ./chall` | Mulai binary di bawah debugger |
| `break main` | Berhenti saat `main` dimulai |
| `run < input.txt` | Jalankan dengan input yang sudah disiapkan |
| `info registers` | Tampilkan register saat ini |
| `disassemble main` | Tampilkan kode mesin dari `main` |
| `x/32gx $rsp` | Dump 32 slot stack — perhatikan penimpaannya terjadi |
| `quit` | Keluar |

Dengan ekstensi remote-debugging di gdb (`gef`, `pwndbg`, atau `gdb-peda` — verifikasi mana yang Anda pasang), tampilan stack menjadi jauh lebih mudah dibaca.

## Di mana berlatih

Kategori pwn picoCTF adalah tujuan pertama yang biasa, wargame Narnia OverTheWire adalah seri latihan stack klasik (Bandit membahas dasar-dasar Unix terlebih dahulu — verifikasi ketersediaan wargame saat ini), dan Hack The Box meng-host mesin dan tantangan bergaya pwn. Mulai di tempat tantangan memberi Anda binary dan salinan lokal untuk di-debug.

Hanya eksploit instance yang diserahkan kepada Anda: endpoint remote di deskripsi tugas, container Docker yang dikirim acara, atau binary yang berjalan di lab Anda sendiri. Menyerang host mana pun di luar cakupan keterampilan ini — dan di luar batas secara hukum.

## Lihat juga

- [Tantangan Reverse Engineering](reversing.md)
- [Kategori CTF](../fundamentals/ctf-categories.md)
- [Platform Latihan](../practice/platforms.md)
- [Toolkit CTF](../practice/toolkit.md)