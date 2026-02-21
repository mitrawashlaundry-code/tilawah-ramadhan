# Changelog — Target Tilawah Ramadhan

---

## [v1.5.1] — 2026-02-21

### 🐛 Bug Fixed

- **[BUG-012]** Tombol "Tutup" pada modal syarat badge tidak berfungsi — `showBadgeInfo()` memanggil `showModal()` dengan `fn1 = null` sehingga `onclick` handler tidak terpasang. Diperbaiki dengan mengganti `null` menjadi `closeModal`

---

## [v1.5.0] — 2026-02-21

### 🐛 Bug Fixed

- **[BUG-001]** `showModal()` menerima string sebagai callback (`new Function(fn)`) — rentan XSS, diganti dengan function reference langsung
- **[BUG-002]** FAB input sheet hardcoded ke hari ini — sekarang punya date picker sendiri, bisa dipakai untuk backfill
- **[BUG-003]** Streak counter tidak konsisten antara `renderStreakBar()` dan `checkBadges()` — disatukan ke helper `getStreakCount(mode)`
- **[BUG-004]** CSV export: hanya field "Posisi Baca" yang di-escape — sekarang semua field dilewatkan `csvEscape()` (aman di Excel)
- **[BUG-005]** Memory leak: `URL.createObjectURL()` di `backupData()` tidak pernah di-`revoke` — diperbaiki
- **[BUG-008]** Setup wizard bisa lanjut dengan nama kosong — sekarang ada validasi real-time + pesan error inline
- **[BUG-011]** Celebration overlay tidak punya tombol tutup yang efektif dan tidak auto-dismiss — sekarang auto-close setelah 12 detik

### ✨ Fitur Baru

- **Progressive Disclosure** — Beranda kini hanya tampilkan ring + status + CTA di atas fold. Streak, insight, badge, pie chart masuk ke dalam toggle "Lihat Statistik Lengkap ▼"
- **Smart Prayer Suggestion** — Waktu sholat otomatis dipilih saat buka form, berdasarkan jam saat ini (Subuh 03–07, Dhuha 07–11, Dzuhur 11–15, Ashar 15–18, Maghrib 18–20, Tarawih malam)
- **Khatam Info Box di Wizard** — Penjelasan "Apa itu Khatam?" + estimasi menit baca harian muncul di step 2 wizard
- **Date Picker di FAB Sheet** — Input sheet via FAB sekarang bisa pilih tanggal, bukan hanya hari ini
- **Locked Nav Tooltip** — Klik tab yang terkunci menampilkan tooltip floating dengan tombol "Mulai Setup →" (sebelumnya hanya toast yang menghilang)
- **Reminder Status Bar** — Menampilkan status izin notifikasi (Diberikan / Ditolak / Belum Diminta) dengan tombol "Izinkan" dan peringatan khusus untuk pengguna iOS non-PWA
- **Pie Chart Distribusi Sholat** — Canvas pie chart distribusi halaman per waktu sholat, muncul otomatis jika ada data
- **Badge Info Modal** — Klik badge menampilkan syarat lengkap untuk meraihnya
- **Total Aktual di Breakdown** — Total halaman aktual per sholat ditampilkan di bawah angka target

### 🎨 Perubahan Tampilan

- **Font minimum 11px** — Semua elemen dinaikkan ke minimum 0.69rem: nav label (9.3px→11.1px), badge name, header chips, log position
- **Streak dots diperbesar** — 16×16px → 24×24px; label teks di dalam dot dihapus; legenda warna ditambahkan di bawah
- **Backup/Restore dipindah** — Dari tab Bagikan ke tab Pengaturan; label diubah menjadi "Simpan Data" dan "Pulihkan Data"
- **Delete log inline confirm** — Tombol hapus log minta konfirmasi klik kedua, bukan langsung hapus
- **Footer versi** — Diperbarui ke v1.5.0

### 🔒 Keamanan

- Semua modal action button memakai `addEventListener`, bukan string di `onclick`
- Autocomplete dropdown memakai event delegation, bukan `onmousedown` inline di `innerHTML`

---

## [v1.4.0] — 2026-02-21

### ✨ Fitur Baru

