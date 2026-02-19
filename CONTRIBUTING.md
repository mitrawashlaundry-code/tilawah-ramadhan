# 🤝 Panduan Kontribusi

Terima kasih telah tertarik untuk berkontribusi pada **Target Tilawah Ramadhan**! 
Setiap kontribusi — sekecil apapun — sangat berarti.

---

## 📋 Daftar Isi

- [Cara Berkontribusi](#cara-berkontribusi)
- [Melaporkan Bug](#melaporkan-bug)
- [Mengusulkan Fitur](#mengusulkan-fitur)
- [Panduan Kode](#panduan-kode)
- [Konvensi Commit](#konvensi-commit)
- [Proses Pull Request](#proses-pull-request)

---

## Cara Berkontribusi

### 1. Fork & Clone
```bash
# Fork dulu di GitHub, lalu clone fork kamu
git clone https://github.com/USERNAME_KAMU/tilawah-ramadhan.git
cd tilawah-ramadhan
```

### 2. Buat Branch
```bash
# Gunakan nama yang deskriptif
git checkout -b fix/bug-log-tidak-muncul
git checkout -b feat/export-csv
git checkout -b docs/update-readme
```

### 3. Kembangkan & Test
```bash
# Jalankan lokal
python3 -m http.server 8000
# Buka http://localhost:8000

# Atau langsung buka file di browser
open index.html
```

### 4. Commit & Push
```bash
git add index.html
git commit -m "fix: log langsung muncul setelah input"
git push origin fix/bug-log-tidak-muncul
```

### 5. Buat Pull Request
- Buka GitHub → repository fork kamu → "Compare & pull request"
- Isi deskripsi: apa yang diubah dan mengapa

---

## Melaporkan Bug

Buka [Issues](../../issues) dan gunakan template ini:

```
**Versi:** 1.0.0
**Browser:** Chrome 120 / Safari 17 / Firefox 121
**Device:** Android 14 / iPhone 15 / Desktop

**Langkah Reproduksi:**
1. Buka aplikasi
2. Klik tab "Catat"
3. Input 5 halaman → klik Tambah
4. Log tidak muncul di "Log Hari Ini"

**Yang Diharapkan:**
Log langsung muncul setelah input.

**Yang Terjadi:**
Log tidak muncul, harus pindah tab dulu.

**Screenshot / Error Console:**
[lampirkan jika ada]
```

---

## Mengusulkan Fitur

Buka [Issues](../../issues) dengan label `enhancement` dan jelaskan:
- **Masalah yang dipecahkan:** apa yang sulit dilakukan sekarang?
- **Solusi yang diusulkan:** bagaimana fitur ini akan bekerja?
- **Alternatif yang dipertimbangkan:** ada solusi lain?

---

## Panduan Kode

Karena ini aplikasi single-file HTML, ikuti prinsip berikut:

### ✅ DO — Yang Harus Dilakukan

```javascript
// 1. Selalu gunakan helper null-safe untuk akses DOM
function el(id) { return document.getElementById(id); }
function setText(id, val) { const e = el(id); if (e) e.textContent = val; }
function setHTML(id, val) { const e = el(id); if (e) e.innerHTML = val; }

// 2. Gunakan _activeInputDate, BUKAN DOM logDate.value
const dateStr = getActiveInputDate(); // ✅
const dateStr = document.getElementById('logDate').value; // ❌

// 3. Semua UI update melalui updateAllUI()
function doAddLog(...) {
  // ... simpan data ...
  save();
  updateAllUI(); // ✅ selalu panggil ini
}

// 4. Deklarasikan variabel sebelum dipakai
const pastRam = projDate > ramEndDate; // ✅ deklarasikan dulu
if (!pastRam) { ... }                  // ✅ baru pakai

// 5. Null-check untuk elemen opsional
const saveBtn = document.querySelector('#inputSheet .btn-primary');
if (saveBtn) { saveBtn.disabled = true; } // ✅
```

### ❌ DON'T — Yang Harus Dihindari

```javascript
// Jangan akses DOM langsung tanpa null check
document.getElementById('inputTodayTotal').textContent = val; // ❌ bisa null!

// Jangan gunakan DOM sebagai state
const date = document.getElementById('logDate').value; // ❌ bisa kosong/stale

// Jangan tambahkan library eksternal
import React from 'react'; // ❌ bukan proyek ini
<script src="jquery.min.js"> // ❌

// Jangan gunakan setInterval untuk update UI
setInterval(() => updateAllUI(), 1000); // ❌ pemborosan resource

// Jangan akumulasikan quick input
el.value += n; // ❌ harus SET, bukan tambah
el.value = n;  // ✅
```

### Prinsip Desain

1. **Single File** — semua tetap dalam 1 file `index.html`
2. **Offline First** — tidak boleh ada network request saat runtime (kecuali Google Fonts di load awal)
3. **Null Safe** — setiap akses DOM harus toleran terhadap null
4. **No Framework** — vanilla JS murni, tidak ada library
5. **Bahasa Indonesia** — semua teks UI dalam Bahasa Indonesia yang ramah dan memotivasi
6. **Mobile First** — desain untuk layar kecil, baru ke desktop

### Struktur CSS

Gunakan CSS variables yang sudah ada:
```css
/* Warna utama */
var(--gold), var(--gold2)  /* aksen */
var(--green)               /* sukses / ahead */
var(--blue)                /* on-track */
var(--amber)               /* peringatan / behind */
var(--red)                 /* hanya untuk tombol hapus */

/* Surface */
var(--surface), var(--surface2), var(--surface3)

/* Teks */
var(--text), var(--text2), var(--text3)
```

---

## Konvensi Commit

Gunakan format [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <deskripsi singkat dalam Bahasa Indonesia>

[body opsional — penjelasan lebih detail]
[footer opsional — closes #issue]
```

**Types:**
| Type | Digunakan untuk |
|------|----------------|
| `feat` | Fitur baru |
| `fix` | Perbaikan bug |
| `docs` | Perubahan dokumentasi |
| `style` | Perubahan CSS/visual (bukan logika) |
| `refactor` | Refaktor kode tanpa mengubah perilaku |
| `perf` | Peningkatan performa |
| `test` | Penambahan test (jika ada) |
| `chore` | Perubahan konfigurasi, tooling |

**Contoh:**
```bash
git commit -m "feat: tambah export data ke CSV"
git commit -m "fix: log tidak muncul setelah input dari FAB sheet"
git commit -m "docs: update README dengan screenshot terbaru"
git commit -m "style: perbaiki tampilan prayer breakdown di mobile kecil"
git commit -m "perf: canvas share hanya re-render saat data berubah"
```

---

## Proses Pull Request

### Checklist sebelum PR:

- [ ] Kode berjalan di Chrome (mobile & desktop)
- [ ] Kode berjalan di Safari (iPhone jika memungkinkan)
- [ ] Tidak ada error di browser console
- [ ] Fitur/fix tidak merusak fungsi yang sudah ada
- [ ] `index.html` masih single file (tidak ada file JS/CSS terpisah)
- [ ] Deskripsi PR menjelaskan: apa yang diubah + mengapa

### Yang akan dicek reviewer:

1. Apakah masih null-safe?
2. Apakah ada potensi bug baru?
3. Apakah UX tetap intuitif untuk pengguna awam?
4. Apakah kode masih efisien (tidak ada loop tak perlu)?

---

## 💬 Pertanyaan?

Buka [Discussions](../../discussions) atau tambahkan komentar di Issue yang relevan.

Jazakallahu khairan atas kontribusinya! 🌙
