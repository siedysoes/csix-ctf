# Tantangan Reverse Engineering

Tantangan reverse engineering (rev) menyerahkan kepada Anda sebuah program yang sudah dikompilasi — sering berupa "crackme" kecil — dan meminta Anda memulihkan flag, password, atau logika yang disembunyikannya. Ini penting karena membaca binary orang lain adalah keterampilan inti: ia mengajarkan Anda bagaimana program benar-benar berperilaku saat kode sumbernya hilang. Semua di sini berlaku untuk binary yang diberikan kepada Anda oleh sebuah acara, platform, atau lab Anda sendiri.

## Alur kerja rev di CTF

```
file chall                     # 1. identify: ELF? PE? bytecode? archive in disguise?
strings chall | grep -i flag   # 2. cheap wins: embedded literals
./chall                        # 3. run it: what does it ask for?
ls                             # 4. check for given source, hints, or a flag file
ghidraRun                      # 5. import, auto-analyze, open the decompiler
                               # 6. find main, read the check, understand the compare
                               # 7. extract or derive the flag, submit it
```

Kerjakan langkah murahnya dulu. Banyak tantangan rev pemula selesai hanya dengan `strings`, dan menjalankan binary memberi tahu Anda apa sebenarnya yang diminta program itu.

### Sepuluh detik pertama

| Perintah | Apa yang diberitahukannya |
|---|---|
| `file chall` | Format dan arsitektur (ELF x86-64, PE, Java class, ZIP dengan ekstensi yang salah) |
| `strings chall \| grep -i flag` | Literal string yang menyebut flag, key, password, correct, wrong |
| `./chall` | Apa yang dimintanya dan bagaimana reaksinya terhadap input (lakukan ini di VM atau sandbox yang Anda kendalikan) |
| `ls -la` + deskripsi tantangan | Apakah kode sumber, `flag.txt`, atau petunjuk ikut disertakan dalam tantangan |

### Strings dulu

Flag sering bocor sebagai literal biasa: penulis mencetak `"Correct! flag{...}"` saat berhasil, dan kompiler menyimpan teks itu di dalam binary. Grep secara luas sebelum menyentuh disassembler:

```bash
strings -n 6 chall | grep -i -E 'flag|key|pass|secret|correct'
strings -n 6 chall | grep -E 'flag\{|CTF\{'
```

Jika flag tidak tergeletak di sana, strings tetap memperlihatkan nama fungsi, pesan error, dan format string yang memberi tahu Anda di mana harus melihat di decompiler.

### Dasar Ghidra untuk CTF

Ghidra adalah opsi gratis yang diasumsikan kebanyakan write-up (unduh dari situs resmi Ghidra; rilis berubah-ubah, jadi verifikasi versi terkininya dan langkah impornya). Loop CTF di dalamnya:

1. **Import** binary tantangan (File > Import File) dan terima default-nya.
2. **Jalankan auto-analysis** saat diminta (ya) — ini membangun nama fungsi dan grafik cross-reference.
3. **Temukan `main`** — buka Symbol Tree, atau pakai Search > Function by Name. Jika binary-nya pembungkus, ikuti apa yang dipanggilnya.
4. **Baca tampilan decompiled** (Window > Decompiler) di samping listing — pseudocode mirip C adalah tempat pemeriksaannya berada.
5. **Ikuti pemeriksaannya**: `strcmp`, `memcmp`, atau loop perbandingan byte. Nilai yang dibandingkan biasanya password Anda atau flag-nya.

Alternatif: IDA Free, Cutter, atau `objdump -d` untuk melihat cepat. Pilih satu dan kuasai.

### Bentuk tantangan yang umum

| Bentuk | Apa yang dilakukan pemeriksaannya | Apa yang Anda lakukan |
|---|---|---|
| Password check | `strcmp(input, expected)` atau loop byte | Baca nilai yang diharapkan langsung dari kode hasil dekompilasi |
| Crackme | Menurunkan kunci dari input dan konstanta | Reproduksi proses penurunannya dalam skrip pendek, berikan flag-nya |
| Bytecode / Python marshal | Logika hidup di bytecode terkompilasi, bukan mesin | Disassemble (mis. bawaan Python `dis`; tool pihak ketiga seperti `xdis` atau `pycdc` — verifikasi mana yang cocok dengan versi Python Anda) |
| Binary packed | Stub unpacker menyembunyikan kode aslinya | Pengantar unpacking: secara konseptual, stub mendekripsi kode asli saat berjalan, jadi Anda biarkan berjalan di lab Anda dan periksa citra hasil unpack — unpacking mendalam adalah keterampilan untuk nanti |

### Membaca sebuah perbandingan

Password check hasil dekompilasi yang khas hampir terlihat seperti source:

```c
// decompiled main()
if (strcmp(input, "s3cr3t") == 0) {
    puts("Correct! flag{...}");
} else {
    puts("Wrong password");
}
```

Versi loop membandingkan per byte — sisi kanan setiap `!=` adalah jawabannya, satu karakter pada satu waktu:

```c
for (int i = 0; i < 16; i++)
    if (input[i] != expected[i]) return fail();
```

### Anti-debug di CTF

Beberapa tantangan mencoba mendeteksi bahwa Anda sedang menganalisisnya: pemeriksaan `ptrace` diri di Linux, `IsDebuggerPresent` di Windows, pemeriksaan timing yang memakai timestamp counter CPU, atau pemeriksaan lingkungan. Di perangkat lunak sungguhan ini bersifat defensif; di CTF ini hanyalah teka-teki lain yang harus Anda sadari. Jika sebuah pemeriksaan memblokir salinan lokal Anda dari binary yang disediakan, memahaminya — dan, di salinan Anda sendiri, melewatkannya agar bisa terus belajar — adalah praktik analisis yang wajar. Patching memang ada, tetapi tujuan belajarnya adalah memahami kode, bukan membangun toolkit evasion.

### Inti x86

| Bagian | Arti |
|---|---|
| `rax` / `eax` / `al` | Register utama; sering menyimpan nilai return |
| `rdi`, `rsi`, `rdx`, `rcx` | Empat argumen integer pertama (System V AMD64 ABI) |
| `rsp` / `rbp` | Stack pointer / frame pointer |
| `rip` | Instruction pointer (di mana eksekusi berlanjut) |
| `mov rax, [rbp-0x10]` | Muat variabel lokal ke dalam register |
| `cmp byte [rax], 0x41` | Bandingkan sebuah byte dengan `'A'` |
| `je` / `jne` | Jump if equal / not equal — cabang tepat setelah sebuah pemeriksaan |

Anda tidak perlu menulis assembly untuk memecahkan tantangan rev pemula; Anda perlu mengenali beberapa pola ini dan mengikuti perbandingannya.

### Petunjuk latihan

Tempat awal yang baik: crackmes.one (crackmes komunitas, verifikasi apa yang saat ini tersedia), tantangan reverse-engineering picoCTF, dan wargame OverTheWire (Behemoth adalah seri yang condong ke binary — periksa daftar wargame terkini sebelum memulai). Selalu baca deskripsi tantangan terlebih dahulu; itu adalah bagian dari teka-teki.

Hanya reverse binary yang diberikan kepada Anda — tugas kompetisi, tantangan platform, atau program di lab Anda sendiri. Berlatih pada perangkat lunak orang lain tanpa izin bukan reverse engineering, itu akses tanpa izin.

## Lihat juga

- [Kategori CTF](../fundamentals/ctf-categories.md)
- [Tantangan Binary Exploitation](pwn.md)
- [Platform Latihan](../practice/platforms.md)
- [Toolkit CTF](../practice/toolkit.md)