- **Export CSV** — Unduh riwayat tilawah ke Excel/Google Sheets dalam dua mode: Detail (per sesi) dan Harian (ringkasan 30 hari)
- **Trend Mingguan** — Bar chart mini rata-rata baca per minggu dengan indikator naik/turun/stabil dan garis target
- **Autocomplete Surah** — Input posisi baca kini punya dropdown autocomplete dari 114 surah; navigasi keyboard ↑↓ Enter Esc
- **Target Sholat Manual** — Tombol "✏️ Ubah" di card breakdown untuk atur target per sholat secara manual, dengan tombol "Reset ke otomatis"
- **Riwayat Ramadhan** — Kartu year-over-year di tab Riwayat, simpan rekap khatam tahun sebelumnya

### 🐛 Bug Fixed

- Progress ring warna tidak sinkron dengan status on-track/behind
- Streak bar error di hari pertama Ramadhan sebelum ada log apapun

---

## [v1.3.0] — 2026-02-20

### ✨ Fitur Baru

- **Prioritas Subuh** — Bobot Subuh dinaikkan ke 2.5× dalam distribusi target (sebelumnya sama dengan sholat lain)
- **Hapus Pengingat** — Tombol hapus individual per item pengingat
- **Ikon Homescreen** — SVG bulan sabit emas inline, tidak perlu file gambar terpisah
- **Badges** — 6 pencapaian: Langkah Pertama, Konsisten 3 Hari, Seminggu Penuh, Setengah Jalan, Khatam Pertama, On Track
- **Share Progress Card** — Canvas 700×440px berisi nama, %, khatam, link app; bisa diunduh atau dibagikan via Web Share API
- **Backfill Banner** — Banner otomatis muncul di tab Catat jika ada hari yang belum tercatat

### 🎨 Perubahan Tampilan

- Redesain header dengan mini progress bar harian

---

## [v1.2.0] — 2026-02-20

### ✨ Fitur Baru

- **Cap Maghrib** — Waktu Maghrib dibatasi maksimal 6 halaman (berbuka puasa singkat); kelebihan otomatis dialihkan ke Tarawih
- **Aksesibilitas Huruf** — Toggle 3 ukuran teks (Normal / Besar / Sangat Besar), berlaku untuk 40+ elemen via class `font-md` dan `font-lg`
- **Pengingat Tilawah** — Notifikasi terjadwal per waktu sholat, toggle aktif/nonaktif
- **Install PWA Prompt** — Tombol "Pasang Sekarang" muncul otomatis dari `beforeinstallprompt` event

### 🐛 Bug Fixed

- localStorage tidak tersimpan di mode private browser — ditangani dengan try/catch tanpa crash

---

## [v1.1.0] — 2026-02-19

### ✨ Fitur Baru

- **Backup & Restore** — Ekspor data ke file JSON, impor untuk pindah HP atau cadangan
- **Backfill** — Bisa input catatan untuk tanggal sebelumnya lewat date picker di tab Catat
- **Kalkulator Kejar Setoran** — Input sisa hari → kalkulasi halaman/hari yang dibutuhkan + saran
- **FAB (Floating Action Button)** — Tombol catat mengambang, akses cepat tanpa pindah tab
- **Quick Input Buttons** — Tombol pilih cepat: 2 hal / 4 hal / 8 hal / 1 Juz
- **Font Size Toggle** — 3 level ukuran huruf (diperkenalkan awalnya)

---

## [v1.0.0] — 2026-02-19

Rilis pertama.

### Fitur
- Setup wizard 3 langkah: nama, target khatam (1–5×), tanggal Ramadhan
- Kalkulasi target harian otomatis berdasarkan bobot per waktu sholat
- 5 tab: Beranda, Catat, Riwayat, Bagikan, Pengaturan
- Progress ring SVG dengan gradient dinamis
- Status banner: Ahead / On-Track / Behind
- Streak bar 30 hari
- Statistik: rata-rata, estimasi khatam, sisa halaman
- Log harian dengan hapus per entri
- Penyimpanan `localStorage` (key: `tilawahV3`)
- Dark mode dengan warna emas — tema Islami
- Responsif untuk layar 360–430px
