# Changelog

Semua perubahan penting pada proyek ini akan didokumentasikan di file ini.

Format mengikuti [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
dan proyek ini mengikuti [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] - 2026-02-19

### 🎉 Rilis Pertama

#### Ditambahkan
- **Setup Wizard 3 Langkah** — onboarding yang ramah untuk pengguna baru
  - Step 1: Input nama dan tanggal 1 Ramadhan
  - Step 2: Pilih target khatam (1–5×) dengan preview langsung
  - Step 3: Ringkasan + perbandingan vs metode manual
- **Tab Beranda** — dashboard utama dengan:
  - Status banner adaptif (on-track / ahead / behind)
  - Progress ring dengan metrik on-track harian + total target
  - 4 stat card: Dibaca, Tersisa, Estimasi Khatam, Rata-rata Hal/Hari
  - Insight personal (tersedia setelah 3+ hari data)
  - Streak bar 30 hari dengan 5 status visual
  - Target per waktu sholat (breakdown otomatis berdasarkan bobot)
  - 6 badge pencapaian
- **Tab Catat** — input tilawah dengan:
  - Pilih tanggal (bisa input untuk hari lain)
  - Select waktu sholat + input halaman (font besar)
  - Quick input buttons: 2 / 4 / 8 hal / 1 Juz
  - Input posisi baca (opsional)
  - Log hari ini dengan 2-tap confirm untuk hapus
- **FAB Button** — floating action button untuk input cepat dari tab mana pun
- **Tab Riwayat** — grafik 30 hari, ringkasan mingguan, detail per hari, riwayat multi-tahun
- **Tab Bagikan** — kartu progres canvas + share WhatsApp + download PNG + bagikan app
- **Tab Pengaturan** — ubah target, kalkulator kejar setoran, statistik, pengingat, install PWA
- **Aksesibilitas** — 3 ukuran huruf (Normal / Besar / Sangat Besar), persisten
- **Perayaan Khatam** — overlay konfeti + animasi saat khatam tercapai
- **PWA Support** — installable di Android & iPhone, 100% offline
- **Konversi Hijriyah** — lookup otomatis 1445H–1450H

#### Teknis
- Single file HTML (inline CSS + JS), zero dependencies
- localStorage untuk persistensi data
- Null-safe DOM access via helper `el()`, `setText()`, `setHTML()`
- `_activeInputDate` sebagai single source of truth (bukan DOM)
- Progress ring color-coded (hijau/biru/amber) berdasarkan status on-track
- Estimasi khatam dicap dalam bulan Ramadhan; jika melewati → tampilkan "X hal/hr Kejar Khatam"
- Canvas share dirender hanya saat data berubah (`_shareCanvasDirty` flag)

---

## Versi Pengembangan (Internal, Tidak Dirilis)

### [0.5.0] - Pra-rilis v5 (Internal)
- Perbaikan: log tidak langsung muncul setelah input (bug single source of truth)
- Perbaikan: delete log tidak langsung hilang tanpa pindah tab
- Perbaikan: TypeError null saat updateAllUI dipanggil sebelum mainApp tampil
- Perbaikan: FAB sheet tidak reset saat dibuka ulang
- Perbaikan: bagikan aplikasi hanya download, tidak membuka WA

### [0.4.0] - Pra-rilis v4 (Internal)
- Perbaikan: FAB terlalu besar dan opak (54px → 44px, opacity 0.55)
- Perbaikan: sheet langsung tutup tanpa feedback sukses
- Perbaikan: quick input akumulatif (diubah ke set, bukan tambah)
- Tambah: ukuran huruf persisten via localStorage
- Tambah: tombol perbesar huruf di welcome screen dan wizard

### [0.3.0] - Pra-rilis v3 (Internal)
- Versi fungsional pertama dengan semua 5 tab
- Setup wizard dasar
- localStorage storage

---

## Rencana ke Depan

### [1.1.0] - Direncanakan
- [ ] Export data ke CSV
- [ ] Grafik trend mingguan di tab Riwayat
- [ ] Autocomplete nama surah saat input posisi baca

### [1.2.0] - Direncanakan
- [ ] Service Worker untuk notifikasi push yang benar-benar terjadwal
- [ ] Target per waktu sholat yang bisa diubah manual
- [ ] Mode terang (light theme)

### [2.0.0] - Jangka Panjang
- [ ] Multi-bahasa (EN/AR/MS)
- [ ] Sinkronisasi lintas perangkat (opsional, via kode ekspor/impor)
- [ ] Widget homescreen Android
