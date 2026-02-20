# 📖 Target Tilawah Ramadhan

> Aplikasi web progresif (PWA) untuk melacak target khatam Al-Qur'an selama Ramadhan — gratis, offline, tanpa instalasi.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PWA](https://img.shields.io/badge/PWA-Ready-blue)](https://mitrawashlaundry-code.github.io/tilawah-ramadhan/)
[![Version](https://img.shields.io/badge/version-1.3.0-green)](#)
[![HTML](https://img.shields.io/badge/HTML-Single%20File-orange)](#)

---

## 🚀 Demo Langsung

👉 **[Buka Aplikasi — mitrawashlaundry-code.github.io/tilawah-ramadhan](https://mitrawashlaundry-code.github.io/tilawah-ramadhan/)**

Atau unduh file `index.html` dan buka langsung di browser — tidak perlu server, tidak perlu koneksi internet!

---

## ✨ Fitur Utama

| Fitur | Keterangan |
|-------|-----------|
| 🧮 **Kalkulasi Otomatis** | Target harian, per waktu sholat, dan proyeksi khatam dihitung real-time |
| 🌅 **Distribusi Cerdas per Sholat** | Subuh mendapat porsi terbesar; Maghrib dibatasi 6 hal (waktu singkat + berbuka) |
| 📊 **Insight Personal** | Waktu terbanyak membaca, hari terkonsisten, tren bacaan |
| 🔥 **Streak & Badge** | Pantau konsistensi harian + 6 pencapaian yang bisa diraih |
| 🏃 **Kalkulator Kejar Setoran** | Hitung berapa halaman/hari untuk mengejar ketertinggalan |
| 📈 **Grafik & Riwayat** | Grafik batang 30 hari + ringkasan mingguan + riwayat Ramadhan tahun lalu |
| 🎨 **Kartu Progres** | Share progres ke WhatsApp dalam bentuk kartu gambar |
| 🔔 **Pengingat Fleksibel** | Tambah, aktifkan/nonaktifkan, dan **hapus** pengingat tilawah harian |
| 🔡 **Aksesibilitas** | 3 ukuran huruf (Normal / Besar / Sangat Besar) — berlaku menyeluruh |
| 🌙 **Ikon Homescreen** | Ikon bulan sabit emas otomatis muncul saat dipasang ke homescreen |
| 📲 **Installable PWA** | Bisa dipasang di homescreen Android & iPhone tanpa app store |
| 🔒 **100% Offline & Privat** | Semua data tersimpan di perangkat — tidak dikirim ke mana pun |

---

## 📲 Pasang di HP

**Android Chrome:**
1. Buka URL di Chrome → tap menu ⋮ → "Tambahkan ke layar utama" → "Tambah"
2. Ikon bulan sabit emas akan muncul di homescreen

**iPhone Safari:**
1. Buka URL di Safari → tap tombol Bagikan ⬆ → "Tambahkan ke Layar Utama" → "Tambah"

---

## 🔄 Update dari Android (Tanpa PC)

Lihat panduan lengkap di **[docs/CARA-UPDATE-ANDROID.md](docs/CARA-UPDATE-ANDROID.md)**

Ringkasan:
1. Edit `index.html` di GitHub (via browser atau GitHub Mobile)
2. Commit ke branch `main` → GitHub Pages rebuild ±1–2 menit
3. Buka aplikasi → Hard Refresh (⋮ → Muat ulang)

---

## 📐 Arsitektur Teknis

```
index.html          ← Seluruh aplikasi dalam 1 file
├── CSS (inline)    ← Dark theme, CSS variables, font-size toggle 3 level
├── HTML (inline)   ← 5 section + wizard + modals + overlays
└── JS (inline)     ← Vanilla JS, localStorage, Canvas API
```

**Logika distribusi target per sholat (v1.3):**
```
Bobot: Subuh=2.5, Dhuha=1, Dzuhur=1, Ashar=1, Maghrib=1.5, Tarawih=2
Cap:   Maghrib maksimal 6 halaman → kelebihan ke Tarawih
Hasil: Subuh ≈ 28%, Tarawih ≈ 22%, Maghrib ≤ 6 hal
```

---

## 🗂️ Struktur Repository

```
tilawah-ramadhan/
├── index.html                        ← Aplikasi utama (satu file lengkap)
├── README.md                         ← Dokumentasi ini
├── LICENSE                           ← MIT License
├── CHANGELOG.md                      ← Catatan perubahan versi
├── CONTRIBUTING.md                   ← Panduan kontribusi
└── docs/
    ├── prompt-replikasi-v1_3.md      ← Prompt lengkap untuk replikasi/pengembangan
    └── CARA-UPDATE-ANDROID.md        ← Panduan update dari Android tanpa PC
```

---

## 📝 Changelog Singkat

Lihat [CHANGELOG.md](CHANGELOG.md) untuk riwayat lengkap.

**v1.3.0** (2026-02-20)
- 🌅 Subuh mendapat porsi terbesar (bobot 2.5×)
- 🗑️ Fitur hapus pengingat dengan konfirmasi
- 🌙 Ikon bulan sabit otomatis di homescreen
- 🔡 Penjelasan aksesibilitas huruf + override CSS tambahan

**v1.2.0** (2026-02-20) — Cap Maghrib 6 hal, font toggle 40+ kelas CSS

**v1.1.0** (2026-02-19) — 8 bug kritis diperbaiki, backup/restore JSON

**v1.0.0** (2026-02-19) — 🎉 Rilis pertama

---

## 📄 Lisensi

[MIT License](LICENSE) — bebas digunakan, dimodifikasi, dan didistribusikan.

---

## 🌙 Doa & Harapan

> *"Dan bacalah Al-Qur'an itu dengan tartil (perlahan-lahan)."*
> — QS. Al-Muzzammil: 4

Semoga aplikasi ini membantu kita semua meraih khatam di bulan Ramadhan yang penuh berkah. Aamiin.

---

<p align="center">
  Dibuat dengan ❤️ untuk umat Muslim di seluruh dunia<br>
  <strong>Gratis selamanya • Offline • Data tidak dikirim ke mana pun</strong><br>
  <a href="https://mitrawashlaundry-code.github.io/tilawah-ramadhan/">mitrawashlaundry-code.github.io/tilawah-ramadhan</a>
</p>
