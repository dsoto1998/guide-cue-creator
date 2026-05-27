# Guide Cue Creator

Membaca sesi Ableton Live (`.als`), mencocokkan locator dengan file cue audio, dan menghasilkan file WAV stereo tunggal (`guide_cues.wav`) yang siap dimasukkan ke track cue panduan khusus pada beat 0.

---

## Yang Disertakan

```
cue_creator.html    ← aplikasinya (buka file ini)
GUIDE CUES/         ← perpustakaan cue audio (sudah disertakan)
```

---

## Persyaratan

- **Chrome atau Edge** (direkomendasikan) — akses folder tanpa klik tambahan setelah pertama kali
- **Firefox** — berfungsi, pilih ulang folder sekali per sesi browser jika diperlukan
- Koneksi Internet (memuat pako, Fuse.js, Dexie dari CDN)

---

## Mulai Cepat

1. Klik dua kali `cue_creator.html` — terbuka di browser default Anda
2. Klik **Pilih Folder** dan pilih folder yang berisi `GUIDE CUES/`
3. Pilih **bahasa** dari menu tarik-turun
4. Seret file `.als` Anda ke area unggah (atau klik untuk mencari)
5. Tinjau tabel cue — hijau = cocok, kuning = kepercayaan rendah, merah = tidak cocok
6. Klik **Pratinjau** (atau tekan **Spasi**) untuk mendengarkan
7. Klik **Render WAV** — mengunduh `guide_cues.wav`
8. Seret `guide_cues.wav` ke sesi Ableton Anda pada beat 0 di track khusus

Di Chrome/Edge, pilihan folder diingat. Langkah 2 hanya perlu dilakukan sekali.

---

## Yang Dilakukan Aplikasi

- Menempatkan setiap cue bagian **1 bar sebelum** locator-nya
- Menghasilkan **hitungan masuk** untuk 2 bar pertama (beat 0–7 untuk 4/4)
- Menekan cue bagian yang jatuh di dalam wilayah hitungan masuk
- Melewati locator bernama `count off` atau `next song`
- Mendukung otomasi tempo (sesi BPM variabel)

---

## Tanda Waktu yang Didukung

3/4 · 4/4 · 6/8 · 12/8

---

## Bahasa yang Didukung

Inggris · Spanyol · Prancis · Indonesia · Korea · Filipina · Portugis

---

## Kontrol Tabel Cue

| Kolom | Yang Harus Dilakukan |
|-------|----------------------|
| Locator | Hanya baca — dari file .als Anda |
| Cue yang Cocok | Edit untuk mengganti pencocokan otomatis |
| Kepercayaan | Hijau ≥ 80% · Kuning ≥ 50% · Merah < 50% |

---

## Kontrol Timeline

| Aksi | Hasil |
|------|-------|
| Klik | Pindah ke posisi |
| Spasi | Putar / Jeda |
| Ctrl+gulir atau cubit | Zoom ke arah kursor |
| Tombol +/− | Perbesar/perkecil |
| Reset | Sesuaikan seluruh sesi dengan lebar |
| Ikuti Playhead | Aktifkan/nonaktifkan gulir otomatis |

---

## Spesifikasi Output

| Pengaturan | Nilai |
|------------|-------|
| Frekuensi sampel | 48.000 Hz |
| Saluran | Stereo |
| Kedalaman bit | PCM 16-bit |
| File | `guide_cues.wav` |
