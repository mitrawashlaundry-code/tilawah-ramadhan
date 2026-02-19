# 📖 Target Tilawah Ramadhan

> Aplikasi web progresif (PWA) untuk melacak target khatam Al-Qur'an selama Ramadhan — gratis, offline, tanpa instalasi.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PWA](https://img.shields.io/badge/PWA-Ready-blue)](https://tilawah.pages.dev)
[![Version](https://img.shields.io/badge/version-1.0.0-green)](#)
[![HTML](https://img.shields.io/badge/HTML-Single%20File-orange)](#)

---

## ✨ Fitur Utama

| Fitur | Keterangan |
|-------|-----------|
| 🧮 **Kalkulasi Otomatis** | Target harian, per waktu sholat, dan proyeksi khatam dihitung real-time |
| 📊 **Insight Personal** | Waktu terbanyak membaca, hari terkonsisten, tren bacaan |
| 🔥 **Streak & Badge** | Pantau konsistensi harian + 6 pencapaian yang bisa diraih |
| 🏃 **Kalkulator Kejar Setoran** | Hitung berapa halaman/hari yang dibutuhkan untuk mengejar ketertinggalan |
| 📈 **Grafik & Riwayat** | Grafik batang 30 hari + ringkasan mingguan + riwayat Ramadhan tahun lalu |
| 🎨 **Kartu Progres** | Share progres ke WhatsApp dalam bentuk kartu gambar |
| 🔔 **Pengingat** | Atur pengingat tilawah harian (menggunakan Notification API) |
| 🔡 **Aksesibilitas** | 3 ukuran huruf (Normal / Besar / Sangat Besar) untuk lansia |
| 📲 **Installable PWA** | Bisa dipasang di homescreen Android & iPhone tanpa app store |
| 🔒 **100% Offline & Privat** | Semua data tersimpan di perangkat — tidak dikirim ke mana pun |

---

## 🚀 Demo Langsung

👉 **[Buka Aplikasi](https://tilawah.pages.dev)** *(ganti dengan URL GitHub Pages kamu)*

Atau unduh file `index.html` dan buka langsung di browser — tidak perlu server!

---

## 📸 Screenshot

| Welcome | Setup | Beranda | Catat |
|---------|-------|---------|-------|
| *Setup wizard 3 langkah* | *Target khatam 1–5×* | *Progress ring & insight* | *Input cepat per waktu sholat* |

---

## 🛠️ Cara Pakai

### Opsi 1: Langsung di Browser
1. Unduh file [`index.html`](index.html)
2. Buka di browser (Chrome/Firefox/Safari)
3. Klik "✨ Mulai Penyusunan Target"

### Opsi 2: Pasang di HP (Recommended)
**Android Chrome:**
- Buka URL → Menu ⋮ → "Tambahkan ke layar utama"

**iPhone Safari:**
- Buka URL → Tombol Bagikan ⬆ → "Tambahkan ke Layar Utama"

### Opsi 3: Jalankan Lokal dengan Python
```bash
git clone https://github.com/USERNAME/tilawah-ramadhan.git
cd tilawah-ramadhan
python3 -m http.server 8000
# Buka http://localhost:8000
```

---

## 📐 Arsitektur Teknis

```
index.html          ← Seluruh aplikasi dalam 1 file
│
├── CSS (inline)    ← Dark theme, CSS variables, responsive
├── HTML (inline)   ← 5 section + wizard + modals
└── JS (inline)     ← Vanilla JS, localStorage, Canvas API
```

**Stack:**
- **Frontend:** HTML5 + CSS3 + Vanilla JavaScript (ES6+)
- **Storage:** localStorage (key: `tilawahV3`)
- **Font:** Google Fonts — Amiri (display) + DM Sans (body)
- **Build:** Tidak ada — langsung buka HTML-nya!
- **Dependencies:** Tidak ada library eksternal

**Cara kerja data:**
```javascript
// Struktur data tersimpan di localStorage
{
  userName: "Eko",
  startDate: "2026-02-19",      // 1 Ramadhan
  khatamTarget: 1,               // target berapa kali khatam
  totalTarget: 604,              // total halaman (khatamTarget × 604)
  dailyTarget: 21,               // ceil(totalTarget / 30)
  totalRead: 0,                  // dihitung ulang dari logs
  dailyLogs: {                   // log per hari
    "2026-02-19": [
      { id: 1708..., prayer: "Subuh", pages: 4, pos: "Al-Baqarah 1", time: "05:30" }
    ]
  },
  breakdown: { Subuh: 4, Dhuha: 3, ... },  // target per waktu sholat
  earnedBadges: ["first_log"],
  yearHistory: []                // riwayat Ramadhan tahun lalu
}
```

---

## 📊 Logika Progress Ring

Progress ring menampilkan **dua lapisan informasi**:

- **Angka utama (%)** = `totalRead / (daysGone × dailyTarget)` — seberapa on-track hari ini
- **Sub-label** = `totalRead / totalTarget` — progres keseluruhan Ramadhan
- **Warna ring** = Hijau (≥100%), Biru (≥80%), Amber (<80%) berdasarkan on-track %

Ini mencegah progress ring terlihat "2%" di awal Ramadhan padahal target memang baru dimulai.

---

## 🗂️ Struktur Repository

```
tilawah-ramadhan/
├── index.html              ← Aplikasi utama (satu file lengkap)
├── README.md               ← Dokumentasi ini
├── LICENSE                 ← MIT License
├── CHANGELOG.md            ← Catatan perubahan versi
├── CONTRIBUTING.md         ← Panduan kontribusi
└── docs/
    └── prompt-replikasi.md ← Prompt untuk mereplikasi/mengembangkan app
```

---

## 🤝 Kontribusi

Kontribusi sangat disambut! Lihat [CONTRIBUTING.md](CONTRIBUTING.md) untuk panduan lengkap.

**Cara cepat:**
1. Fork repository ini
2. Buat branch baru: `git checkout -b fitur/nama-fitur`
3. Edit `index.html`
4. Commit: `git commit -m "feat: tambah fitur X"`
5. Push & buat Pull Request

---

## 📝 Changelog

Lihat [CHANGELOG.md](CHANGELOG.md) untuk riwayat lengkap perubahan.

**v1.0.0** (2026-02-19)
- 🎉 Rilis pertama
- Setup wizard 3 langkah
- 5 tab: Beranda, Catat, Riwayat, Bagikan, Pengaturan
- Progress ring dengan metrik on-track harian
- Estimasi khatam capped dalam bulan Ramadhan
- 2-tap confirm untuk hapus log
- FAB sheet untuk input cepat

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE) — bebas digunakan, dimodifikasi, dan didistribusikan.

---

## 🌙 Doa & Harapan

> *"Dan bacalah Al-Qur'an itu dengan tartil (perlahan-lahan)."*
> — QS. Al-Muzzammil: 4

Semoga aplikasi ini membantu kita semua meraih khatam di bulan Ramadhan yang penuh berkah. Aamiin.

---

<p align="center">
  Dibuat dengan ❤️ untuk umat Muslim di seluruh dunia<br>
  <strong>Gratis selamanya • Offline • Data tidak dikirim ke mana pun</strong>
</p>
