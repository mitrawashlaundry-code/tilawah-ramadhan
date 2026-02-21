# 📖 Target Tilawah Ramadhan

Aplikasi Progressive Web App (PWA) untuk melacak target tilawah Al-Qur'an selama Ramadhan. Gratis, tanpa instalasi, tanpa akun — semua data tersimpan lokal di perangkat.

[![Versi](https://img.shields.io/badge/versi-1.5.0-gold)](https://github.com/mitrawashlaundry-code/tilawah-ramadhan)
[![PWA](https://img.shields.io/badge/PWA-ready-green)](https://web.dev/progressive-web-apps/)
[![Lisensi](https://img.shields.io/badge/lisensi-MIT-blue)](LICENSE)

🌐 **Live:** https://mitrawashlaundry-code.github.io/tilawah-ramadhan/

---

## ✨ Fitur Utama

| Fitur | Keterangan |
|-------|-----------|
| 🎯 **Target Fleksibel** | Setup 1–5 kali khatam, target harian otomatis dihitung |
| ⏰ **Pembagian per Sholat** | Target per Subuh/Dhuha/Dzuhur/Ashar/Maghrib/Tarawih (bisa diubah manual) |
| ✏️ **Catat Tilawah** | Input per sesi + posisi baca + backfill hari sebelumnya |
| 📝 **Autocomplete Surah** | Ketik nama surah, sistem sarankan dari 114 surah |
| 🕐 **Saran Waktu Sholat** | Waktu sholat otomatis dipilih berdasarkan jam saat ini (v1.5) |
| 📊 **Grafik Harian** | Bar chart 30 hari Ramadhan, klik bar untuk detail |
| 📉 **Trend Mingguan** | Grafik naik/turun rata-rata per minggu |
| 🔥 **Streak** | Dot chart konsistensi harian — diperbesar & lebih terbaca (v1.5) |
| 🥧 **Pie Chart Sholat** | Distribusi tilawah per waktu sholat (v1.5) |
| 📈 **Insight Otomatis** | Waktu favorit, hari terkonsisten, proyeksi khatam |
| 🏅 **Badge** | 6 pencapaian — klik untuk lihat syarat (v1.5) |
| 🎨 **Kartu Progres** | Share kartu bergambar ke WhatsApp/Instagram |
| 📊 **Export CSV** | Unduh data ke Excel/Google Sheets — mode Detail & Harian |
| ☁️ **Simpan & Pulihkan** | Ekspor/impor file JSON untuk pindah HP (v1.5) |
| 🔔 **Pengingat Fleksibel** | Tambah/hapus pengingat, status izin notifikasi jelas (v1.5) |
| 📲 **PWA** | Pasang ke homescreen, ikon bulan sabit emas |
| 🔤 **Aksesibilitas Huruf** | Toggle 3 ukuran teks (Normal/Besar/Sangat Besar) |
| 📚 **Riwayat Ramadhan** | Rekap khatam tahun-tahun sebelumnya |

---

## 🚀 Cara Pakai

1. Buka https://mitrawashlaundry-code.github.io/tilawah-ramadhan/
2. Isi nama, target khatam, dan tanggal mulai Ramadhan
3. Catat tilawah setiap kali selesai baca
4. Pantau progres di tab Beranda dan Riwayat

**Pasang di Homescreen:**
- **Android Chrome:** Menu ⋮ → Tambahkan ke layar utama
- **iPhone Safari:** Bagikan ⬆ → Tambahkan ke Layar Utama

---

## 📁 Struktur File

```
tilawah-ramadhan/
├── index.html          # Aplikasi utama (single-file PWA)
├── CHANGELOG.md        # Riwayat perubahan versi
├── CONTRIBUTING.md     # Panduan kontribusi
├── CARA-UPDATE-ANDROID.md  # Panduan update di Android
├── docs/
│   ├── ARCHITECTURE.md            # Dokumentasi teknis arsitektur
│   └── replikasi-prompt.md        # Prompt untuk replikasi AI
└── README.md
```

---

## 📋 Changelog Singkat

| Versi | Tanggal | Ringkasan |
|-------|---------|-----------|
| **v1.5.0** | 2026-02-21 | Perbaikan UX & bug besar: progressive disclosure, smart prayer, streak redesign, security fix modal, pie chart, badge info, notif status |
| v1.4.0 | 2026-02-21 | Export CSV, Trend Mingguan, Autocomplete Surah, Target Sholat Manual |
| v1.3.0 | 2026-02-20 | Prioritas Subuh, hapus pengingat, ikon homescreen |
| v1.2.0 | 2026-02-20 | Cap Maghrib 6 hal, aksesibilitas huruf 40+ elemen |
| v1.1.0 | 2026-02-19 | Backup/Restore, backfill, kalkulator kejar setoran |
| v1.0.0 | 2026-02-19 | Rilis pertama — wizard, 5 tab, 6 badge, PWA |

---

## 🛠️ Teknologi

- **Vanilla HTML/CSS/JS** — tanpa framework, tanpa dependensi NPM
- **localStorage** — penyimpanan data lokal
- **Canvas API** — render kartu progres & pie chart distribusi sholat
- **Web Share API** — share native di Android/iOS
- **PWA Manifest** — ikon homescreen inline (tanpa file gambar terpisah)

---

## 📄 Lisensi

MIT License — bebas digunakan, dimodifikasi, dan didistribusikan.
